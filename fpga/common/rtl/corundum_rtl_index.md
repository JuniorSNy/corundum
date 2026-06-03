# Corundum `fpga/common/rtl` 索引

本文件面向快速理解 Corundum NIC 在 `fpga/common/rtl` 下的 RTL 组成：每个文件/模块的接口概览、功能定位、内部实例化的子模块，以及在整个 `fpga/` 目录内被哪些模块引用（基于文本扫描的启发式结果）。

- 生成范围：`fpga/common/rtl/*.{v,sv,vh,svh}`
- 引用关系扫描范围：`fpga/**/*.{v,sv,vh,svh}`
- 说明：端口/实例化解析为启发式规则，少数 generate/宏展开场景可能遗漏或误判；但足以做“导航索引”。

## 目录

<!-- TOC -->
- [fpga/common/rtl/avst2axis.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/avst2axis.v)
- [fpga/common/rtl/axis2avst.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/axis2avst.v)
- [fpga/common/rtl/cmac_gty_ch_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_gty_ch_wrapper.v)
- [fpga/common/rtl/cmac_gty_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_gty_wrapper.v)
- [fpga/common/rtl/cmac_pad.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_pad.v)
- [fpga/common/rtl/cpl_op_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cpl_op_mux.v)
- [fpga/common/rtl/cpl_queue_manager.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cpl_queue_manager.v)
- [fpga/common/rtl/cpl_write.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cpl_write.v)
- [fpga/common/rtl/desc_fetch.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/desc_fetch.v)
- [fpga/common/rtl/desc_op_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/desc_op_mux.v)
- [fpga/common/rtl/eth_xcvr_phy_10g_gty_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/eth_xcvr_phy_10g_gty_quad_wrapper.v)
- [fpga/common/rtl/eth_xcvr_phy_10g_gty_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/eth_xcvr_phy_10g_gty_wrapper.v)
- [fpga/common/rtl/event_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/event_mux.v)
- [fpga/common/rtl/i2c_single_reg.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/i2c_single_reg.v)
- [fpga/common/rtl/led_sreg_driver.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/led_sreg_driver.v)
- [fpga/common/rtl/mac_ts_insert.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mac_ts_insert.v)
- [fpga/common/rtl/mqnic_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core.v)
- [fpga/common/rtl/mqnic_core_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_axi.v)
- [fpga/common/rtl/mqnic_core_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie.v)
- [fpga/common/rtl/mqnic_core_pcie_ptile.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_ptile.v)
- [fpga/common/rtl/mqnic_core_pcie_s10.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_s10.v)
- [fpga/common/rtl/mqnic_core_pcie_us.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_us.v)
- [fpga/common/rtl/mqnic_dram_if.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_dram_if.v)
- [fpga/common/rtl/mqnic_egress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_egress.v)
- [fpga/common/rtl/mqnic_ingress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ingress.v)
- [fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)
- [fpga/common/rtl/mqnic_interface_rx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_rx.v)
- [fpga/common/rtl/mqnic_interface_tx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_tx.v)
- [fpga/common/rtl/mqnic_l2_egress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_l2_egress.v)
- [fpga/common/rtl/mqnic_l2_ingress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_l2_ingress.v)
- [fpga/common/rtl/mqnic_port.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port.v)
- [fpga/common/rtl/mqnic_port_map_mac_axis.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_map_mac_axis.v)
- [fpga/common/rtl/mqnic_port_map_phy_xgmii.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_map_phy_xgmii.v)
- [fpga/common/rtl/mqnic_port_rx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_rx.v)
- [fpga/common/rtl/mqnic_port_tx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_tx.v)
- [fpga/common/rtl/mqnic_ptp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp.v)
- [fpga/common/rtl/mqnic_ptp_clock.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp_clock.v)
- [fpga/common/rtl/mqnic_ptp_perout.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp_perout.v)
- [fpga/common/rtl/mqnic_rb_clk_info.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_rb_clk_info.v)
- [fpga/common/rtl/mqnic_rx_queue_map.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_rx_queue_map.v)
- [fpga/common/rtl/mqnic_tdma_ber.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tdma_ber.v)
- [fpga/common/rtl/mqnic_tx_scheduler_block_rr.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr.v)
- [fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v)
- [fpga/common/rtl/queue_manager.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/queue_manager.v)
- [fpga/common/rtl/rb_drp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rb_drp.v)
- [fpga/common/rtl/rx_checksum.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_checksum.v)
- [fpga/common/rtl/rx_engine.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_engine.v)
- [fpga/common/rtl/rx_fifo.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_fifo.v)
- [fpga/common/rtl/rx_hash.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_hash.v)
- [fpga/common/rtl/stats_collect.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_collect.v)
- [fpga/common/rtl/stats_counter.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_counter.v)
- [fpga/common/rtl/stats_dma_if_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_axi.v)
- [fpga/common/rtl/stats_dma_if_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_pcie.v)
- [fpga/common/rtl/stats_dma_latency.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_latency.v)
- [fpga/common/rtl/stats_pcie_if.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_pcie_if.v)
- [fpga/common/rtl/stats_pcie_tlp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_pcie_tlp.v)
- [fpga/common/rtl/tdma_ber.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_ber.v)
- [fpga/common/rtl/tdma_ber_ch.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_ber_ch.v)
- [fpga/common/rtl/tdma_scheduler.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_scheduler.v)
- [fpga/common/rtl/tx_checksum.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_checksum.v)
- [fpga/common/rtl/tx_engine.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_engine.v)
- [fpga/common/rtl/tx_fifo.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_fifo.v)
- [fpga/common/rtl/tx_req_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_req_mux.v)
- [fpga/common/rtl/tx_scheduler_ctrl_tdma.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_scheduler_ctrl_tdma.v)
- [fpga/common/rtl/tx_scheduler_rr.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_scheduler_rr.v)
<!-- /TOC -->

## 逐文件索引

 
### [tdma_scheduler.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_scheduler.v)
- 定义模块：`tdma_scheduler`
- 功能：TDMA/BER 测试与调度相关模块
- 接口概览：clk/rst(2), cfg/status(1), other(17)
- 参数（节选）：`INDEX_WIDTH`, `SCHEDULE_START_S`, `SCHEDULE_START_NS`, `SCHEDULE_PERIOD_S`, `SCHEDULE_PERIOD_NS`, `TIMESLOT_PERIOD_S`, `TIMESLOT_PERIOD_NS`, `ACTIVE_PERIOD_S`, `ACTIVE_PERIOD_NS`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_tdma_ber.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tdma_ber.v), [fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v), [fpga/common/rtl/tdma_ber.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_ber.v)

### [tx_checksum.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_checksum.v)
- 定义模块：`tx_checksum`
- 功能：TX 数据通路相关模块（checksum/FIFO/调度等）
- 接口概览：clk/rst(2), AXI-stream(22)
- 参数（节选）：`DATA_WIDTH`, `KEEP_WIDTH`, `ID_ENABLE`, `ID_WIDTH`, `DEST_ENABLE`, `DEST_WIDTH`, `USER_ENABLE`, `USER_WIDTH`, `USE_INIT_VALUE`, `DATA_FIFO_DEPTH` …(+2)
- 内含模块（类型，节选）：`axis_fifo`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_egress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_egress.v)

### [tx_engine.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_engine.v)
- 定义模块：`tx_engine`
- 功能：TX 引擎：取描述符、DMA 读包、发包、完成写回
- 接口概览：clk/rst(4), AXI-stream(75), cfg/status(1), other(1)
- 参数（节选）：`PORTS`, `RAM_ADDR_WIDTH`, `DMA_ADDR_WIDTH`, `DMA_LEN_WIDTH`, `DMA_CLIENT_LEN_WIDTH`, `REQ_TAG_WIDTH`, `DESC_REQ_TAG_WIDTH`, `CPL_REQ_TAG_WIDTH`, `DMA_TAG_WIDTH`, `DMA_CLIENT_TAG_WIDTH` …(+30)
- 内含模块（类型，节选）：`ptp_td_rel2tod`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface_tx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_tx.v)

### [tx_fifo.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_fifo.v)
- 定义模块：`tx_fifo`
- 功能：TX 数据通路相关模块（checksum/FIFO/调度等）
- 接口概览：clk/rst(2), AXI-stream(16), cfg/status(5)
- 参数（节选）：`FIFO_DEPTH`, `FIFO_DEPTH_WIDTH`, `PORTS`, `S_DATA_WIDTH`, `S_KEEP_ENABLE`, `S_KEEP_WIDTH`, `M_DATA_WIDTH`, `M_KEEP_ENABLE`, `M_KEEP_WIDTH`, `ID_ENABLE` …(+6)
- 内含模块（类型，节选）：`axis_demux`, `axis_fifo_adapter`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [tx_req_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_req_mux.v)
- 定义模块：`tx_req_mux`
- 功能：TX 数据通路相关模块（checksum/FIFO/调度等）
- 接口概览：clk/rst(2), AXI-stream(38)
- 参数（节选）：`PORTS`, `QUEUE_INDEX_WIDTH`, `S_REQ_TAG_WIDTH`, `M_REQ_TAG_WIDTH`, `DEST_WIDTH`, `LEN_WIDTH`, `ARB_TYPE_ROUND_ROBIN`, `ARB_LSB_HIGH_PRIORITY`, `CL_PORTS`
- 内含模块（类型，节选）：`arbiter`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [tx_scheduler_ctrl_tdma.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_scheduler_ctrl_tdma.v)
- 定义模块：`tx_scheduler_ctrl_tdma`
- 功能：TX 数据通路相关模块（checksum/FIFO/调度等）
- 接口概览：clk/rst(2), AXI-stream(4), AXI-lite(19), other(5)
- 参数（节选）：`AXIL_DATA_WIDTH`, `AXIL_ADDR_WIDTH`, `AXIL_STRB_WIDTH`, `TDMA_INDEX_WIDTH`, `QUEUE_INDEX_WIDTH`, `PIPELINE`, `WORD_WIDTH`, `WORD_SIZE`, `QUEUE_COUNT`, `QUEUE_RAM_BE_WIDTH` …(+5)
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v)

### [tx_scheduler_rr.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tx_scheduler_rr.v)
- 定义模块：`tx_scheduler_rr`
- 功能：TX 数据通路相关模块（checksum/FIFO/调度等）
- 接口概览：clk/rst(2), AXI-stream(25), AXI-lite(19), cfg/status(1), other(12)
- 参数（节选）：`LEN_WIDTH`, `REQ_DEST_WIDTH`, `REQ_TAG_WIDTH`, `QUEUE_INDEX_WIDTH`, `PIPELINE`, `SCHED_CTRL_ENABLE`, `REQ_DEST_DEFAULT`, `MAX_TX_SIZE`, `FC_SCALE`, `AXIL_BASE_ADDR` …(+9)
- 内含模块（类型，节选）：`axis_fifo`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_tx_scheduler_block_rr.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr.v), [fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v)

### [stats_counter.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_counter.v)
- 定义模块：`stats_counter`
- 功能：统计相关模块（计数、汇聚、DMA/PCIe 统计）
- 接口概览：clk/rst(2), AXI-stream(4), AXI-lite(19)
- 参数（节选）：`STAT_INC_WIDTH`, `STAT_ID_WIDTH`, `STAT_COUNT_WIDTH`, `AXIL_DATA_WIDTH`, `AXIL_ADDR_WIDTH`, `AXIL_STRB_WIDTH`, `PIPELINE`, `ID_SHIFT`, `WORD_SELECT_SHIFT`, `WORD_SELECT_WIDTH`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core.v)

### [stats_dma_if_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_axi.v)
- 定义模块：`stats_dma_if_axi`
- 功能：统计相关模块（计数、汇聚、DMA/PCIe 统计）
- 接口概览：clk/rst(2), AXI-stream(4), other(29)
- 参数（节选）：`LEN_WIDTH`, `READ_OP_TABLE_SIZE`, `WRITE_OP_TABLE_SIZE`, `STAT_INC_WIDTH`, `STAT_ID_WIDTH`, `UPDATE_PERIOD`
- 内含模块（类型，节选）：`stats_dma_latency`, `stats_collect`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_axi.v)

### [stats_dma_if_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_pcie.v)
- 定义模块：`stats_dma_if_pcie`
- 功能：统计相关模块（计数、汇聚、DMA/PCIe 统计）
- 接口概览：clk/rst(2), AXI-stream(4), other(33)
- 参数（节选）：`PCIE_TAG_COUNT`, `LEN_WIDTH`, `READ_OP_TABLE_SIZE`, `WRITE_OP_TABLE_SIZE`, `STAT_INC_WIDTH`, `STAT_ID_WIDTH`, `UPDATE_PERIOD`
- 内含模块（类型，节选）：`stats_dma_latency`, `stats_collect`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie.v)

### [stats_dma_latency.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_latency.v)
- 定义模块：`stats_dma_latency`
- 功能：统计相关模块（计数、汇聚、DMA/PCIe 统计）
- 接口概览：clk/rst(2), other(11)
- 参数（节选）：`COUNT_WIDTH`, `TAG_WIDTH`, `LEN_WIDTH`, `STATUS_WIDTH`
- 被引用（文件，节选）：[fpga/common/rtl/stats_dma_if_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_axi.v), [fpga/common/rtl/stats_dma_if_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_pcie.v)

### [stats_pcie_if.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_pcie_if.v)
- 定义模块：`stats_pcie_if`
- 功能：统计相关模块（计数、汇聚、DMA/PCIe 统计）
- 接口概览：clk/rst(2), AXI-stream(4), other(21)
- 参数（节选）：`TLP_HDR_WIDTH`, `TLP_SEG_COUNT`, `STAT_INC_WIDTH`, `STAT_ID_WIDTH`, `UPDATE_PERIOD`
- 内含模块（类型，节选）：`stats_pcie_tlp`, `stats_collect`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie.v)

### [stats_pcie_tlp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_pcie_tlp.v)
- 定义模块：`stats_pcie_tlp`
- 功能：统计相关模块（计数、汇聚、DMA/PCIe 统计）
- 接口概览：clk/rst(2), RAM(2), other(16)
- 参数（节选）：`TLP_HDR_WIDTH`, `TLP_SEG_COUNT`
- 被引用（文件，节选）：[fpga/common/rtl/stats_pcie_if.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_pcie_if.v)

### [tdma_ber.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_ber.v)
- 定义模块：`tdma_ber`
- 功能：TDMA/BER 测试与调度相关模块
- 接口概览：clk/rst(4), AXI-lite(19), other(5)
- 参数（节选）：`COUNT`, `INDEX_WIDTH`, `SLICE_WIDTH`, `AXIL_DATA_WIDTH`, `AXIL_ADDR_WIDTH`, `AXIL_STRB_WIDTH`, `SCHEDULE_START_S`, `SCHEDULE_START_NS`, `SCHEDULE_PERIOD_S`, `SCHEDULE_PERIOD_NS` …(+5)
- 内含模块（类型，节选）：`tdma_scheduler`, `axil_interconnect`, `tdma_ber_ch`
- 被引用（文件，节选）：[fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/520N_MX/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/520N_MX/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v), [fpga/mqnic/KR260/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/KR260/fpga/rtl/fpga_core.v), [fpga/mqnic/Nexus_K3P_Q/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Nexus_K3P_Q/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/Nexus_K3P_S/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Nexus_K3P_S/fpga_25g/rtl/fpga_core.v)

### [tdma_ber_ch.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_ber_ch.v)
- 定义模块：`tdma_ber_ch`
- 功能：TDMA/BER 测试与调度相关模块
- 接口概览：clk/rst(4), AXI-lite(19), other(6)
- 参数（节选）：`INDEX_WIDTH`, `SLICE_WIDTH`, `AXIL_DATA_WIDTH`, `AXIL_ADDR_WIDTH`, `AXIL_STRB_WIDTH`, `PHY_PIPELINE`, `VALID_ADDR_WIDTH`, `WORD_WIDTH`, `WORD_SIZE`
- 被引用（文件，节选）：[fpga/common/rtl/tdma_ber.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/tdma_ber.v)

### [mqnic_tdma_ber.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tdma_ber.v)
- 定义模块：`mqnic_tdma_ber`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(4), other(16)
- 参数（节选）：`COUNT`, `TDMA_INDEX_W`, `ERR_BITS`, `ERR_CNT_W`, `RAM_SIZE`, `PHY_PIPELINE`, `REG_ADDR_WIDTH`, `REG_DATA_WIDTH`, `REG_STRB_WIDTH`, `RB_BASE_ADDR` …(+1)
- 内含模块（类型，节选）：`tdma_scheduler`
- 被引用（文件，节选）：[fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/fb2CG/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/fb2CG/fpga_25g/rtl/fpga_core.v)

### [mqnic_tx_scheduler_block_rr.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr.v)
- 定义模块：`mqnic_tx_scheduler_block`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(5), AXI-stream(21), AXI-lite(19), other(25)
- 参数（节选）：`PORTS`, `INDEX`, `CLK_PERIOD_NS_NUM`, `CLK_PERIOD_NS_DENOM`, `PTP_CLK_PERIOD_NS_NUM`, `PTP_CLK_PERIOD_NS_DENOM`, `PTP_CLOCK_CDC_PIPELINE`, `PTP_PEROUT_ENABLE`, `PTP_PEROUT_COUNT`, `QUEUE_INDEX_WIDTH` …(+18)
- 内含模块（类型，节选）：`tx_scheduler_rr`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [mqnic_tx_scheduler_block_rr_tdma.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v)
- 定义模块：`mqnic_tx_scheduler_block`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(5), AXI-stream(21), AXI-lite(19), other(25)
- 参数（节选）：`PORTS`, `INDEX`, `CLK_PERIOD_NS_NUM`, `CLK_PERIOD_NS_DENOM`, `PTP_CLK_PERIOD_NS_NUM`, `PTP_CLK_PERIOD_NS_DENOM`, `PTP_CLOCK_CDC_PIPELINE`, `PTP_PEROUT_ENABLE`, `PTP_PEROUT_COUNT`, `QUEUE_INDEX_WIDTH` …(+19)
- 内含模块（类型，节选）：`axil_crossbar`, `tx_scheduler_rr`, `tdma_scheduler`, `tx_scheduler_ctrl_tdma`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [queue_manager.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/queue_manager.v)
- 定义模块：`queue_manager`
- 功能：（自动解析）
- 接口概览：clk/rst(2), AXI-stream(21), AXI-lite(19), cfg/status(1)
- 参数（节选）：`ADDR_WIDTH`, `REQ_TAG_WIDTH`, `OP_TABLE_SIZE`, `OP_TAG_WIDTH`, `QUEUE_INDEX_WIDTH`, `CPL_INDEX_WIDTH`, `QUEUE_PTR_WIDTH`, `LOG_QUEUE_SIZE_WIDTH`, `DESC_SIZE`, `LOG_BLOCK_SIZE_WIDTH` …(+9)
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [rb_drp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rb_drp.v)
- 定义模块：`rb_drp`
- 功能：（自动解析）
- 接口概览：clk/rst(4), other(17)
- 参数（节选）：`DRP_ADDR_WIDTH`, `DRP_DATA_WIDTH`, `DRP_INFO`, `REG_ADDR_WIDTH`, `REG_DATA_WIDTH`, `REG_STRB_WIDTH`, `RB_TYPE`, `RB_BASE_ADDR`, `RB_NEXT_PTR`
- 被引用（文件，节选）：[fpga/mqnic/250_SoC/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/Alveo/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v), [fpga/mqnic/KR260/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/KR260/fpga/rtl/fpga_core.v)

### [rx_checksum.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_checksum.v)
- 定义模块：`rx_checksum`
- 功能：RX 数据通路相关模块（hash/checksum/FIFO 等）
- 接口概览：clk/rst(2), AXI-stream(6)
- 参数（节选）：`DATA_WIDTH`, `KEEP_WIDTH`, `START_OFFSET`, `LEVELS`, `OFFSET_WIDTH`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_ingress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ingress.v)

### [rx_engine.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_engine.v)
- 定义模块：`rx_engine`
- 功能：RX 引擎：取描述符、收包、DMA 写回、完成写回
- 接口概览：clk/rst(4), AXI-stream(57), AXI-lite(19), cfg/status(1), other(13)
- 参数（节选）：`PORTS`, `RAM_ADDR_WIDTH`, `DMA_ADDR_WIDTH`, `DMA_LEN_WIDTH`, `DMA_CLIENT_LEN_WIDTH`, `REQ_TAG_WIDTH`, `DESC_REQ_TAG_WIDTH`, `CPL_REQ_TAG_WIDTH`, `DMA_TAG_WIDTH`, `DMA_CLIENT_TAG_WIDTH` …(+32)
- 内含模块（类型，节选）：`mqnic_rx_queue_map`, `ptp_td_rel2tod`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface_rx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_rx.v)

### [rx_fifo.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_fifo.v)
- 定义模块：`rx_fifo`
- 功能：RX 数据通路相关模块（hash/checksum/FIFO 等）
- 接口概览：clk/rst(2), AXI-stream(16), cfg/status(5)
- 参数（节选）：`FIFO_DEPTH`, `FIFO_DEPTH_WIDTH`, `PORTS`, `S_DATA_WIDTH`, `S_KEEP_ENABLE`, `S_KEEP_WIDTH`, `M_DATA_WIDTH`, `M_KEEP_ENABLE`, `M_KEEP_WIDTH`, `ID_ENABLE` …(+7)
- 内含模块（类型，节选）：`axis_fifo_adapter`, `axis_arb_mux`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [rx_hash.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_hash.v)
- 定义模块：`rx_hash`
- 功能：RX 数据通路相关模块（hash/checksum/FIFO 等）
- 接口概览：clk/rst(2), AXI-stream(7), other(1)
- 参数（节选）：`DATA_WIDTH`, `KEEP_WIDTH`, `CYCLE_COUNT`, `PTR_WIDTH`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_ingress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ingress.v)

### [stats_collect.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_collect.v)
- 定义模块：`stats_collect`
- 功能：统计相关模块（计数、汇聚、DMA/PCIe 统计）
- 接口概览：clk/rst(2), AXI-stream(4), other(3)
- 参数（节选）：`COUNT`, `INC_WIDTH`, `STAT_INC_WIDTH`, `STAT_ID_WIDTH`, `UPDATE_PERIOD`, `COUNT_WIDTH`, `PERIOD_COUNT_WIDTH`, `ACC_WIDTH`
- 被引用（文件，节选）：[fpga/common/rtl/stats_dma_if_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_axi.v), [fpga/common/rtl/stats_dma_if_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_dma_if_pcie.v), [fpga/common/rtl/stats_pcie_if.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/stats_pcie_if.v)

### [mqnic_port.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port.v)
- 定义模块：`mqnic_port`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(6), AXI-stream(96), other(29)
- 参数（节选）：`PTP_TS_WIDTH`, `PTP_TS_ENABLE`, `TX_CPL_ENABLE`, `TX_CPL_FIFO_DEPTH`, `TX_TAG_WIDTH`, `PFC_ENABLE`, `LFC_ENABLE`, `MAC_CTRL_ENABLE`, `TX_FIFO_DEPTH`, `RX_FIFO_DEPTH` …(+25)
- 内含模块（类型，节选）：`mqnic_port_tx`, `mqnic_port_rx`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [mqnic_port_map_mac_axis.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_map_mac_axis.v)
- 定义模块：`mqnic_port_map_mac_axis`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(16), AXI-stream(32), other(36)
- 参数（节选）：`MAC_COUNT`, `PORT_MASK`, `PORT_GROUP_SIZE`, `IF_COUNT`, `PORTS_PER_IF`, `PORT_COUNT`, `PTP_TS_WIDTH`, `PTP_TAG_WIDTH`, `AXIS_DATA_WIDTH`, `AXIS_KEEP_WIDTH` …(+2)
- 被引用（文件，节选）：[fpga/mqnic/250_SoC/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/Alveo/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/DE10_Agilex/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/IA_420F/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/IA_420F/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/VCU118/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/VCU118/fpga_100g/rtl/fpga_core.v)

### [mqnic_port_map_phy_xgmii.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_map_phy_xgmii.v)
- 定义模块：`mqnic_port_map_phy_xgmii`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(8), other(12)
- 参数（节选）：`PHY_COUNT`, `PORT_MASK`, `PORT_GROUP_SIZE`, `IF_COUNT`, `PORTS_PER_IF`, `PORT_COUNT`, `XGMII_DATA_WIDTH`, `XGMII_CTRL_WIDTH`
- 被引用（文件，节选）：[fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/520N_MX/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/520N_MX/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v), [fpga/mqnic/KR260/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/KR260/fpga/rtl/fpga_core.v), [fpga/mqnic/NetFPGA_SUME/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/NetFPGA_SUME/fpga/rtl/fpga_core.v)

### [mqnic_port_rx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_rx.v)
- 定义模块：`mqnic_port_rx`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(4), AXI-stream(36), other(10)
- 参数（节选）：`PTP_TS_WIDTH`, `PTP_TS_ENABLE`, `PFC_ENABLE`, `LFC_ENABLE`, `MAC_CTRL_ENABLE`, `MAX_RX_SIZE`, `APP_AXIS_DIRECT_ENABLE`, `APP_AXIS_SYNC_ENABLE`, `AXIS_DATA_WIDTH`, `AXIS_KEEP_WIDTH` …(+7)
- 内含模块（类型，节选）：`mqnic_l2_ingress`, `axis_async_fifo_adapter`, `axis_pipeline_fifo`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_port.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port.v)

### [mqnic_port_tx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_tx.v)
- 定义模块：`mqnic_port_tx`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(4), AXI-stream(60), other(10)
- 参数（节选）：`PTP_TS_WIDTH`, `PTP_TS_ENABLE`, `TX_CPL_ENABLE`, `TX_CPL_FIFO_DEPTH`, `TX_TAG_WIDTH`, `PFC_ENABLE`, `LFC_ENABLE`, `MAC_CTRL_ENABLE`, `MAX_TX_SIZE`, `APP_AXIS_DIRECT_ENABLE` …(+10)
- 内含模块（类型，节选）：`axis_async_fifo`, `axis_pipeline_fifo`, `axis_async_fifo_adapter`, `mqnic_l2_egress`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_port.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port.v)

### [mqnic_ptp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp.v)
- 定义模块：`mqnic_ptp`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(5), other(24)
- 参数（节选）：`PTP_CLK_PERIOD_NS_NUM`, `PTP_CLK_PERIOD_NS_DENOM`, `PTP_CLOCK_CDC_PIPELINE`, `PTP_PEROUT_ENABLE`, `PTP_PEROUT_COUNT`, `REG_ADDR_WIDTH`, `REG_DATA_WIDTH`, `REG_STRB_WIDTH`, `RB_BASE_ADDR`, `RB_NEXT_PTR`
- 内含模块（类型，节选）：`mqnic_ptp_clock`, `mqnic_ptp_perout`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core.v)

### [mqnic_ptp_clock.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp_clock.v)
- 定义模块：`mqnic_ptp_clock`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(5), other(21)
- 参数（节选）：`PTP_CLK_PERIOD_NS_NUM`, `PTP_CLK_PERIOD_NS_DENOM`, `PTP_CLOCK_CDC_PIPELINE`, `REG_ADDR_WIDTH`, `REG_DATA_WIDTH`, `REG_STRB_WIDTH`, `RB_BASE_ADDR`, `RB_NEXT_PTR`, `PTP_FNS_WIDTH`, `PTP_CLK_PERIOD_NS` …(+3)
- 内含模块（类型，节选）：`ptp_td_phc`, `ptp_td_leaf`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_ptp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp.v)

### [mqnic_ptp_perout.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp_perout.v)
- 定义模块：`mqnic_ptp_perout`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(2), other(16)
- 参数（节选）：`REG_ADDR_WIDTH`, `REG_DATA_WIDTH`, `REG_STRB_WIDTH`, `RB_BASE_ADDR`, `RB_NEXT_PTR`
- 内含模块（类型，节选）：`ptp_perout`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_ptp.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ptp.v)

### [mqnic_rb_clk_info.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_rb_clk_info.v)
- 定义模块：`mqnic_rb_clk_info`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(4), other(11)
- 参数（节选）：`CLK_PERIOD_NS_NUM`, `CLK_PERIOD_NS_DENOM`, `REF_CLK_PERIOD_NS_NUM`, `REF_CLK_PERIOD_NS_DENOM`, `CH_CNT`, `REG_ADDR_WIDTH`, `REG_DATA_WIDTH`, `REG_STRB_WIDTH`, `RB_TYPE`, `RB_BASE_ADDR` …(+1)
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core.v)

### [mqnic_rx_queue_map.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_rx_queue_map.v)
- 定义模块：`mqnic_rx_queue_map`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(2), AXI-lite(19), other(19)
- 参数（节选）：`PORTS`, `QUEUE_INDEX_WIDTH`, `INDIR_TBL_ADDR_WIDTH`, `ID_WIDTH`, `DEST_WIDTH`, `HASH_WIDTH`, `TAG_WIDTH`, `REG_ADDR_WIDTH`, `REG_DATA_WIDTH`, `REG_STRB_WIDTH` …(+6)
- 被引用（文件，节选）：[fpga/common/rtl/rx_engine.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/rx_engine.v)

### [mqnic_core_pcie_s10.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_s10.v)
- 定义模块：`mqnic_core_pcie_s10`
- 功能：MQNIC 核心 + PCIe 主机接口封装（含 DMA/AXI-lite 等 glue logic）
- 接口概览：clk/rst(17), AXI-stream(16), AXI-lite(19), other(145)
- 参数（节选）：`FPGA_ID`, `FW_ID`, `FW_VER`, `BOARD_ID`, `BOARD_VER`, `BUILD_DATE`, `GIT_HASH`, `RELEASE_INFO`, `IF_COUNT`, `PORTS_PER_IF` …(+150)
- 内含模块（类型，节选）：`pcie_s10_if`, `mqnic_core_pcie`
- 被引用（文件，节选）：[fpga/mqnic/520N_MX/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/520N_MX/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v)

### [mqnic_core_pcie_us.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_us.v)
- 定义模块：`mqnic_core_pcie_us`
- 功能：MQNIC 核心 + PCIe 主机接口封装（含 DMA/AXI-lite 等 glue logic）
- 接口概览：clk/rst(15), AXI-stream(44), AXI-lite(19), cfg/status(32), other(64)
- 参数（节选）：`FPGA_ID`, `FW_ID`, `FW_VER`, `BOARD_ID`, `BOARD_VER`, `BUILD_DATE`, `GIT_HASH`, `RELEASE_INFO`, `IF_COUNT`, `PORTS_PER_IF` …(+155)
- 内含模块（类型，节选）：`pcie_us_if`, `mqnic_core_pcie`
- 被引用（文件，节选）：[fpga/mqnic/250_SoC/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/Alveo/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v), [fpga/mqnic/NetFPGA_SUME/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/NetFPGA_SUME/fpga/rtl/fpga_core.v)

### [mqnic_dram_if.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_dram_if.v)
- 定义模块：`mqnic_dram_if`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(6), cfg/status(1), other(85)
- 参数（节选）：`CH`, `GROUP_SIZE`, `AXI_DATA_WIDTH`, `AXI_ADDR_WIDTH`, `AXI_STRB_WIDTH`, `AXI_ID_WIDTH`, `AXI_AWUSER_ENABLE`, `AXI_AWUSER_WIDTH`, `AXI_WUSER_ENABLE`, `AXI_WUSER_WIDTH` …(+10)
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core.v)

### [mqnic_egress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_egress.v)
- 定义模块：`mqnic_egress`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(2), AXI-stream(16), other(5)
- 参数（节选）：`TX_CHECKSUM_ENABLE`, `AXIS_DATA_WIDTH`, `AXIS_KEEP_WIDTH`, `AXIS_ID_WIDTH`, `AXIS_DEST_WIDTH`, `AXIS_USER_WIDTH`, `MAX_TX_SIZE`
- 内含模块（类型，节选）：`axis_fifo`, `tx_checksum`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface_tx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_tx.v)

### [mqnic_ingress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_ingress.v)
- 定义模块：`mqnic_ingress`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(2), AXI-stream(16), other(3)
- 参数（节选）：`REQ_TAG_WIDTH`, `RX_QUEUE_INDEX_WIDTH`, `RX_HASH_ENABLE`, `RX_CHECKSUM_ENABLE`, `AXIS_DATA_WIDTH`, `AXIS_KEEP_WIDTH`, `AXIS_ID_WIDTH`, `AXIS_DEST_WIDTH`, `S_AXIS_USER_WIDTH`, `M_AXIS_USER_WIDTH` …(+1)
- 内含模块（类型，节选）：`rx_hash`, `axis_fifo`, `rx_checksum`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface_rx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_rx.v)

### [mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)
- 定义模块：`mqnic_interface`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(6), AXI-stream(159), AXI-lite(38), RAM(28), other(7)
- 参数（节选）：`PORTS`, `SCHEDULERS`, `CLK_PERIOD_NS_NUM`, `CLK_PERIOD_NS_DENOM`, `PTP_CLK_PERIOD_NS_NUM`, `PTP_CLK_PERIOD_NS_DENOM`, `PTP_CLOCK_CDC_PIPELINE`, `PTP_PEROUT_ENABLE`, `PTP_PEROUT_COUNT`, `EVENT_QUEUE_OP_TABLE_SIZE` …(+83)
- 内含模块（类型，节选）：`axis_fifo`, `axil_reg_if`, `axil_crossbar`, `dma_if_mux_wr`, `cpl_queue_manager`, `cpl_write`, `cpl_op_mux`, `queue_manager`, `desc_op_mux`, `desc_fetch`, `mqnic_tx_scheduler_block`, `tx_req_mux` …(+6)
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core.v)

### [mqnic_interface_rx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_rx.v)
- 定义模块：`mqnic_interface_rx`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(4), AXI-stream(44), AXI-lite(19), RAM(6), other(13)
- 参数（节选）：`PORTS`, `RX_QUEUE_INDEX_WIDTH`, `QUEUE_INDEX_WIDTH`, `CQN_WIDTH`, `QUEUE_PTR_WIDTH`, `LOG_QUEUE_SIZE_WIDTH`, `LOG_BLOCK_SIZE_WIDTH`, `RX_MAX_DESC_REQ`, `RX_DESC_FIFO_SIZE`, `DESC_SIZE` …(+41)
- 内含模块（类型，节选）：`axis_fifo`, `rx_engine`, `dma_psdpram`, `mqnic_ingress`, `dma_client_axis_sink`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [mqnic_interface_tx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface_tx.v)
- 定义模块：`mqnic_interface_tx`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(4), AXI-stream(67), RAM(6), other(2)
- 参数（节选）：`PORTS`, `TX_QUEUE_INDEX_WIDTH`, `QUEUE_INDEX_WIDTH`, `CQN_WIDTH`, `QUEUE_PTR_WIDTH`, `LOG_QUEUE_SIZE_WIDTH`, `LOG_BLOCK_SIZE_WIDTH`, `TX_MAX_DESC_REQ`, `TX_DESC_FIFO_SIZE`, `DESC_SIZE` …(+31)
- 内含模块（类型，节选）：`axis_fifo`, `tx_engine`, `dma_psdpram`, `dma_client_axis_source`, `mqnic_egress`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [mqnic_l2_egress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_l2_egress.v)
- 定义模块：`mqnic_l2_egress`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(2), AXI-stream(12), other(8)
- 参数（节选）：`PFC_ENABLE`, `LFC_ENABLE`, `MAC_CTRL_ENABLE`, `AXIS_DATA_WIDTH`, `AXIS_KEEP_WIDTH`, `AXIS_USER_WIDTH`
- 内含模块（类型，节选）：`mac_ctrl_tx`, `mac_pause_ctrl_tx`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_port_tx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_tx.v)

### [mqnic_l2_ingress.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_l2_ingress.v)
- 定义模块：`mqnic_l2_ingress`
- 功能：MQNIC 组件（端口/接口/调度/PTP/统计/寄存器块等）
- 接口概览：clk/rst(2), AXI-stream(12), other(8)
- 参数（节选）：`PFC_ENABLE`, `LFC_ENABLE`, `MAC_CTRL_ENABLE`, `AXIS_DATA_WIDTH`, `AXIS_KEEP_WIDTH`, `AXIS_USER_WIDTH`, `AXIS_USE_READY`
- 内含模块（类型，节选）：`mac_ctrl_rx`, `mac_pause_ctrl_rx`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_port_rx.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_port_rx.v)

### [eth_xcvr_phy_10g_gty_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/eth_xcvr_phy_10g_gty_quad_wrapper.v)
- 定义模块：`eth_xcvr_phy_10g_gty_quad_wrapper`
- 功能：高速以太网 MAC/PHY/收发器相关封装或适配
- 接口概览：clk/rst(20), PCIe(2), other(75)
- 参数（节选）：`COUNT`, `GT_GTH`, `GT_USP`, `QPLL0_PD`, `QPLL1_PD`, `QPLL0_EXT_CTRL`, `QPLL1_EXT_CTRL`, `GT_1_TX_PD`, `GT_1_TX_QPLL_SEL`, `GT_1_TX_POLARITY` …(+58)
- 内含模块（类型，节选）：`eth_xcvr_phy_10g_gty_wrapper`
- 被引用（文件，节选）：[fpga/mqnic/250_SoC/fpga_25g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_25g/rtl/fpga.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au200.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au200.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au280.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au280.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au50.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au50.v), [fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au55.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au55.v), [fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga.v), [fpga/mqnic/KR260/fpga/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/KR260/fpga/rtl/fpga.v)

### [eth_xcvr_phy_10g_gty_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/eth_xcvr_phy_10g_gty_wrapper.v)
- 定义模块：`eth_xcvr_phy_10g_gty_wrapper`
- 功能：高速以太网 MAC/PHY/收发器相关封装或适配
- 接口概览：clk/rst(8), PCIe(2), other(42)
- 参数（节选）：`INDEX`, `HAS_COMMON`, `GT_GTH`, `GT_USP`, `QPLL0_PD`, `QPLL1_PD`, `QPLL0_EXT_CTRL`, `QPLL1_EXT_CTRL`, `GT_TX_PD`, `GT_TX_QPLL_SEL` …(+20)
- 内含模块（类型，节选）：`sync_reset`, `eth_phy_10g`
- 被引用（文件，节选）：[fpga/common/rtl/eth_xcvr_phy_10g_gty_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/eth_xcvr_phy_10g_gty_quad_wrapper.v)

### [event_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/event_mux.v)
- 定义模块：`event_mux`
- 功能：（自动解析）
- 接口概览：clk/rst(2), AXI-stream(10)
- 参数（节选）：`PORTS`, `QUEUE_INDEX_WIDTH`, `EVENT_TYPE_WIDTH`, `EVENT_SOURCE_WIDTH`, `ARB_TYPE_ROUND_ROBIN`, `ARB_LSB_HIGH_PRIORITY`, `CL_PORTS`
- 内含模块（类型，节选）：`arbiter`
- 被引用：未在 fpga/ 扫描范围内发现实例化引用（或仅宏/生成语句间接引用）

### [i2c_single_reg.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/i2c_single_reg.v)
- 定义模块：`i2c_single_reg`
- 功能：（自动解析）
- 接口概览：clk/rst(2), other(9)
- 参数（节选）：`FILTER_LEN`, `DEV_ADDR`
- 被引用（文件，节选）：[fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/Nexus_K3P_S/fpga_25g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Nexus_K3P_S/fpga_25g/rtl/fpga_core.v)

### [led_sreg_driver.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/led_sreg_driver.v)
- 定义模块：`led_sreg_driver`
- 功能：（自动解析）
- 接口概览：clk/rst(3), other(4)
- 参数（节选）：`COUNT`, `INVERT`, `REVERSE`, `INTERLEAVE`, `PRESCALE`
- 被引用（文件，节选）：[fpga/lib/eth/example/fb2CG/fpga_25g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/lib/eth/example/fb2CG/fpga_25g/rtl/fpga.v), [fpga/lib/pcie/example/fb2CG/fpga/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/lib/pcie/example/fb2CG/fpga/rtl/fpga.v), [fpga/lib/pcie/example/fb2CG/fpga_axi/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/lib/pcie/example/fb2CG/fpga_axi/rtl/fpga.v), [fpga/mqnic/fb2CG/fpga_100g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/fb2CG/fpga_100g/rtl/fpga.v), [fpga/mqnic/fb2CG/fpga_25g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/fb2CG/fpga_25g/rtl/fpga.v), [fpga/mqnic/fb4CGg3/fpga_100g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/fb4CGg3/fpga_100g/rtl/fpga.v), [fpga/mqnic/fb4CGg3/fpga_25g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/fb4CGg3/fpga_25g/rtl/fpga.v)

### [mac_ts_insert.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mac_ts_insert.v)
- 定义模块：`mac_ts_insert`
- 功能：（自动解析）
- 接口概览：clk/rst(2), AXI-stream(12), other(1)
- 参数（节选）：`PTP_TS_WIDTH`, `DATA_WIDTH`, `KEEP_WIDTH`, `S_USER_WIDTH`, `M_USER_WIDTH`
- 被引用（文件，节选）：[fpga/common/rtl/cmac_gty_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_gty_wrapper.v), [fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v), [fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_wrapper.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_quad_wrapper.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_wrapper.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_wrapper.v), [fpga/mqnic/IA_420F/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/IA_420F/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v)

### [mqnic_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core.v)
- 定义模块：`mqnic_core`
- 功能：MQNIC 核心顶层/封装（队列、PTP、统计、应用区等）
- 接口概览：clk/rst(15), AXI-stream(38), AXI-lite(76), RAM(14), other(50)
- 参数（节选）：`FPGA_ID`, `FW_ID`, `FW_VER`, `BOARD_ID`, `BOARD_VER`, `BUILD_DATE`, `GIT_HASH`, `RELEASE_INFO`, `IF_COUNT`, `PORTS_PER_IF` …(+142)
- 内含模块（类型，节选）：`axil_reg_if`, `mqnic_ptp`, `mqnic_rb_clk_info`, `axil_crossbar`, `axis_arb_mux`, `stats_counter`, `dma_if_mux`, `irq_rate_limit`, `mqnic_dram_if`, `mqnic_interface`, `ptp_td_leaf`, `mqnic_app_block`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_axi.v), [fpga/common/rtl/mqnic_core_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie.v)

### [mqnic_core_axi.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_axi.v)
- 定义模块：`mqnic_core_axi`
- 功能：MQNIC 核心顶层/封装（队列、PTP、统计、应用区等）
- 接口概览：clk/rst(15), AXI-stream(16), AXI-lite(57), other(120)
- 参数（节选）：`FPGA_ID`, `FW_ID`, `FW_VER`, `BOARD_ID`, `BOARD_VER`, `BUILD_DATE`, `GIT_HASH`, `RELEASE_INFO`, `IF_COUNT`, `PORTS_PER_IF` …(+144)
- 内含模块（类型，节选）：`dma_if_axi`, `stats_dma_if_axi`, `axis_arb_mux`, `mqnic_core`
- 被引用（文件，节选）：[fpga/mqnic/KR260/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/KR260/fpga/rtl/fpga_core.v), [fpga/mqnic/ZCU102/fpga/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ZCU102/fpga/rtl/fpga_core.v), [fpga/mqnic/ZCU102/fpga/rtl/fpga_core_custom_port_demo.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ZCU102/fpga/rtl/fpga_core_custom_port_demo.v), [fpga/mqnic/ZCU106/fpga_zynqmp/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ZCU106/fpga_zynqmp/rtl/fpga_core.v)

### [mqnic_core_pcie.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie.v)
- 定义模块：`mqnic_core_pcie`
- 功能：MQNIC 核心 + PCIe 主机接口封装（含 DMA/AXI-lite 等 glue logic）
- 接口概览：clk/rst(15), AXI-stream(20), AXI-lite(19), PCIe(47), other(75)
- 参数（节选）：`FPGA_ID`, `FW_ID`, `FW_VER`, `BOARD_ID`, `BOARD_VER`, `BUILD_DATE`, `GIT_HASH`, `RELEASE_INFO`, `IF_COUNT`, `PORTS_PER_IF` …(+153)
- 内含模块（类型，节选）：`pcie_tlp_demux_bar`, `pcie_tlp_mux`, `pcie_axil_master`, `dma_if_pcie`, `pcie_msix`, `pulse_merge`, `stats_pcie_if`, `stats_dma_if_pcie`, `axis_arb_mux`, `mqnic_core`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_core_pcie_ptile.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_ptile.v), [fpga/common/rtl/mqnic_core_pcie_s10.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_s10.v), [fpga/common/rtl/mqnic_core_pcie_us.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_us.v)

### [mqnic_core_pcie_ptile.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_core_pcie_ptile.v)
- 定义模块：`mqnic_core_pcie_ptile`
- 功能：MQNIC 核心 + PCIe 主机接口封装（含 DMA/AXI-lite 等 glue logic）
- 接口概览：clk/rst(17), AXI-stream(16), AXI-lite(19), other(140)
- 参数（节选）：`FPGA_ID`, `FW_ID`, `FW_VER`, `BOARD_ID`, `BOARD_VER`, `BUILD_DATE`, `GIT_HASH`, `RELEASE_INFO`, `IF_COUNT`, `PORTS_PER_IF` …(+151)
- 内含模块（类型，节选）：`pcie_ptile_if`, `mqnic_core_pcie`
- 被引用（文件，节选）：[fpga/mqnic/DE10_Agilex/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/fpga_core.v), [fpga/mqnic/IA_420F/fpga_100g/rtl/fpga_core.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/IA_420F/fpga_100g/rtl/fpga_core.v)

### [avst2axis.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/avst2axis.v)
- 定义模块：`avst2axis`
- 功能：Intel AVST <-> AXI-stream 适配
- 接口概览：clk/rst(2), other(13)
- 参数（节选）：`DATA_WIDTH`, `KEEP_WIDTH`, `KEEP_ENABLE`, `EMPTY_WIDTH`, `BYTE_REVERSE`, `BYTE_WIDTH`
- 被引用（文件，节选）：[fpga/lib/eth/example/S10DX_DK/fpga_10g/rtl/eth_mac_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/lib/eth/example/S10DX_DK/fpga_10g/rtl/eth_mac_quad_wrapper.v), [fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v), [fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_wrapper.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_quad_wrapper.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_wrapper.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_wrapper.v), [fpga/mqnic/IA_420F/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/IA_420F/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v)

### [axis2avst.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/axis2avst.v)
- 定义模块：`axis2avst`
- 功能：Intel AVST <-> AXI-stream 适配
- 接口概览：clk/rst(2), other(13)
- 参数（节选）：`DATA_WIDTH`, `KEEP_WIDTH`, `KEEP_ENABLE`, `EMPTY_WIDTH`, `BYTE_REVERSE`, `BYTE_WIDTH`
- 被引用（文件，节选）：[fpga/lib/eth/example/S10DX_DK/fpga_10g/rtl/eth_mac_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/lib/eth/example/S10DX_DK/fpga_10g/rtl/eth_mac_quad_wrapper.v), [fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v), [fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DE10_Agilex/fpga_100g/rtl/eth_mac_dual_wrapper.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_quad_wrapper.v), [fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_1SDX_P_A/fpga_100g/rtl/eth_mac_wrapper.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v), [fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/DK_DEV_AGF014EA/fpga_100g/rtl/eth_mac_dual_wrapper.v), [fpga/mqnic/IA_420F/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/IA_420F/fpga_100g/rtl/eth_mac_dual_quad_wrapper.v)

### [cmac_gty_ch_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_gty_ch_wrapper.v)
- 定义模块：`cmac_gty_ch_wrapper`
- 功能：高速以太网 MAC/PHY/收发器相关封装或适配
- 接口概览：clk/rst(4), other(45)
- 参数（节选）：`INDEX`, `HAS_COMMON`, `QPLL0_PD`, `QPLL1_PD`, `GT_TX_PD`, `GT_TX_QPLL_SEL`, `GT_TX_POLARITY`, `GT_TX_ELECIDLE`, `GT_TX_INHIBIT`, `GT_TX_DIFFCTRL` …(+7)
- 内含模块（类型，节选）：`sync_reset`
- 被引用（文件，节选）：[fpga/common/rtl/cmac_gty_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_gty_wrapper.v)

### [cmac_gty_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_gty_wrapper.v)
- 定义模块：`cmac_gty_wrapper`
- 功能：高速以太网 MAC/PHY/收发器相关封装或适配
- 接口概览：clk/rst(11), AXI-stream(11), other(29)
- 参数（节选）：`COUNT`, `DRP_CLK_FREQ_HZ`, `QPLL0_PD`, `QPLL1_PD`, `GT_1_TX_PD`, `GT_1_TX_QPLL_SEL`, `GT_1_TX_POLARITY`, `GT_1_TX_ELECIDLE`, `GT_1_TX_INHIBIT`, `GT_1_TX_DIFFCTRL` …(+52)
- 内含模块（类型，节选）：`BUFG_GT`, `sync_reset`, `cmac_gty_ch_wrapper`, `cmac_pad`, `mac_ts_insert`, `cmac_usplus`
- 被引用（文件，节选）：[fpga/mqnic/250_SoC/fpga_100g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/250_SoC/fpga_100g/rtl/fpga.v), [fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/ADM_PCIE_9V3/fpga_100g/rtl/fpga.v), [fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au200.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au200.v), [fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au280.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au280.v), [fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au50.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au50.v), [fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au55.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au55.v), [fpga/mqnic/VCU118/fpga_100g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/VCU118/fpga_100g/rtl/fpga.v), [fpga/mqnic/XUPP3R/fpga_100g/rtl/fpga.v](file:///Users/sj/Documents/FPGA/corundum/fpga/mqnic/XUPP3R/fpga_100g/rtl/fpga.v)

### [cmac_pad.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_pad.v)
- 定义模块：`cmac_pad`
- 功能：高速以太网 MAC/PHY/收发器相关封装或适配
- 接口概览：clk/rst(2), AXI-stream(12)
- 参数（节选）：`DATA_WIDTH`, `KEEP_WIDTH`, `USER_WIDTH`
- 被引用（文件，节选）：[fpga/common/rtl/cmac_gty_wrapper.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cmac_gty_wrapper.v)

### [cpl_op_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cpl_op_mux.v)
- 定义模块：`cpl_op_mux`
- 功能：完成写回操作复用：合并多个源到单个 cpl_write
- 接口概览：clk/rst(2), AXI-stream(20)
- 参数（节选）：`PORTS`, `SELECT_WIDTH`, `QUEUE_INDEX_WIDTH`, `S_REQ_TAG_WIDTH`, `M_REQ_TAG_WIDTH`, `CPL_SIZE`, `ARB_TYPE_ROUND_ROBIN`, `ARB_LSB_HIGH_PRIORITY`, `CL_PORTS`
- 内含模块（类型，节选）：`arbiter`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [cpl_queue_manager.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cpl_queue_manager.v)
- 定义模块：`cpl_queue_manager`
- 功能：完成队列管理：完成/事件队列状态存储与出入队
- 接口概览：clk/rst(2), AXI-stream(22), AXI-lite(19), cfg/status(1)
- 参数（节选）：`ADDR_WIDTH`, `REQ_TAG_WIDTH`, `OP_TABLE_SIZE`, `OP_TAG_WIDTH`, `QUEUE_INDEX_WIDTH`, `EVENT_WIDTH`, `QUEUE_PTR_WIDTH`, `LOG_QUEUE_SIZE_WIDTH`, `CPL_SIZE`, `PIPELINE` …(+8)
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [cpl_write.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/cpl_write.v)
- 定义模块：`cpl_write`
- 功能：完成/事件记录写回：入队并通过 DMA 写回主机内存
- 接口概览：clk/rst(2), AXI-stream(34), RAM(6), cfg/status(1)
- 参数（节选）：`PORTS`, `SELECT_WIDTH`, `SEG_COUNT`, `SEG_DATA_WIDTH`, `SEG_ADDR_WIDTH`, `SEG_BE_WIDTH`, `RAM_ADDR_WIDTH`, `RAM_PIPELINE`, `DMA_ADDR_WIDTH`, `DMA_LEN_WIDTH` …(+10)
- 内含模块（类型，节选）：`priority_encoder`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [desc_fetch.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/desc_fetch.v)
- 定义模块：`desc_fetch`
- 功能：描述符抓取：从队列管理器取指针并通过 DMA 读描述符
- 接口概览：clk/rst(2), AXI-stream(46), RAM(6), cfg/status(1)
- 参数（节选）：`PORTS`, `SELECT_WIDTH`, `SEG_COUNT`, `SEG_DATA_WIDTH`, `SEG_ADDR_WIDTH`, `SEG_BE_WIDTH`, `RAM_ADDR_WIDTH`, `RAM_PIPELINE`, `DMA_ADDR_WIDTH`, `DMA_LEN_WIDTH` …(+16)
- 内含模块（类型，节选）：`priority_encoder`, `dma_psdpram`, `dma_client_axis_source`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)

### [desc_op_mux.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/desc_op_mux.v)
- 定义模块：`desc_op_mux`
- 功能：描述符抓取操作复用：合并多个源到单个 desc_fetch
- 接口概览：clk/rst(2), AXI-stream(38)
- 参数（节选）：`PORTS`, `SELECT_WIDTH`, `QUEUE_INDEX_WIDTH`, `QUEUE_PTR_WIDTH`, `CPL_QUEUE_INDEX_WIDTH`, `S_REQ_TAG_WIDTH`, `M_REQ_TAG_WIDTH`, `AXIS_DATA_WIDTH`, `AXIS_KEEP_WIDTH`, `ARB_TYPE_ROUND_ROBIN` …(+2)
- 内含模块（类型，节选）：`arbiter`
- 被引用（文件，节选）：[fpga/common/rtl/mqnic_interface.v](file:///Users/sj/Documents/FPGA/corundum/fpga/common/rtl/mqnic_interface.v)
