# Corundum/mqnic 收包（RX）从驱动配置到硬件把包写入主机内存：硬件逻辑详解

本文以 mqnic Linux 驱动与 FPGA RTL 的 RX 数据通路为主线，尽可能把“驱动配置 RX 队列与缓冲区 → 硬件接收以太网帧 → 选择 RX 队列 → 取 RX 描述符（host 读）→ 把帧暂存到 RX RAM → DMA 写入主机内存 → 写 completion → 驱动从 CQ 取 completion 组 skb 并补充 buffer”的完整链路串起来。所有引用均以 C/Verilog 代码摘要形式给出。

---

## 1. 一句话总览（RX 的关键思想）

RX 的核心是“主机先准备好可写的 buffer，并把 buffer 的 DMA 地址写进 RX 描述符 ring；硬件收帧后从 RX 描述符 ring 取出一个 buffer 地址，把包写到该地址，然后生成 completion 通知驱动消费”。

在本设计里，硬件侧通常采用两段式搬运：

- 先把线侧收到的帧写入 FPGA 内部的 RX RAM（作为弹性缓冲）
- 再由 DMA 子系统把 RX RAM 内容写回主机内存（DMA write）

---

## 2. 软件侧：驱动如何“设置接收队列”和“提供可写 buffer”

### 2.1 启动端口时创建并打开 RX 队列

端口打开路径会为每个 RX queue 创建一个 CQ（用于 completion），并创建/打开 RX ring：

```c
int mqnic_start_port(struct net_device *ndev)
{
    /* ... */

    for (k = 0; k < priv->rxq_count; k++) {
        cq = mqnic_create_cq(iface);
        mqnic_open_cq(cq, eq, priv->rx_ring_size);

        netif_napi_add(ndev, &cq->napi, mqnic_poll_rx_cq, NAPI_POLL_WEIGHT);
        napi_enable(&cq->napi);
        mqnic_arm_cq(cq);

        q = mqnic_create_rx_ring(iface);
        q->mtu = ndev->mtu;
        q->page_order = /* 按 MTU 选择页面阶数 */;

        mqnic_open_rx_ring(q, priv, cq, priv->rx_ring_size, 1);
        radix_tree_insert(&priv->rxq_table, k, q);
    }

    /* 配置 MTU、RX 队列映射、开启队列与端口 RX */
    mqnic_interface_set_rx_mtu(iface, ndev->mtu + ETH_HLEN);
    mqnic_update_indir_table(ndev);

    radix_tree_for_each_slot(rxq_table) mqnic_enable_rx_ring(q);
    mqnic_port_set_rx_ctrl(priv->port, MQNIC_PORT_RX_CTRL_EN);
}
```

这里有三个“硬件最终必须知道”的信息：

- RX ring（描述符 ring）的 **DMA 基址**（base address）
- RX ring 的 **大小**（queue size）与 **desc block size**
- RX ring 对应的 **completion queue number（CQN）**，硬件写 completion 时会写入对应 CQ

这些都在 `mqnic_open_rx_ring()` 里通过 MMIO 写寄存器交给硬件。

---

### 2.2 mqnic_open_rx_ring：配置硬件 RX 队列寄存器（base/size/cqn/ptr）

RX ring 的队列内存 `ring->buf` 是一段 DMA coherent 内存，里面放的就是 RX 描述符数组（host 端可写，硬件端可读）：

```c
int mqnic_open_rx_ring(struct mqnic_ring *ring, struct mqnic_priv *priv,
        struct mqnic_cq *cq, int size, int desc_block_size)
{
    ring->index = mqnic_res_alloc(ring->interface->rxq_res);
    ring->log_desc_block_size = desc_block_size < 2 ? 0 : ilog2(desc_block_size - 1) + 1;
    ring->desc_block_size = 1 << ring->log_desc_block_size;

    ring->size = roundup_pow_of_two(size);
    ring->size_mask = ring->size - 1;
    ring->stride = roundup_pow_of_two(MQNIC_DESC_SIZE * ring->desc_block_size);

    ring->buf_size = ring->size * ring->stride;
    ring->buf = dma_alloc_coherent(ring->dev, ring->buf_size, &ring->buf_dma_addr, GFP_KERNEL);

    ring->cq = cq;
    cq->src_ring = ring;
    cq->handler = mqnic_rx_irq;

    ring->hw_addr = mqnic_res_get_addr(ring->interface->rxq_res, ring->index);

    ring->prod_ptr = 0;
    ring->cons_ptr = 0;

    iowrite32(MQNIC_QUEUE_CMD_SET_ENABLE | 0,
            ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);

    iowrite32((ring->buf_dma_addr & 0xfffff000),
            ring->hw_addr + MQNIC_QUEUE_BASE_ADDR_VF_REG + 0);
    iowrite32(ring->buf_dma_addr >> 32,
            ring->hw_addr + MQNIC_QUEUE_BASE_ADDR_VF_REG + 4);

    iowrite32(MQNIC_QUEUE_CMD_SET_SIZE | ilog2(ring->size) | (ring->log_desc_block_size << 8),
            ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);

    iowrite32(MQNIC_QUEUE_CMD_SET_CQN | ring->cq->cqn,
            ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);

    iowrite32(MQNIC_QUEUE_CMD_SET_PROD_PTR | (ring->prod_ptr & MQNIC_QUEUE_PTR_MASK),
            ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);
    iowrite32(MQNIC_QUEUE_CMD_SET_CONS_PTR | (ring->cons_ptr & MQNIC_QUEUE_PTR_MASK),
            ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);

    return mqnic_refill_rx_buffers(ring);
}
```

直观理解：

- `BASE_ADDR` 指向“RX 描述符 ring 在主机内存中的起始 DMA 地址”
- `SIZE` 告诉硬件 ring 有多少个 entry（2^log2(size)）
- `CQN` 告诉硬件收到包后 completion 应写入哪个 CQ
- `PROD_PTR`/`CONS_PTR` 初始化为 0

---

### 2.3 驱动如何往 RX ring 里放“可写 buffer”（RX 描述符）

RX 描述符与 TX 描述符复用同一个结构体（关键字段是 `len` 与 `addr`）：

```c
struct mqnic_desc {
    __le16 rsvd0;
    union {
        struct { __le16 csum_cmd; } tx;
        struct { __le16 rsvd0;    } rx;
    };
    __le32 len;
    __le64 addr;
};
```

对 RX 来说：

- `addr`：主机 buffer 的 DMA 地址（DMA_FROM_DEVICE）
- `len`：buffer 长度（硬件写入最多不超过该长度）

驱动为每个 RX entry 分配 page，并 DMA map，然后把 DMA 地址写入 RX 描述符 ring：

```c
int mqnic_prepare_rx_desc(struct mqnic_ring *ring, int index)
{
    struct mqnic_rx_info *rx_info = &ring->rx_info[index];
    struct mqnic_desc *rx_desc = (struct mqnic_desc *)(ring->buf + index * ring->stride);
    u32 len = PAGE_SIZE << ring->page_order;
    dma_addr_t dma_addr;
    struct page *page;

    page = dev_alloc_pages(ring->page_order);
    dma_addr = dma_map_page(ring->dev, page, 0, len, DMA_FROM_DEVICE);

    rx_desc->len  = cpu_to_le32(len);
    rx_desc->addr = cpu_to_le64(dma_addr);

    rx_info->page = page;
    rx_info->page_order = ring->page_order;
    rx_info->page_offset = 0;
    rx_info->dma_addr = dma_addr;
    rx_info->len = len;
    return 0;
}
```

当补充了足够多的 entry 后，驱动推进 `prod_ptr` 并写寄存器通知硬件（doorbell）：

```c
void mqnic_rx_write_prod_ptr(struct mqnic_ring *ring)
{
    iowrite32(MQNIC_QUEUE_CMD_SET_PROD_PTR | (ring->prod_ptr & MQNIC_QUEUE_PTR_MASK),
            ring->hw_addr + MQNIC_QUEUE_CTRL_STATUS_REG);
}

int mqnic_refill_rx_buffers(struct mqnic_ring *ring)
{
    u32 missing = ring->size - (ring->prod_ptr - ring->cons_ptr);

    if (missing < 8)
        return 0;

    while (missing-- > 0) {
        mqnic_prepare_rx_desc(ring, ring->prod_ptr & ring->size_mask);
        ring->prod_ptr++;
    }

    dma_wmb();
    mqnic_rx_write_prod_ptr(ring);
    return 0;
}
```

这个“写 prod_ptr”对 RX 来说等价于告诉硬件：

- “RX 描述符 ring 中从旧 prod_ptr 到新 prod_ptr 的这些 entry 已经准备好了（里面的 addr/len 可以被硬件使用）”

---

### 2.4 驱动如何从 completion queue 取回 RX 包（便于理解硬件 completion 的意义）

硬件写完包并生成 completion 后，驱动在 NAPI 里处理 CQ：

```c
int mqnic_process_rx_cq(struct mqnic_cq *cq, int budget)
{
    struct mqnic_ring *rx_ring = cq->src_ring;
    u32 cq_cons_ptr = cq->cons_ptr;

    while (done < budget) {
        struct mqnic_cpl *cpl = (struct mqnic_cpl *)(cq->buf + (cq_cons_ptr & cq->size_mask) * cq->stride);

        if (!!(cpl->phase & cpu_to_le32(0x80000000)) == !!(cq_cons_ptr & cq->size))
            break;

        dma_rmb();

        ring_index = le16_to_cpu(cpl->index) & rx_ring->size_mask;
        rx_info = &rx_ring->rx_info[ring_index];
        page = rx_info->page;
        len = min_t(u32, le16_to_cpu(cpl->len), rx_info->len);

        skb = napi_get_frags(&cq->napi);
        dma_unmap_page(dev, dma_unmap_addr(rx_info, dma_addr), dma_unmap_len(rx_info, len), DMA_FROM_DEVICE);
        __skb_fill_page_desc(skb, 0, page, rx_info->page_offset, len);
        rx_info->page = NULL;
        napi_gro_frags(&cq->napi);

        cq_cons_ptr++;
    }

    cq->cons_ptr = cq_cons_ptr;
    mqnic_cq_write_cons_ptr(cq);

    /* 释放已消费 entry 并补充新 buffer */
    /* ...推进 rx_ring->cons_ptr... */
    mqnic_refill_rx_buffers(rx_ring);
}
```

这段逻辑说明了 completion 里最关键的字段：

- `cpl->index`：对应 RX ring 的哪个 entry（驱动用它找到那一页 page）
- `cpl->len`：实际收包长度

硬件侧必须保证 completion 的 `index/len` 与它写入到 host buffer 的数据一致。

---

## 3. 硬件侧：从线侧帧到“写入主机内存”的 RX 数据通路

### 3.1 接口级 RX 的顶层结构（mqnic_interface_rx）

接口 RX 路径可以概括为四块：

1) `mqnic_ingress`：对输入帧做 hash/checksum 等 sideband 生成，并输出带 metadata 的 AXI-stream
2) `dma_client_axis_sink`：把 AXI-stream 帧写入 FPGA 内部 RX RAM（形成一个“帧缓冲区”）
3) `rx_engine`：为每个接收帧分配一个 host buffer（通过取 RX 描述符得到 DMA 地址），并发起 DMA write 把 RX RAM 内容写入该 host buffer，同时生成 completion
4) `dma_psdpram`：RX RAM，本地暂存接收帧数据

典型的连接形态（代码摘要）：

```verilog
mqnic_ingress ingress_inst (
    .s_axis_tdata(s_axis_rx_tdata),
    .s_axis_tkeep(s_axis_rx_tkeep),
    .s_axis_tvalid(s_axis_rx_tvalid),
    .s_axis_tready(s_axis_rx_tready),
    .s_axis_tlast(s_axis_rx_tlast),
    .s_axis_tid(s_axis_rx_tid),
    .s_axis_tdest(s_axis_rx_tdest),
    .s_axis_tuser(s_axis_rx_tuser),

    .m_axis_tdata(rx_axis_tdata_int),
    .m_axis_tkeep(rx_axis_tkeep_int),
    .m_axis_tvalid(rx_axis_tvalid_int),
    .m_axis_tready(rx_axis_tready_int),
    .m_axis_tlast(rx_axis_tlast_int),
    .m_axis_tid(rx_axis_tid_int),
    .m_axis_tdest(rx_axis_tdest_int),
    .m_axis_tuser(rx_axis_tuser_int),

    .rx_csum(rx_csum),
    .rx_csum_valid(rx_csum_valid),
    .rx_csum_ready(rx_csum_ready)
);

dma_client_axis_sink dma_client_axis_sink_inst (
    .s_axis_write_desc_ram_addr(dma_rx_desc_addr),
    .s_axis_write_desc_len(dma_rx_desc_len),
    .s_axis_write_desc_tag(dma_rx_desc_tag),
    .s_axis_write_desc_valid(dma_rx_desc_valid),
    .s_axis_write_desc_ready(dma_rx_desc_ready),

    .m_axis_write_desc_status_len(dma_rx_desc_status_len),
    .m_axis_write_desc_status_tag(dma_rx_desc_status_tag),
    .m_axis_write_desc_status_id(dma_rx_desc_status_id),
    .m_axis_write_desc_status_dest(dma_rx_desc_status_dest),
    .m_axis_write_desc_status_user(dma_rx_desc_status_user),
    .m_axis_write_desc_status_valid(dma_rx_desc_status_valid),

    .s_axis_write_data_tdata(rx_axis_tdata_int),
    .s_axis_write_data_tkeep(rx_axis_tkeep_int),
    .s_axis_write_data_tvalid(rx_axis_tvalid_int),
    .s_axis_write_data_tready(rx_axis_tready_int),
    .s_axis_write_data_tlast(rx_axis_tlast_int),
    .s_axis_write_data_tid(rx_axis_tid_int),
    .s_axis_write_data_tdest(rx_axis_tdest_int),
    .s_axis_write_data_tuser(rx_axis_tuser_int),

    .ram_wr_cmd_be(dma_ram_wr_cmd_be_int),
    .ram_wr_cmd_addr(dma_ram_wr_cmd_addr_int),
    .ram_wr_cmd_data(dma_ram_wr_cmd_data_int),
    .ram_wr_cmd_valid(dma_ram_wr_cmd_valid_int),
    .ram_wr_cmd_ready(dma_ram_wr_cmd_ready_int),
    .ram_wr_done(dma_ram_wr_done_int)
);

dma_psdpram dma_psdpram_rx_inst (
    .wr_cmd_be(dma_ram_wr_cmd_be_int),
    .wr_cmd_addr(dma_ram_wr_cmd_addr_int),
    .wr_cmd_data(dma_ram_wr_cmd_data_int),
    .wr_cmd_valid(dma_ram_wr_cmd_valid_int),
    .wr_cmd_ready(dma_ram_wr_cmd_ready_int),
    .wr_done(dma_ram_wr_done_int),

    .rd_cmd_addr(dma_ram_rd_cmd_addr),
    .rd_cmd_valid(dma_ram_rd_cmd_valid),
    .rd_cmd_ready(dma_ram_rd_cmd_ready),
    .rd_resp_data(dma_ram_rd_resp_data),
    .rd_resp_valid(dma_ram_rd_resp_valid),
    .rd_resp_ready(dma_ram_rd_resp_ready)
);
```

上面体现了一个关键点：`rx_engine` 会产生 `dma_rx_desc_*`（写入 RX RAM 的位置/长度/tag），供 `dma_client_axis_sink` 把线侧数据写到 RX RAM；然后 `rx_engine` 再基于 RX RAM 的内容发起 DMA write 写入主机内存。

---

### 3.2 mqnic_ingress：为帧生成 hash/checksum 等 sideband（影响队列选择与 completion）

`mqnic_ingress` 会在 RX 使能 hash 时生成 hash 与 hash_type，并把它们拼接到 `tuser` 中，使得后续模块在处理该帧时可以使用 RSS 信息：

```verilog
assign m_axis_tdata  = fifo_axis_tdata;
assign m_axis_tkeep  = fifo_axis_tkeep;
assign m_axis_tvalid = fifo_axis_tvalid && sideband_valid;
assign m_axis_tlast  = fifo_axis_tlast;
assign m_axis_tid    = fifo_axis_tid;
assign m_axis_tdest  = fifo_axis_tdest;
assign m_axis_tuser  = {rx_fifo_hash_type, rx_fifo_hash, fifo_axis_tuser};
```

这保证了后续 `rx_engine` 能拿到 hash，进而按 indirection table 选择 RX queue。

---

### 3.3 “每来一帧就产生一个 RX request”：rx_req_cnt_reg

在接口 RX 顶层里，有一个简单的“帧计数器”逻辑：每检测到一个新帧开始（`s_axis_rx_tvalid` 且当前不在帧中），就把 `rx_req_cnt_reg` 加一，表示需要 `rx_engine` 处理一个新的接收请求：

```verilog
reg rx_frame_reg = 1'b0;
reg [5:0] rx_req_cnt_reg = 0;

wire rx_req_valid = rx_req_cnt_reg != 0;
wire rx_req_ready;

always @(posedge clk) begin
    if (rx_req_valid && rx_req_ready) begin
        rx_req_cnt_reg <= rx_req_cnt_reg - 1;
    end

    if (s_axis_rx_tvalid) begin
        if (!rx_frame_reg) begin
            if (!(rx_req_valid && rx_req_ready))
                rx_req_cnt_reg <= rx_req_cnt_reg + 1;
            rx_frame_reg <= 1'b1;
        end
        if (s_axis_rx_tready && s_axis_rx_tvalid && s_axis_rx_tlast) begin
            rx_frame_reg <= 1'b0;
        end
    end

    if (rst) begin
        rx_frame_reg <= 1'b0;
        rx_req_cnt_reg <= 0;
    end
end
```

`rx_engine` 接到 `rx_req_valid` 后，会建立一个 in-flight 表项，驱动后续“选队列→取描述符→写数据→写 completion”等步骤。

---

## 4. 硬件侧：如何选择 RX 队列（RSS/indirection table）

RX 队列选择由 `rx_engine` 内部的 `mqnic_rx_queue_map` 完成，它综合以下信息：

- `id`（端口/通道等）
- `dest`（输入帧携带的 tdest）
- `hash/hash_type`（由 ingress 生成）

并通过“RSS mask + APP mask + indirection table”选择最终的 RX queue。

驱动侧会在端口启动时更新 indirection table（示例：把表项映射到实际 RX ring 的硬件 index）：

```c
int mqnic_update_indir_table(struct net_device *ndev)
{
    mqnic_interface_set_rx_queue_map_rss_mask(iface, priv->port->index, 0xffffffff);
    mqnic_interface_set_rx_queue_map_app_mask(iface, priv->port->index, 0);

    for (k = 0; k < priv->rx_queue_map_indir_table_size; k++) {
        q = radix_tree_lookup(&priv->rxq_table, priv->rx_queue_map_indir_table[k]);
        if (q)
            mqnic_interface_set_rx_queue_map_indir_table(iface, priv->port->index, k, q->index);
    }
}
```

硬件侧 `rx_engine` 发起队列映射请求（把 metadata 送入 queue_map），并得到 `resp_queue`：

```verilog
mqnic_rx_queue_map mqnic_rx_queue_map_inst (
    .req_id(s_axis_rx_desc_status_id),
    .req_dest(s_axis_rx_desc_status_dest),
    .req_hash(s_axis_rx_desc_status_user >> TUSER_HASH_OFFSET),
    .req_tag(s_axis_rx_desc_status_tag),
    .req_valid(s_axis_rx_desc_status_valid),

    .resp_queue(queue_map_resp_queue),
    .resp_tag(queue_map_resp_tag),
    .resp_valid(queue_map_resp_valid)
);
```

`resp_queue` 决定了本帧应该从哪个 RX queue（哪个 RX ring）取出一个 host buffer（通过读取 RX 描述符获得 DMA 地址）。

---

## 5. 硬件侧：如何“取 RX 描述符”（host 读）得到 host buffer 的 DMA 地址

### 5.1 rx_engine 发起 desc_req

`rx_engine` 会输出 `m_axis_desc_req_queue/tag/valid`，请求 `desc_fetch` 从指定 RX queue dequeue 一块 descriptor block：

```verilog
output wire [QUEUE_INDEX_WIDTH-1:0]  m_axis_desc_req_queue,
output wire [DESC_REQ_TAG_WIDTH-1:0] m_axis_desc_req_tag,
output wire                          m_axis_desc_req_valid,
input  wire                          m_axis_desc_req_ready,
```

### 5.2 desc_fetch + queue_manager：dequeue 计算 descriptor block 地址并 DMA 读回

`desc_fetch` 的基本模式是：

1) 向 RX queue 的 `queue_manager` 发 dequeue 请求，获得当前 cons_ptr 对应的 descriptor block DMA 地址
2) 发起 DMA read 把该 block 读入内部 RAM
3) 以 AXI-stream 形式把 descriptor 数据流输出给 `rx_engine`

RX 队列的 descriptor block 地址由 queue_manager 按 base_addr 与 cons_ptr 计算（示意）：

```verilog
m_axis_dequeue_resp_addr_next =
    queue_ram_read_data_base_addr +
    ((queue_ram_read_active_cons_ptr & mask) << (CL_DESC_SIZE + queue_ram_read_data_log_block_size));
```

此处的 `base_addr` 正是驱动在 `mqnic_open_rx_ring()` 中通过 `MQNIC_QUEUE_BASE_ADDR_VF_REG` 写入的 `ring->buf_dma_addr`。

### 5.3 rx_engine 如何从 RX 描述符得到 host buffer 地址/长度

RX 描述符中：

- `addr` 是 host buffer DMA 地址（驱动提前填好）
- `len` 是可写长度（驱动提前填好）

`rx_engine` 解析 `s_axis_desc_tdata` 后，会得到“把本帧写到 host 内存的目的地址与长度”，并在后续产生 DMA write 描述符。

---

## 6. 硬件侧：如何把线侧帧写入 host 内存（DMA write）

RX 的“写入主机内存”通常分两步：

### 6.1 先把帧写入 FPGA 内部 RX RAM（dma_client_axis_sink）

`dma_client_axis_sink` 一边接收 ingress 输出的 AXI-stream 帧，一边把它写进 RX RAM。写入完成后，它会输出一个 status，包含：

- 实际写入长度 `len`
- `tag`（对应 in-flight 表项）
- 帧的 `id/dest/user`（用于队列映射与 completion 内容）

接口摘要：

```verilog
output wire [DMA_CLIENT_LEN_WIDTH-1:0]  m_axis_write_desc_status_len,
output wire [DMA_CLIENT_TAG_WIDTH-1:0]  m_axis_write_desc_status_tag,
output wire [AXIS_RX_ID_WIDTH-1:0]      m_axis_write_desc_status_id,
output wire [AXIS_RX_DEST_WIDTH-1:0]    m_axis_write_desc_status_dest,
output wire [AXIS_RX_USER_WIDTH-1:0]    m_axis_write_desc_status_user,
output wire                             m_axis_write_desc_status_valid,
```

`rx_engine` 会接收这些 status，并把它们与“该帧对应的 RX 描述符（host buffer 地址）”关联起来。

### 6.2 再由 rx_engine 触发 DMA 子系统：从 RX RAM 写到 host buffer DMA 地址

`rx_engine` 输出 DMA write descriptor，告诉 DMA/PCIe 子系统：

- `dma_addr`：目标 host buffer DMA 地址（来自 RX 描述符 `addr`）
- `ram_addr`：源 RX RAM 地址（由 `rx_engine` 分配给该帧的缓冲区位置）
- `len`：写入长度（来自 `dma_client_axis_sink` 的实际帧长度，且不超过 RX 描述符 `len` 与 MTU）

接口摘要：

```verilog
output wire [DMA_ADDR_WIDTH-1:0] m_axis_dma_write_desc_dma_addr,
output wire [RAM_ADDR_WIDTH-1:0] m_axis_dma_write_desc_ram_addr,
output wire [DMA_LEN_WIDTH-1:0]  m_axis_dma_write_desc_len,
output wire [DMA_TAG_WIDTH-1:0]  m_axis_dma_write_desc_tag,
output wire                      m_axis_dma_write_desc_valid,
input  wire                      m_axis_dma_write_desc_ready,
```

DMA write 成功/失败会通过 `s_axis_dma_write_desc_status_*` 返回，`rx_engine` 用它决定是否生成 error completion、是否推进队列指针等。

到这里，“硬件把以太网包放到内存”的核心动作就完成了：主机内存中 `rx_desc->addr` 指向的 buffer 被 DMA 写入了这一帧的数据。

---

## 7. 硬件侧：生成 completion（驱动据此回收 entry 并上交 skb）

当数据已经成功写入 host buffer 后，`rx_engine` 会生成 completion request（`m_axis_cpl_req_*`），里面会携带：

- RX ring index（entry index）
- 包长度
- hash/hash_type（若使能）
- 校验和信息（若使能）
- 时间戳（若使能）

接口摘要：

```verilog
output wire [CQN_WIDTH-1:0]      m_axis_cpl_req_queue,
output wire [CPL_REQ_TAG_WIDTH-1:0] m_axis_cpl_req_tag,
output wire [CPL_SIZE*8-1:0]     m_axis_cpl_req_data,
output wire                      m_axis_cpl_req_valid,
input  wire                      m_axis_cpl_req_ready,
```

completion 写入 host 内存通常由 `cpl_write` 模块完成，其模式是：

1) 向 completion queue manager 发 enqueue 请求，获取写入地址与 phase
2) 把 completion 数据放在内部 RAM
3) 发起 DMA write，把 completion 写入 host completion ring
4) commit enqueue，推进 completion queue 的 producer pointer

最终驱动在 `mqnic_process_rx_cq()` 中读取 CQ，看到新 completion 后：

- 用 `cpl->index` 找到对应的 RX ring entry
- 把该 entry 对应的 page 组装成 skb 并交给上层（GRO）
- 把 `rx_info->page` 置空，表示该 entry 已被消费
- 推进 `rx_ring->cons_ptr` 并补充新的 RX buffer，写 `prod_ptr` 让硬件继续收包

---

## 8. 关键点回顾：RX “队列/描述符/内存写入”这三者的因果链

1) 驱动为 RX ring 分配 DMA coherent 内存作为“RX 描述符数组”，并把其 DMA 基址写给硬件（queue base address）。

2) 驱动为每个 RX entry 分配 page，DMA map 得到 `dma_addr`，写入：

```c
rx_desc->addr = cpu_to_le64(dma_addr);
rx_desc->len  = cpu_to_le32(len);
```

并推进 `prod_ptr`，写 `MQNIC_QUEUE_CMD_SET_PROD_PTR` 通知硬件。

3) 硬件每收到一帧，就产生一个 RX request；`rx_engine` 按 hash/indirection table 选择一个 RX queue。

4) `rx_engine` 通过 `desc_fetch` 从该 RX queue dequeue 并 DMA 读回 RX 描述符，从中得到 host buffer 的 DMA 地址。

5) 线侧帧由 `mqnic_ingress -> dma_client_axis_sink` 写入 RX RAM。

6) `rx_engine` 发起 DMA write，把 RX RAM 内容写入 host buffer DMA 地址（完成“硬件把包放到内存”）。

7) `rx_engine` 写 completion 到 CQ；驱动在 NAPI 中读取 completion，构造 skb 并补充新的 RX buffer，形成持续收包闭环。

