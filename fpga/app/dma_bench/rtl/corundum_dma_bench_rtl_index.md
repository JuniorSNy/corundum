# Corundum `fpga/app/dma_bench/rtl` 索引

- 范围：仅覆盖 `fpga/app/dma_bench/rtl` 目录下 Verilog 文件（`.v`）
- 输出：每文件给出接口名（按常见前缀/后缀风格聚类）、功能、输入输出要点、内含模块、状态机/流程（重点描述内部控制流程）
- 说明：该目录更偏“应用/bench”，不少逻辑是寄存器驱动的流程控制（run/enable + 计数器 + ready/valid），不一定存在显式 `state` 编码的 FSM

## 目录
- [Corundum `fpga/app/dma_bench/rtl` 索引](#corundum-fpgaappdma_benchrtl-索引)
  - [目录](#目录)
  - [逐文件索引](#逐文件索引)
    - [fpga/app/dma\_bench/rtl/dma\_bench.v](#fpgaappdma_benchrtldma_benchv)
    - [fpga/app/dma\_bench/rtl/dram\_test\_ch.v](#fpgaappdma_benchrtldram_test_chv)
    - [fpga/app/dma\_bench/rtl/mqnic\_app\_block\_dma\_bench.v](#fpgaappdma_benchrtlmqnic_app_block_dma_benchv)

## 逐文件索引

<a id="fpgaappdma-benchrtldma-benchv"></a>
### fpga/app/dma_bench/rtl/dma_bench.v
- 接口名（聚类）：REG(reg_wr_*, reg_rd_*); DMA(m_axis_dma_read_desc_*, s_axis_dma_read_desc_status_*, m_axis_dma_write_desc_*, s_axis_dma_write_desc_status_*); RAM(dma_ram_wr_cmd_*, dma_ram_rd_cmd_*, dma_ram_rd_resp_*)
- 功能：DMA benchmark 控制与发起器；通过寄存器配置发出 DMA read/write 描述符，统计完成情况，并提供单次与批量（block）两种模式
- 输入输出要点：
  - 输入：`reg_wr_* / reg_rd_*` 配置寄存器读写；`s_axis_dma_*_desc_status_*` 接收 DMA 完成状态；`dma_ram_*` 为 DMA 写入/读取 RAM 的分段接口
  - 输出：`m_axis_dma_read_desc_* / m_axis_dma_write_desc_*` 发出 DMA 描述符（ready/valid）；`reg_rd_data/ack` 返回寄存器读数据
- 内含模块（实例化类型）：`dma_psdpram`
- 状态机/流程（工作流程）：
  - 寄存器访问与描述符发起：
    - 写寄存器写到 `RBB+0x100/0x200` 区域可配置“单次 read/write”描述符字段；写到 `RBB+0x114/0x214` 会同时写入 tag/imm_en 并置位 `*_desc_valid`，从而触发一次描述符发送（见 fpga/app/dma_bench/rtl/dma_bench.v:L278-L306）。
    - `*_desc_valid` 在 `always @*` 里实现“保持直到握手成功”：`valid_next = valid_reg && !ready`，因此只要对端未 `ready`，会持续保持有效，直到握手完成（见 fpga/app/dma_bench/rtl/dma_bench.v:L228-L244）。
  - 完成计数与“排队深度”跟踪：
    - `dma_read_active_count_reg`/`dma_write_active_count_reg` 以“已发出且握手成功的描述符数”减去“收到的 status 数”来跟踪 in-flight 操作数量（见 fpga/app/dma_bench/rtl/dma_bench.v:L516-L521）。
    - `s_axis_dma_*_desc_status_*` 到来时把 tag/error/valid 缓存在寄存器里；读寄存器 `RBB+0x118/0x218` 会返回并清除 `*_status_valid`（见 fpga/app/dma_bench/rtl/dma_bench.v:L373-L394）。
  - Block 模式（“隐式状态机”：run + count + ready/valid）：
    - 启动：写 `RBB+0x300`（read）或 `RBB+0x400`（write）置位 `*_block_run_reg`；同时通过 `*_block_len/count/base_addr/offset/mask/stride` 等寄存器配置访问模式（见 fpga/app/dma_bench/rtl/dma_bench.v:L306-L345）。
    - 运行中每拍递增 `*_block_cycle_count`（用于测时）；当 `*_block_count_reg != 0` 且“当前没有 pending valid 或者已 ready”时，生成下一条描述符：根据 base + (offset & mask) 计算 DMA 地址与 RAM 地址；offset 每次加 stride；count 递减；tag 取当前 count（见 fpga/app/dma_bench/rtl/dma_bench.v:L463-L506）。
    - 收尾：当 `*_block_count_reg == 0` 且 `*_active_count_reg == 0`（说明所有已发出的操作都已返回 status）时，自动清除 `*_block_run_reg` 停止（见 fpga/app/dma_bench/rtl/dma_bench.v:L467-L493）。

<a id="fpgaappdma-benchrtldram-test-chv"></a>
### fpga/app/dma_bench/rtl/dram_test_ch.v
- 接口名（聚类）：REG(reg_wr_*, reg_rd_*); AXI(m_axi_*); STREAM(input_data/valid/ready, output_data/valid/ready)
- 功能：DRAM 测试通道；用 PRBS31 生成测试数据写入基于 AXI 的虚拟 FIFO（vfifo），再从 vfifo 读回并做 PRBS 校验与误码统计
- 输入输出要点：
  - 输入：寄存器配置（base/mask/enable/reset、gen/check 控制、计数器设置）；AXI 时钟域 `m_axi_clk/m_axi_rst` 与对应 AXI master 通道 ready/response；vfifo 输出数据 `out_axis_tdata/out_axis_tvalid`
  - 输出：AXI master 发起 DRAM 读写；寄存器读回 occupancy/active/reset 状态与各 lane 的错误计数；vfifo 输入数据 `in_axis_tdata/in_axis_tvalid`
- 内含模块（实例化类型）：`lfsr`（每 lane 两个：gen/check）、`axi_vfifo_raw`
- 状态机/流程（工作流程）：
  - 配置与跨时钟域同步：
    - `fifo_base_addr/fifo_size_mask/fifo_enable/fifo_reset` 在 `clk` 域通过寄存器配置，并用两级寄存器同步到 `m_axi_clk` 域供 `axi_vfifo_raw` 使用（见 fpga/app/dma_bench/rtl/dram_test_ch.v:L130-L172）。
    - vfifo 的 `occupancy/reset/active` 状态从 `m_axi_clk` 域采样，再同步回 `clk` 域供寄存器读出（见 fpga/app/dma_bench/rtl/dram_test_ch.v:L174-L208）。
  - PRBS 生成（写入 vfifo 的数据源）：
    - 每个 lane 使用 `lfsr` 维护 `lane_gen_state_reg`，在 `prbs_gen_en` 时推进状态并输出 `prbs_gen_data`（见 fpga/app/dma_bench/rtl/dram_test_ch.v:L248-L280）。
    - 当 `data_gen_enable_reg` 为 1 且 vfifo 输入侧 ready（`!in_axis_tvalid_reg || in_axis_tready`）时，装载 `in_axis_tdata_next = prbs_gen_data` 并置 `in_axis_tvalid_next`，同时递减 `write_count_reg`；当 `write_count_reg == 1` 时自动清除 `data_gen_enable_reg`（见 fpga/app/dma_bench/rtl/dram_test_ch.v:L370-L382）。
  - PRBS 校验（从 vfifo 读出的数据对比）：
    - `out_axis_tready` 由 `data_check_enable_reg` 控制；当 `out_axis_tready_reg && out_axis_tvalid` 发生一次握手，就对 `out_axis_tdata` 执行 PRBS check（`prbs_check_en`），递减 `read_count_reg`；当 `read_count_reg == 1` 时自动清除 `data_check_enable_reg` 并拉低 ready（见 fpga/app/dma_bench/rtl/dram_test_ch.v:L384-L395）。
    - 每 lane 使用 feed-forward 的 `lfsr` 产生 error bit 向量，并统计每拍 lane 的 bit error 数，累加到 `lane_check_err_cnt_acc_reg`，供寄存器区间 `RBB+0x80 + lane*4` 读出（见 fpga/app/dma_bench/rtl/dram_test_ch.v:L282-L330 和 fpga/app/dma_bench/rtl/dram_test_ch.v:L464-L469）。
  - 计时：只要 gen 或 check 任一 enable 为 1，就递增 `active_cycle_count`（用于衡量测试耗时）（见 fpga/app/dma_bench/rtl/dram_test_ch.v:L397-L399）。

<a id="fpgaappdma-benchrtlmqnic-app-block-dma-benchv"></a>
### fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v
- 接口名（聚类）：AXIL(s_axil_app_ctrl_*); AXIL(m_axil_ctrl_*); DMA(m_axis_*_dma_*_desc_*, s_axis_*_dma_*_desc_status_*); RAM(ctrl_dma_ram_*, data_dma_ram_*); AXIS(s_axis_direct_*, m_axis_direct_*, s_axis_sync_*, m_axis_sync_*, s_axis_if_*, m_axis_if_*); AXI(m_axi_ddr_*, m_axi_hbm_*)
- 功能：MQNIC application block 的 DMA bench 变体；把 host 的 AXI-Lite 控制面接入寄存器接口，实例化 `dma_bench` 作为数据 DMA benchmark，并可选实例化多个 `dram_test_ch` 对 DDR/HBM 做 vfifo+PRBS 压测
- 输入输出要点：
  - 控制面：`s_axil_app_ctrl_*` 输入来自 host；通过 `axil_reg_if` 转换成内部 `ctrl_reg_*`；随后把同一套 `ctrl_reg_*` 广播到 `dma_bench` 与各个 `dram_test_ch`，并把各从模块的 ack/data 做 OR 合并返回（见 fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v:L752-L854）。
  - 数据面：以“直通”方式把 direct/sync/if 的 AXI-stream 端口原样连通（本 app 不改变以太网数据流）（见 fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v:L664-L733）。
  - DMA：将 `dma_bench` 的 DMA 描述符/状态口连接到 data DMA 通道（`m_axis_data_dma_*` / `s_axis_data_dma_*`），控制 DMA 通道在此文件中固定为 0（未启用）（见 fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v:L640-L663 和 fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v:L856-L953）。
  - DRAM：当 `DDR_ENABLE/HBM_ENABLE` 为 1 时，在 generate-for 中为每个通道实例化一个 `dram_test_ch`，并按 group/stride 计算其 vfifo 的 base/mask 与寄存器地址窗口（见 fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v:L955-L1331）。
- 内含模块（实例化类型）：`axil_reg_if`, `dma_bench`, `dram_test_ch`
- 状态机/流程（工作流程）：
  - 本文件本身基本不含显式状态机；主要流程由子模块 `dma_bench`（描述符发起与 block 流程）和 `dram_test_ch`（vfifo+PRBS 流程）承担。
  - 本文件的“控制流程”体现在寄存器事务的 fanout 与 OR 合并：`ctrl_reg_*` 同时驱动多个寄存器块，`*_ack`/`*_data` 通过组合 OR 汇聚返回（见 fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v:L840-L854）。
