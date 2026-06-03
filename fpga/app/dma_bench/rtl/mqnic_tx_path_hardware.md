# Corundum/mqnic 发包（TX）从 skb 到硬件取包发送：硬件逻辑详解

本文以本仓库的 mqnic 驱动与通用 RTL 为主线，尽可能把“Linux 生成 skb → mqnic 驱动写描述符/敲 doorbell → FPGA 侧发现队列非空 → 取描述符 → DMA 取包数据 → 从 MAC 口发出”的完整链路串起来。重点放在“硬件如何理解 doorbell 与 prod_ptr，并最终把包数据从主机内存搬到线侧”。

---

## 1. 一句话总览（数据与控制分离）

TX 路径可以分成两条并行但耦合的通道：

- **控制/队列通道（doorbell 与指针）**：驱动更新 TX ring 的 `prod_ptr` 并写寄存器（doorbell），硬件侧 `queue_manager` 记录新 `prod_ptr` 并发出 doorbell 事件，`tx_scheduler_rr` 看到 doorbell 后把对应队列加入调度队列，开始驱动后续“取描述符/取数据”的流程。
- **数据通道（descriptor 与 payload DMA）**：硬件先 DMA 读**描述符**（descriptor block）得到 packet 的 DMA 地址/长度等，再 DMA 读**payload**把包数据搬到 FPGA 内部 TX RAM，然后以 AXI-Stream 形式送到 egress/port/MAC，最终出线。

---

## 2. 软件侧：从 skb 到“敲 doorbell”

### 2.1 Linux 把 skb 交给 mqnic 的入口

Linux 协议栈最终会调用网卡驱动的 `ndo_start_xmit`。mqnic 在 `net_device_ops` 里把它绑定到 `mqnic_start_xmit()`：

```c
static const struct net_device_ops mqnic_netdev_ops = {
    .ndo_open       = mqnic_open,
    .ndo_stop       = mqnic_close,
    .ndo_start_xmit = mqnic_start_xmit,
    /* ... */
};
```

### 2.2 mqnic_start_xmit 做了什么（与硬件可见的关键点）

`mqnic_start_xmit()` 的核心动作（只列与硬件交互相关的）：

1) 选择 TX queue/ring：`ring_index = skb_get_queue_mapping(skb)`，然后从 `priv->txq_table` 找到 `struct mqnic_ring *ring`。

2) 把 skb 映射成 DMA 地址并写入描述符 ring（host 内存中的队列）：`mqnic_map_skb()` 会对 skb head 和每个 frag 调用 DMA map，然后把 `len/addr` 写入 `struct mqnic_desc` 数组（也就是 TX descriptor ring）。

```c
static bool mqnic_map_skb(struct mqnic_ring *ring, struct mqnic_tx_info *tx_info,
        struct mqnic_desc *tx_desc, struct sk_buff *skb)
{
    struct skb_shared_info *shinfo = skb_shinfo(skb);
    const skb_frag_t *frag;
    u32 i;
    u32 len;
    dma_addr_t dma_addr;

    tx_info->skb = skb;
    tx_info->frag_count = 0;

    for (i = 0; i < shinfo->nr_frags; i++) {
        frag = &shinfo->frags[i];
        len = skb_frag_size(frag);
        dma_addr = skb_frag_dma_map(ring->dev, frag, 0, len, DMA_TO_DEVICE);
        if (unlikely(dma_mapping_error(ring->dev, dma_addr)))
            goto map_error;

        tx_desc[i + 1].len = cpu_to_le32(len);
        tx_desc[i + 1].addr = cpu_to_le64(dma_addr);

        tx_info->frag_count = i + 1;
        tx_info->frags[i].len = len;
        tx_info->frags[i].dma_addr = dma_addr;
    }

    len = skb_headlen(skb);
    dma_addr = dma_map_single(ring->dev, skb->data, len, DMA_TO_DEVICE);
    if (unlikely(dma_mapping_error(ring->dev, dma_addr)))
        goto map_error;

    tx_desc[0].len = cpu_to_le32(len);
    tx_desc[0].addr = cpu_to_le64(dma_addr);

    dma_unmap_addr_set(tx_info, dma_addr, dma_addr);
    dma_unmap_len_set(tx_info, len, len);

    return true;

map_error:
    /* ...unmap... */
    tx_info->skb = NULL;
    tx_info->frag_count = 0;
    return false;
}
```

描述符格式在驱动侧与硬件侧一致，大小 16 字节：

```c
struct mqnic_desc {
    __le16 rsvd0;
    union {
        struct {
            __le16 csum_cmd;
        } tx;
        struct {
            __le16 rsvd0;
        } rx;
    };
    __le32 len;
    __le64 addr;
};
```

3) 更新软件维护的 `ring->prod_ptr++`，表示已经在 ring 中放入了一个新的描述符块：

```c
/* enqueue */
ring->prod_ptr++;
```

4) **在需要时**写 MMIO 寄存器把 `prod_ptr` 通知硬件（也就是“敲 doorbell”）：

```c
static inline void mqnic_tx_write_prod_ptr(struct mqnic_ring *ring)
{
    iowrite32(MQNIC_QUEUE_CMD_SET_PROD_PTR | (ring->prod_ptr & MQNIC_QUEUE_PTR_MASK),
            ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);
}
```

它写的是 `MQNIC_QUEUE_CMD_SET_PROD_PTR | prod_ptr` 到 `MQNIC_QUEUE_CTRL_STATUS_REG`（该寄存器在硬件侧由 `queue_manager` 解码处理，见后文）。

### 2.3 “什么时候敲 doorbell？”

驱动并不是每写一个描述符就一定立刻 doorbell，而是做了合并（减少 MMIO 写次数）：

- Linux >= 5.2：`if (!netdev_xmit_more() || stop_queue) mqnic_tx_write_prod_ptr(ring);`
- 老内核：`if (!skb->xmit_more || stop_queue) ...`

对应代码摘要：

```c
stop_queue = mqnic_is_tx_ring_full(ring);
if (unlikely(stop_queue))
    netif_tx_stop_queue(ring->tx_queue);

if (unlikely(!netdev_xmit_more() || stop_queue)) {
    dma_wmb();
    mqnic_tx_write_prod_ptr(ring);
}
```

含义：

- `netdev_xmit_more()/skb->xmit_more` 表示“后面很可能马上还有包要发”，驱动会先把多个 skb 的描述符批量写进 ring，等批结束再 doorbell 一次。
- 但如果 ring 变满导致 `stop_queue`，必须立即 doorbell（并且停 TX 队列），否则硬件可能收不到最新 `prod_ptr` 从而无法尽快回收/继续发送。

### 2.4 队列寄存器基址与“写到哪儿”

驱动写 doorbell 的地址来自：

- `ring->hw_addr = mqnic_res_get_addr(interface->txq_res, ring->index);`

`mqnic_res_get_addr()` 实际就是 `res->base + index*stride`，它返回 `u8 __iomem *` 的 MMIO 指针：

```c
u8 __iomem *mqnic_res_get_addr(struct mqnic_res *res, int index)
{
    if (index < 0 || index >= res->count)
        return NULL;
    return res->base + index * res->stride;
}
```

`res->base` 最终来自 PCIe BAR0 映射（`pci_ioremap_bar(pdev, 0)`）再加上 interface/资源块偏移：

```c
/* BAR0 映射成 MMIO 基址 */
mqnic->hw_addr = pci_ioremap_bar(pdev, 0);

/* 每个 interface 在 BAR0 内按 if_stride 切分 */
interface = mqnic_create_interface(mqnic, k, mqnic->hw_addr + k * mqnic->if_stride);

/* 在 interface 内按寄存器块给出的 offset/stride 构造资源数组 */
offset = ioread32(interface->txq_rb->regs + MQNIC_RB_TX_QM_REG_OFFSET);
count  = ioread32(interface->txq_rb->regs + MQNIC_RB_TX_QM_REG_COUNT);
stride = ioread32(interface->txq_rb->regs + MQNIC_RB_TX_QM_REG_STRIDE);
interface->txq_res = mqnic_create_res(count, hw_addr + offset, stride);
```

---

## 3. 硬件侧：doorbell 写入后，哪个模块响应？

硬件里，软件“写 prod_ptr（8080xxxx 命令）”会走 AXI-Lite 写通道进入 **`queue_manager`**，它是 TX/RX queue 的队列管理与 dequeue 地址计算模块。

`mqnic_interface` 内部会实例化一个 TX queue manager（`tx_qm_inst`），它提供 dequeue 接口与 doorbell 输出（下文会展示门铃信号是如何接到调度器的）。

### 3.1 queue_manager 如何识别“写 prod_ptr”

`queue_manager` 在 AXI-Lite 写逻辑里解码写入数据的高位命令字段：

- `32'h8080zzzz`：set producer pointer（写 prod_ptr）  
  代码摘要：

```verilog
32'h8080zzzz: begin
    // set producer pointer
    queue_ram_write_data[15:0] = write_data_pipeline_reg[PIPELINE-1][15:0];
    queue_ram_be[1:0] = 2'b11;

    // generate doorbell on queue producer pointer update
    m_axis_doorbell_queue_next = queue_ram_addr_pipeline_reg[PIPELINE-1];
    if (queue_ram_read_data_enable) begin
        m_axis_doorbell_valid_next = 1'b1;
    end
end
```

它会：

1) 把 `prod_ptr` 写入该 queue 的内部 RAM（queue state RAM）
2) **如果该队列处于 enable 状态**，就拉高 `m_axis_doorbell_valid`，并输出 `m_axis_doorbell_queue = queue_index`

这就是硬件对“doorbell 写 prod_ptr”的第一反应：把一次寄存器写转换成“队列 doorbell 事件流”。

### 3.2 doorbell 事件往哪里去：tx_scheduler_rr

`tx_qm_inst` 的 doorbell 输出在 `mqnic_interface.v` 里接到 `mqnic_tx_scheduler_block`（内部是 `tx_scheduler_rr`）：

```verilog
// queue_manager -> tx_doorbell_{queue,valid}
.m_axis_doorbell_queue(tx_doorbell_queue),
.m_axis_doorbell_valid(tx_doorbell_valid),

// tx_doorbell_{queue,valid} -> tx_scheduler_rr
.s_axis_doorbell_queue(tx_doorbell_queue),
.s_axis_doorbell_valid(tx_doorbell_valid),
```

`tx_scheduler_rr` 对 doorbell 的反应逻辑：

1) doorbell 先进入 `doorbell_fifo` 缓冲（AXI-stream FIFO）  
   代码摘要：

```verilog
axis_fifo #(
    .DEPTH(256),
    .DATA_WIDTH(QUEUE_INDEX_WIDTH)
)
doorbell_fifo (
    .clk(clk),
    .rst(rst),
    .s_axis_tdata(s_axis_doorbell_queue),
    .s_axis_tvalid(s_axis_doorbell_valid),
    .m_axis_tdata(axis_doorbell_fifo_queue),
    .m_axis_tvalid(axis_doorbell_fifo_valid),
    .m_axis_tready(axis_doorbell_fifo_ready)
);
```

2) 调度器取出一个 doorbell（得到 queue index），读取对应队列状态，然后进入 “handle doorbell” 操作：  
   代码摘要（取 doorbell 并进入 doorbell 流水线）：

```verilog
end else if (axis_doorbell_fifo_valid) begin
    // handle doorbell
    op_doorbell_pipe_next[0] = 1'b1;
    axis_doorbell_fifo_ready = 1'b1;

    queue_ram_rd_addr = axis_doorbell_fifo_queue;
    queue_ram_addr_pipeline_next[0] = axis_doorbell_fifo_queue;
end
```

代码摘要（doorbell 的核心反应：标记 active，必要时加入 RR 调度 FIFO）：

```verilog
end else if (op_doorbell_pipe_reg[PIPELINE-1]) begin
    // mark queue active
    queue_ram_wr_addr = queue_ram_addr_pipeline_reg[PIPELINE-1];
    queue_ram_wr_data[6] = 1'b1;                 // queue active
    queue_ram_wr_data[15:8] = queue_ram_rd_data_gen_id+1;
    queue_ram_wr_en = 1'b1;

    // schedule queue if necessary
    if (queue_ram_rd_data_enabled && !queue_ram_rd_data_paused && !queue_ram_rd_data_scheduled) begin
        queue_ram_wr_data[7] = 1'b1;             // queue scheduled
        axis_scheduler_fifo_in_queue = queue_ram_addr_pipeline_reg[PIPELINE-1];
        axis_scheduler_fifo_in_valid = 1'b1;
        active_queue_count_next = active_queue_count_reg + 1;
    end
end
```

3) “handle doorbell” 会：

- **把队列标记为 active**（表示该队列有工作要做/可能非空）：`queue_ram_wr_data[6] = 1'b1`
- 如果队列 `enabled && !paused && !scheduled`，则把该队列推入 RR 调度 FIFO（`rr_fifo`）并标记为 scheduled

一句话：doorbell 让调度器“知道某个 TX queue 可能有新描述符了”，并触发把这个 queue 加入调度循环。

---

## 4. 硬件侧：从“队列被调度”到“取到描述符”

### 4.1 scheduler 发起 transmit request

一旦队列在 RR FIFO 中，且满足内部流控与 ready 条件，`tx_scheduler_rr` 会发出一个 transmit request：

代码摘要（发起 transmit request 的触发点）：

```verilog
end else if (enable && enable_reg && !active_op_count_reg[CL_OP_COUNT]
        && axis_scheduler_fifo_out_valid && ch_fetch_fc_av_reg && m_axis_tx_req_ready_int) begin
    // transmit request
    op_req_pipe_next[0] = 1'b1;
    axis_scheduler_fifo_out_ready = 1'b1;
    queue_ram_rd_addr = axis_scheduler_fifo_out_queue;
    queue_ram_addr_pipeline_next[0] = axis_scheduler_fifo_out_queue;
end
```

这会最终产生 `m_axis_tx_req_*` 输出（queue index、tag、dest 等），送入 `mqnic_interface_tx` 里的 `tx_engine`。

### 4.2 tx_engine 发起 descriptor request（desc_req）

`mqnic_interface_tx` 实例化了 `tx_engine`：

```verilog
tx_engine #(
    .QUEUE_INDEX_WIDTH(TX_QUEUE_INDEX_WIDTH),
    .QUEUE_PTR_WIDTH(QUEUE_PTR_WIDTH),
    .CQN_WIDTH(CQN_WIDTH),
    .DESC_TABLE_SIZE(TX_DESC_TABLE_SIZE),
    .MAX_TX_SIZE(MAX_TX_SIZE),
    .DESC_SIZE(DESC_SIZE),
    .CPL_SIZE(CPL_SIZE)
)
tx_engine_inst (
    .s_axis_tx_req_queue(s_axis_tx_req_queue),
    .s_axis_tx_req_tag(s_axis_tx_req_tag),
    .s_axis_tx_req_dest(s_axis_tx_req_dest),
    .s_axis_tx_req_valid(s_axis_tx_req_valid),
    .s_axis_tx_req_ready(s_axis_tx_req_ready),

    .m_axis_desc_req_queue(m_axis_desc_req_queue),
    .m_axis_desc_req_tag(m_axis_desc_req_tag),
    .m_axis_desc_req_valid(m_axis_desc_req_valid),
    .m_axis_desc_req_ready(m_axis_desc_req_ready),

    .s_axis_desc_req_status_queue(s_axis_desc_req_status_queue),
    .s_axis_desc_req_status_ptr(s_axis_desc_req_status_ptr),
    .s_axis_desc_req_status_cpl(s_axis_desc_req_status_cpl),
    .s_axis_desc_req_status_tag(s_axis_desc_req_status_tag),
    .s_axis_desc_req_status_empty(s_axis_desc_req_status_empty),
    .s_axis_desc_req_status_error(s_axis_desc_req_status_error),
    .s_axis_desc_req_status_valid(s_axis_desc_req_status_valid),

    .s_axis_desc_tdata(tx_fifo_desc_tdata),
    .s_axis_desc_tvalid(tx_fifo_desc_tvalid),
    .s_axis_desc_tready(tx_fifo_desc_tready)
);
```

`tx_engine` 接收 scheduler 的 `s_axis_tx_req_*` 后，会对“描述符获取模块”发起 `m_axis_desc_req_*`（本质是：我要从 queue X dequeue 一块 descriptor block）：

```verilog
output wire [QUEUE_INDEX_WIDTH-1:0]  m_axis_desc_req_queue,
output wire [DESC_REQ_TAG_WIDTH-1:0] m_axis_desc_req_tag,
output wire                          m_axis_desc_req_valid,
input  wire                          m_axis_desc_req_ready,

input  wire [QUEUE_INDEX_WIDTH-1:0]  s_axis_desc_req_status_queue,
input  wire [QUEUE_PTR_WIDTH-1:0]    s_axis_desc_req_status_ptr,
input  wire [CQN_WIDTH-1:0]          s_axis_desc_req_status_cpl,
input  wire [DESC_REQ_TAG_WIDTH-1:0] s_axis_desc_req_status_tag,
input  wire                          s_axis_desc_req_status_empty,
input  wire                          s_axis_desc_req_status_error,
input  wire                          s_axis_desc_req_status_valid,
```

### 4.3 desc_op_mux + desc_fetch：真正的“dequeue 并 DMA 读描述符”

在 `mqnic_interface.v` 里，TX 与 RX 的 descriptor request 会被 `desc_op_mux` 仲裁合并，然后进入 `desc_fetch`：

代码摘要（desc_fetch 的输入输出形态：收到 desc_req，向 queue_manager dequeue，再发 DMA 读 descriptor block）：

```verilog
// desc_fetch 输入：来自 tx_engine/rx_engine 的 desc_req（经 mux 后）
input  wire [SELECT_WIDTH-1:0]      s_axis_req_sel,
input  wire [QUEUE_INDEX_WIDTH-1:0] s_axis_req_queue,
input  wire [REQ_TAG_WIDTH-1:0]     s_axis_req_tag,
input  wire                         s_axis_req_valid,
output wire                         s_axis_req_ready,

// 与 queue_manager 的 dequeue 接口
output wire [PORTS*QUEUE_INDEX_WIDTH-1:0] m_axis_desc_dequeue_req_queue,
output wire [PORTS*REQ_TAG_WIDTH-1:0]     m_axis_desc_dequeue_req_tag,
output wire [PORTS-1:0]                   m_axis_desc_dequeue_req_valid,
input  wire [PORTS-1:0]                   m_axis_desc_dequeue_req_ready,

input  wire [PORTS*DMA_ADDR_WIDTH-1:0]    s_axis_desc_dequeue_resp_addr,
input  wire [PORTS*LOG_BLOCK_SIZE_WIDTH-1:0] s_axis_desc_dequeue_resp_block_size,
input  wire [PORTS-1:0]                   s_axis_desc_dequeue_resp_valid,
output wire [PORTS-1:0]                   s_axis_desc_dequeue_resp_ready,

// 发起 DMA 读 descriptor block（写入内部 RAM）
output wire [DMA_ADDR_WIDTH-1:0]          m_axis_dma_read_desc_dma_addr,
output wire [RAM_ADDR_WIDTH-1:0]          m_axis_dma_read_desc_ram_addr,
output wire [DMA_LEN_WIDTH-1:0]           m_axis_dma_read_desc_len,
output wire [DMA_TAG_WIDTH-1:0]           m_axis_dma_read_desc_tag,
output wire                               m_axis_dma_read_desc_valid,
input  wire                               m_axis_dma_read_desc_ready,
```

`desc_fetch` 的职责可以理解为“三段式”：

1) **向 queue_manager 发出 dequeue 请求**（拿到当前 cons_ptr 对应的 descriptor block 地址、block_size、cpl queue 等）  
   - 上面的接口摘要中已经包含 dequeue request/response 形态

2) **把 dequeue 返回的 host 地址转为 DMA read 请求**：输出 `m_axis_dma_read_desc_*`（注意这里的 “desc” 指 DMA engine 的 read descriptor，不是 mqnic 的 tx descriptor）  
   - 上面的接口摘要中已经包含 DMA read descriptor 输出

3) **DMA engine 把 host 内存中的 descriptor block 搬到 FPGA 的内部 RAM**（通过 `dma_ram_wr_cmd_*` 写端口），`desc_fetch` 再把这段 RAM 里的数据以 AXI-stream 形式吐给上层（`m_axis_desc_tdata/...`）  
   - `m_axis_desc_tdata` 就是 `tx_engine` 的 `s_axis_desc_tdata` 输入。

### 4.4 queue_manager 如何把队列指针变成“descriptor 的物理地址”

当 `desc_fetch` 对某个队列发起 dequeue 请求时，`queue_manager` 需要计算“当前要取的 descriptor block 在 host 内存中的 DMA 地址”。它的核心计算是：

```
desc_addr = base_addr + (cons_ptr_mod_queue_size) << (log2(DESC_SIZE) + log_desc_block_size)
```

对应逻辑在：

```verilog
if (op_req_pipe_reg[PIPELINE-1]) begin
    // request
    m_axis_dequeue_resp_queue_next = queue_ram_addr_pipeline_reg[PIPELINE-1];
    m_axis_dequeue_resp_ptr_next   = queue_ram_read_active_cons_ptr;
    m_axis_dequeue_resp_phase_next = !queue_ram_read_active_cons_ptr[queue_ram_read_data_log_queue_size];

    m_axis_dequeue_resp_addr_next =
        queue_ram_read_data_base_addr +
        ((queue_ram_read_active_cons_ptr
            & ({QUEUE_PTR_WIDTH{1'b1}} >> (QUEUE_PTR_WIDTH - queue_ram_read_data_log_queue_size)))
         << (CL_DESC_SIZE + queue_ram_read_data_log_block_size));

    m_axis_dequeue_resp_block_size_next = queue_ram_read_data_log_block_size;
    m_axis_dequeue_resp_cpl_next        = queue_ram_read_data_cpl_queue;
    /* ... */
end
```

其中：

- `base_addr` 是驱动通过寄存器写入的 ring buffer DMA 基址（`MQNIC_QUEUE_BASE_ADDR_VF_REG`）
- `cons_ptr` 是硬件侧维护/推进的 consumer pointer（dequeue commit 后更新）
- `DESC_SIZE` 在 mqnic 里是 16 字节（与驱动 `MQNIC_DESC_SIZE` 一致）
- `log_desc_block_size` 对应“一个 block 里有多少个 desc”（用于 SG 分段，一个包可能需要多个 desc）

---

## 5. 硬件侧：从“拿到描述符”到“DMA 取包数据并发出”

这一段是“硬件取以太网包”的核心：descriptor 给出 payload 的 DMA 地址与长度，硬件发起 PCIe DMA read 把 payload 搬到 FPGA，然后送入 egress/port/MAC。

### 5.1 tx_engine：解析 mqnic descriptor，生成 payload DMA 读任务

`tx_engine` 接收 `desc_fetch` 输出的 descriptor 数据流（`s_axis_desc_*`），并结合 dequeue 状态（queue、queue_ptr、cpl queue 等）构建一条“要发送的 packet”。

它最终输出两类东西：

1) **payload DMA read descriptor**：`m_axis_dma_read_desc_*`  
   代码摘要（tx_engine 对外输出，连接到 DMA 子系统）：

```verilog
output wire [DMA_ADDR_WIDTH-1:0] m_axis_dma_read_desc_dma_addr,
output wire [RAM_ADDR_WIDTH-1:0] m_axis_dma_read_desc_ram_addr,
output wire [DMA_LEN_WIDTH-1:0]  m_axis_dma_read_desc_len,
output wire [DMA_TAG_WIDTH-1:0]  m_axis_dma_read_desc_tag,
output wire                      m_axis_dma_read_desc_valid,
input  wire                      m_axis_dma_read_desc_ready,
```

2) **发给 dma_client_axis_source 的“从 TX RAM 读出并流式输出”的 read_desc**：`m_axis_tx_desc_*`（这里的 addr 是 TX RAM 地址，不是 host 地址）  
   代码摘要（tx_engine → dma_client_axis_source 的 RAM 读任务）：

```verilog
output wire [RAM_ADDR_WIDTH-1:0]       m_axis_tx_desc_addr,
output wire [DMA_CLIENT_LEN_WIDTH-1:0] m_axis_tx_desc_len,
output wire [DMA_CLIENT_TAG_WIDTH-1:0] m_axis_tx_desc_tag,
output wire [AXIS_TX_ID_WIDTH-1:0]     m_axis_tx_desc_id,
output wire [AXIS_TX_DEST_WIDTH-1:0]   m_axis_tx_desc_dest,
output wire [AXIS_TX_USER_WIDTH-1:0]   m_axis_tx_desc_user,
output wire                            m_axis_tx_desc_valid,
input  wire                            m_axis_tx_desc_ready,
```

直观理解：

- `tx_engine` 负责“理解 mqnic descriptor 并调度 DMA 把包搬进 TX RAM”
- `dma_client_axis_source` 负责“把 TX RAM 里的包读出来，变成 AXI-stream 帧”

### 5.2 DMA 写入 TX RAM：dma_psdpram（写端口来自 DMA engine）

`mqnic_interface_tx` 内部有一块 TX RAM（`dma_psdpram_tx_inst`），它的 **写端口**连接的是 `dma_ram_wr_cmd_*`，也就是 PCIe DMA engine 把 host payload 搬过来时写入的落点：

```verilog
dma_psdpram #(
    .SIZE(TX_RAM_SIZE),
    .SEG_COUNT(RAM_SEG_COUNT),
    .SEG_DATA_WIDTH(RAM_SEG_DATA_WIDTH),
    .SEG_BE_WIDTH(RAM_SEG_BE_WIDTH),
    .SEG_ADDR_WIDTH(RAM_SEG_ADDR_WIDTH)
)
dma_psdpram_tx_inst (
    // Write port (from DMA engine)
    .wr_cmd_be(dma_ram_wr_cmd_be),
    .wr_cmd_addr(dma_ram_wr_cmd_addr),
    .wr_cmd_data(dma_ram_wr_cmd_data),
    .wr_cmd_valid(dma_ram_wr_cmd_valid),
    .wr_cmd_ready(dma_ram_wr_cmd_ready),
    .wr_done(dma_ram_wr_done),
    // Read port (to dma_client_axis_source)
    .rd_cmd_addr(dma_ram_rd_cmd_addr_int),
    .rd_cmd_valid(dma_ram_rd_cmd_valid_int),
    .rd_cmd_ready(dma_ram_rd_cmd_ready_int),
    .rd_resp_data(dma_ram_rd_resp_data_int),
    .rd_resp_valid(dma_ram_rd_resp_valid_int),
    .rd_resp_ready(dma_ram_rd_resp_ready_int)
);
```

因此 payload 的实际搬运链路是：

`tx_engine.m_axis_dma_read_desc_*` →（DMA 子系统/PCIe）→ `dma_ram_wr_cmd_*` 写入 `dma_psdpram_tx_inst`

### 5.3 从 TX RAM 读出并输出：dma_client_axis_source

当 `tx_engine` 发出 `dma_tx_desc_*`（RAM addr、len、tag、id、dest、user）后，`dma_client_axis_source` 会：

- 按 `ram_addr/len` 从 `dma_psdpram_tx_inst` 的 **读端口**拉取数据
- 组装成 AXI-stream 帧（`tx_axis_tdata/tkeep/tlast/...`）
- 并在完成/错误时通过 `m_axis_read_desc_status_*` 反馈给 `tx_engine`

实例与连接见：

```verilog
dma_client_axis_source dma_client_axis_source_inst (
    .s_axis_read_desc_ram_addr(dma_tx_desc_addr),
    .s_axis_read_desc_len(dma_tx_desc_len),
    .s_axis_read_desc_tag(dma_tx_desc_tag),
    .s_axis_read_desc_id(dma_tx_desc_id),
    .s_axis_read_desc_dest(dma_tx_desc_dest),
    .s_axis_read_desc_user(dma_tx_desc_user),
    .s_axis_read_desc_valid(dma_tx_desc_valid),
    .s_axis_read_desc_ready(dma_tx_desc_ready),

    .m_axis_read_data_tdata(tx_axis_tdata_int),
    .m_axis_read_data_tkeep(tx_axis_tkeep_int),
    .m_axis_read_data_tvalid(tx_axis_tvalid_int),
    .m_axis_read_data_tready(tx_axis_tready_int),
    .m_axis_read_data_tlast(tx_axis_tlast_int),
    .m_axis_read_data_tid(tx_axis_tid_int),
    .m_axis_read_data_tdest(tx_axis_tdest_int),
    .m_axis_read_data_tuser(tx_axis_tuser_int),

    .ram_rd_cmd_addr(dma_ram_rd_cmd_addr_int),
    .ram_rd_cmd_valid(dma_ram_rd_cmd_valid_int),
    .ram_rd_cmd_ready(dma_ram_rd_cmd_ready_int),
    .ram_rd_resp_data(dma_ram_rd_resp_data_int),
    .ram_rd_resp_valid(dma_ram_rd_resp_valid_int),
    .ram_rd_resp_ready(dma_ram_rd_resp_ready_int)
);
```

### 5.4 egress：校验和/整形/输出到接口侧 AXI-stream

`mqnic_egress` 接收 `dma_client_axis_source` 输出的 AXI-stream，并根据 `tx_engine` 输出的 checksum command 等信息处理后，输出 `m_axis_tx_*`：

```verilog
mqnic_egress egress_inst (
    .s_axis_tdata(tx_axis_tdata_int),
    .s_axis_tkeep(tx_axis_tkeep_int),
    .s_axis_tvalid(tx_axis_tvalid_int),
    .s_axis_tready(tx_axis_tready_int),
    .s_axis_tlast(tx_axis_tlast_int),
    .s_axis_tid(tx_axis_tid_int),
    .s_axis_tdest(tx_axis_tdest_int),
    .s_axis_tuser(tx_axis_tuser_int),

    .m_axis_tdata(m_axis_tx_tdata),
    .m_axis_tkeep(m_axis_tx_tkeep),
    .m_axis_tvalid(m_axis_tx_tvalid),
    .m_axis_tready(m_axis_tx_tready),
    .m_axis_tlast(m_axis_tx_tlast),
    .m_axis_tid(m_axis_tx_tid),
    .m_axis_tdest(m_axis_tx_tdest),
    .m_axis_tuser(m_axis_tx_tuser)
);
```

到这里，包数据已经变成“接口内部的 TX AXI-stream 帧”（带 tid/tdest/tuser 等元数据）。

---

## 6. 硬件侧：从接口到端口，再到 MAC/PHY（出线）

### 6.1 mqnic_interface 内部的 TX FIFO

`mqnic_interface` 会把 `mqnic_interface_tx` 的输出接入 `tx_fifo`，进行跨端口拆分与缓冲：

```verilog
tx_fifo tx_fifo_inst (
    .s_axis_tdata(axis_if_tx_tdata),
    .s_axis_tkeep(axis_if_tx_tkeep),
    .s_axis_tvalid(axis_if_tx_tvalid),
    .s_axis_tready(axis_if_tx_tready),
    .s_axis_tlast(axis_if_tx_tlast),
    .s_axis_tid(axis_if_tx_tid),
    .s_axis_tdest(axis_if_tx_tdest),
    .s_axis_tuser(axis_if_tx_tuser),

    .m_axis_tdata(axis_if_tx_fifo_tdata),
    .m_axis_tkeep(axis_if_tx_fifo_tkeep),
    .m_axis_tvalid(axis_if_tx_fifo_tvalid),
    .m_axis_tready(axis_if_tx_fifo_tready),
    .m_axis_tlast(axis_if_tx_fifo_tlast),
    .m_axis_tid(axis_if_tx_fifo_tid),
    .m_axis_tuser(axis_if_tx_fifo_tuser)
);
```

输出是每个 port 一路的 `axis_if_tx_fifo_*[n]`。

### 6.2 mqnic_port / mqnic_port_tx：进入端口侧处理与 MAC 接口

每个 port 实例化 `mqnic_port`，其 TX 部分是 `mqnic_port_tx`：

```verilog
mqnic_port_tx port_tx_inst (
    // input from interface FIFO (per port slice)
    .s_axis_if_tx_tdata(s_axis_if_tx_tdata),
    .s_axis_if_tx_tkeep(s_axis_if_tx_tkeep),
    .s_axis_if_tx_tvalid(s_axis_if_tx_tvalid),
    .s_axis_if_tx_tready(s_axis_if_tx_tready),
    .s_axis_if_tx_tlast(s_axis_if_tx_tlast),
    .s_axis_if_tx_tuser(s_axis_if_tx_tuser),

    // output to MAC/PCS side (per-port AXI stream)
    .m_axis_tx_tdata(m_axis_tx_tdata),
    .m_axis_tx_tkeep(m_axis_tx_tkeep),
    .m_axis_tx_tvalid(m_axis_tx_tvalid),
    .m_axis_tx_tready(m_axis_tx_tready),
    .m_axis_tx_tlast(m_axis_tx_tlast),
    .m_axis_tx_tuser(m_axis_tx_tuser)
);
```

`mqnic_port_tx` 的输出 `m_axis_tx_*` 就是“面向 MAC/PHY 的 TX 帧流”（仍是 AXI-stream 形式，具体接到哪个 MAC 模块取决于板卡与工程 top）。

### 6.3 mqnic_core 对外的 TX 口

`mqnic_core` 对外导出：

- `m_axis_tx_tdata/tkeep/tvalid/tready/tlast/tuser`（每个 port 一路）

端口定义见：

```verilog
output wire [PORT_COUNT*AXIS_DATA_WIDTH-1:0]    m_axis_tx_tdata,
output wire [PORT_COUNT*AXIS_KEEP_WIDTH-1:0]    m_axis_tx_tkeep,
output wire [PORT_COUNT-1:0]                    m_axis_tx_tvalid,
input  wire [PORT_COUNT-1:0]                    m_axis_tx_tready,
output wire [PORT_COUNT-1:0]                    m_axis_tx_tlast,
output wire [PORT_COUNT*AXIS_TX_USER_WIDTH-1:0] m_axis_tx_tuser,
```

在你的板卡/工程顶层（不同平台不同目录）通常会把 `mqnic_core.m_axis_tx_*` 连接到对应的 Ethernet MAC/PCS（例如 XGMII/GMII/CMAC 等）或应用 block 的直连接口，最终变成线侧信号出网口。

---

## 7. 关键点回顾：doorbell 写完“硬件到底做了什么”

按时序把最关键的因果链条写成清单：

1) CPU：`mqnic_start_xmit()` 把 skb 映射成 DMA 并写 TX ring（host 内存），`ring->prod_ptr++`  
   代码摘要：

```c
mqnic_map_skb(ring, tx_info, tx_desc, skb);
ring->prod_ptr++;
if (!netdev_xmit_more() || stop_queue) {
    dma_wmb();
    mqnic_tx_write_prod_ptr(ring);
}
```

2) CPU：满足条件时 `mqnic_tx_write_prod_ptr()` MMIO 写 `MQNIC_QUEUE_CMD_SET_PROD_PTR`  
   代码摘要：

```c
iowrite32(MQNIC_QUEUE_CMD_SET_PROD_PTR | (ring->prod_ptr & MQNIC_QUEUE_PTR_MASK),
        ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);
```

3) FPGA：`queue_manager` 解码到 `32'h8080zzzz`，更新内部 `prod_ptr` 并输出 `doorbell_valid`（前提队列 enable）  
   代码摘要：

```verilog
32'h8080zzzz: begin
    queue_ram_write_data[15:0] = write_data_pipeline_reg[PIPELINE-1][15:0];
    queue_ram_be[1:0] = 2'b11;
    m_axis_doorbell_queue_next = queue_ram_addr_pipeline_reg[PIPELINE-1];
    if (queue_ram_read_data_enable) m_axis_doorbell_valid_next = 1'b1;
end
```

4) FPGA：`tx_scheduler_rr` 收到 doorbell，把队列标记 active，并在需要时把队列加入 RR 调度 FIFO  
   代码摘要：

```verilog
queue_ram_wr_data[6] = 1'b1; // active
if (queue_ram_rd_data_enabled && !queue_ram_rd_data_paused && !queue_ram_rd_data_scheduled) begin
    queue_ram_wr_data[7] = 1'b1; // scheduled
    axis_scheduler_fifo_in_valid = 1'b1;
end
```

5) FPGA：调度器在合适时机发出 tx_req（队列号） → `tx_engine`  
   代码摘要：

```verilog
if (axis_scheduler_fifo_out_valid && m_axis_tx_req_ready_int) begin
    op_req_pipe_next[0] = 1'b1;
end
```

6) FPGA：`tx_engine` 发出 desc_req → `desc_fetch` → `queue_manager` dequeue 计算 descriptor 地址 → DMA 读 descriptor block → descriptor stream 返回给 `tx_engine`  
   代码摘要（queue_manager 计算 descriptor block 地址）：

```verilog
m_axis_dequeue_resp_addr_next =
    queue_ram_read_data_base_addr +
    ((queue_ram_read_active_cons_ptr & mask) << (CL_DESC_SIZE + queue_ram_read_data_log_block_size));
```

7) FPGA：`tx_engine` 根据 descriptor 得到 payload 的 host 地址/长度，发起 payload DMA read，把数据写入 TX RAM  
   - `tx_engine_inst` 输出 DMA read：  
     代码摘要：

```verilog
tx_engine_inst: m_axis_dma_read_desc_* -> DMA/PCIe 子系统
DMA/PCIe 子系统: dma_ram_wr_cmd_* -> dma_psdpram_tx_inst (TX RAM 写端口)
```
   - DMA engine 写入 TX RAM：  
     代码摘要（写端口信号名）：

```verilog
dma_ram_wr_cmd_be
dma_ram_wr_cmd_addr
dma_ram_wr_cmd_data
dma_ram_wr_cmd_valid
dma_ram_wr_cmd_ready
dma_ram_wr_done
```

8) FPGA：`dma_client_axis_source` 从 TX RAM 读出 payload 并输出 AXI-stream 帧 → `mqnic_egress` → `tx_fifo` → `mqnic_port_tx` → `mqnic_core.m_axis_tx_*` → 板卡顶层 MAC/PHY → 出线  
   代码摘要（模块级数据通路拼接关系）：

```verilog
tx_engine
  -> dma_psdpram (TX RAM, DMA 写入)
  -> dma_client_axis_source (从 TX RAM 读出，输出 AXI-stream)
  -> mqnic_egress
  -> tx_fifo
  -> mqnic_port_tx
  -> mqnic_core.m_axis_tx_*
```

---

## 8. 建议的阅读顺序（从“最像 doorbell”的地方开始）

如果你想把硬件 TX 逻辑看透，推荐按以下顺序阅读：

1) `queue_manager`：AXI-Lite 配置队列；dequeue 计算 descriptor block 地址；prod_ptr 更新时产生 doorbell 事件
2) `tx_scheduler_rr`：消费 doorbell；标记队列 active；把队列放入 RR 调度队列；按条件发起 tx_req
3) `desc_fetch`：接收 desc_req；调用 queue_manager dequeue；DMA 读 descriptor block；输出 descriptor 数据流
4) `tx_engine`：解析 descriptor；发起 payload DMA read；生成 TX RAM 读任务（给 dma_client_axis_source）
5) `dma_client_axis_source`：从 TX RAM 读出 payload 并输出 AXI-stream 帧
6) `mqnic_egress`：对帧做整形/校验和相关处理后输出到 interface TX
7) `tx_fifo`/`mqnic_port_tx`/`mqnic_core`：跨端口缓冲、端口侧处理，最终导出到 MAC/PHY 侧接口
