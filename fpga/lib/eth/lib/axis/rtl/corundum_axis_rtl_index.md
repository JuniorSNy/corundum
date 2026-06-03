# Corundum `fpga/lib/axis/rtl` 索引

- 范围：仅覆盖 `fpga/lib/axis/rtl` 目录下 Verilog 文件（`.v`）
- 输出：每文件给出接口名聚类（按类似 `axi/axis` 的前缀+后缀风格启发式归类）、功能、输入输出摘要、内含模块、被引用（反向扫描）
- 说明：为导航用途做的启发式解析，generate/宏/多行实例化可能导致漏判或误判

## 目录
- [fpga/lib/axis/rtl/arbiter.v](#arbiterv)
- [fpga/lib/axis/rtl/axis_adapter.v](#axis-adapterv)
- [fpga/lib/axis/rtl/axis_arb_mux.v](#axis-arb-muxv)
- [fpga/lib/axis/rtl/axis_async_fifo.v](#axis-async-fifov)
- [fpga/lib/axis/rtl/axis_async_fifo_adapter.v](#axis-async-fifo-adapterv)
- [fpga/lib/axis/rtl/axis_broadcast.v](#axis-broadcastv)
- [fpga/lib/axis/rtl/axis_cobs_decode.v](#axis-cobs-decodev)
- [fpga/lib/axis/rtl/axis_cobs_encode.v](#axis-cobs-encodev)
- [fpga/lib/axis/rtl/axis_crosspoint.v](#axis-crosspointv)
- [fpga/lib/axis/rtl/axis_demux.v](#axis-demuxv)
- [fpga/lib/axis/rtl/axis_fifo.v](#axis-fifov)
- [fpga/lib/axis/rtl/axis_fifo_adapter.v](#axis-fifo-adapterv)
- [fpga/lib/axis/rtl/axis_frame_join.v](#axis-frame-joinv)
- [fpga/lib/axis/rtl/axis_frame_len.v](#axis-frame-lenv)
- [fpga/lib/axis/rtl/axis_frame_length_adjust.v](#axis-frame-length-adjustv)
- [fpga/lib/axis/rtl/axis_frame_length_adjust_fifo.v](#axis-frame-length-adjust-fifov)
- [fpga/lib/axis/rtl/axis_ll_bridge.v](#axis-ll-bridgev)
- [fpga/lib/axis/rtl/axis_mux.v](#axis-muxv)
- [fpga/lib/axis/rtl/axis_pipeline_fifo.v](#axis-pipeline-fifov)
- [fpga/lib/axis/rtl/axis_pipeline_register.v](#axis-pipeline-registerv)
- [fpga/lib/axis/rtl/axis_ram_switch.v](#axis-ram-switchv)
- [fpga/lib/axis/rtl/axis_rate_limit.v](#axis-rate-limitv)
- [fpga/lib/axis/rtl/axis_register.v](#axis-registerv)
- [fpga/lib/axis/rtl/axis_srl_fifo.v](#axis-srl-fifov)
- [fpga/lib/axis/rtl/axis_srl_register.v](#axis-srl-registerv)
- [fpga/lib/axis/rtl/axis_stat_counter.v](#axis-stat-counterv)
- [fpga/lib/axis/rtl/axis_switch.v](#axis-switchv)
- [fpga/lib/axis/rtl/axis_tap.v](#axis-tapv)
- [fpga/lib/axis/rtl/ll_axis_bridge.v](#ll-axis-bridgev)
- [fpga/lib/axis/rtl/priority_encoder.v](#priority-encoderv)
- [fpga/lib/axis/rtl/sync_reset.v](#sync-resetv)

## 逐文件索引

<a id="arbiterv"></a>
### fpga/lib/axis/rtl/arbiter.v
- 功能：Arbiter module
- 定义模块：`arbiter`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：`priority_encoder`
- 被引用（文件，节选）：fpga/common/rtl/cpl_op_mux.v, fpga/common/rtl/desc_op_mux.v, fpga/common/rtl/event_mux.v, fpga/common/rtl/tx_req_mux.v, fpga/lib/axi/rtl/arbiter.v, fpga/lib/axi/rtl/axi_cdma_desc_mux.v, fpga/lib/axi/rtl/axi_crossbar_rd.v, fpga/lib/axi/rtl/axi_crossbar_wr.v, fpga/lib/axi/rtl/axi_dma_desc_mux.v, fpga/lib/axi/rtl/axi_interconnect.v, fpga/lib/axi/rtl/axil_crossbar_rd.v, fpga/lib/axi/rtl/axil_crossbar_wr.v, fpga/lib/axi/rtl/axil_interconnect.v, fpga/lib/eth/lib/axis/rtl/arbiter.v, fpga/lib/eth/lib/axis/rtl/axis_arb_mux.v, fpga/lib/eth/lib/axis/rtl/axis_ram_switch.v (+12)

<a id="axis-adapterv"></a>
### fpga/lib/axis/rtl/axis_adapter.v
- 功能：AXI4-Stream bus width adapter
- 定义模块：`axis_adapter`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/example/VCU108/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/VCU118/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/VCU118/fpga_fmc_htg_6qsfp_25g/rtl/fpga_core.v, fpga/lib/eth/lib/axis/rtl/axis_adapter.v, fpga/lib/eth/lib/axis/rtl/axis_async_fifo_adapter.v, fpga/lib/eth/lib/axis/rtl/axis_fifo_adapter.v, fpga/lib/eth/lib/axis/rtl/axis_ram_switch.v, fpga/lib/eth/lib/axis/tb/test_axis_adapter_64_8.v, fpga/lib/eth/lib/axis/tb/test_axis_adapter_8_64.v

<a id="axis-arb-muxv"></a>
### fpga/lib/axis/rtl/axis_arb_mux.v
- 功能：AXI4-Stream arbitrated multiplexer
- 定义模块：`axis_arb_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：fpga/common/rtl/mqnic_core.v, fpga/common/rtl/mqnic_core_axi.v, fpga/common/rtl/mqnic_core_pcie.v, fpga/common/rtl/mqnic_interface.v, fpga/common/rtl/rx_fifo.v, fpga/lib/eth/lib/axis/rtl/axis_arb_mux.v, fpga/lib/eth/lib/axis/tb/test_axis_arb_mux_4.v, fpga/lib/eth/lib/axis/tb/test_axis_arb_mux_4_64.v, fpga/lib/pcie/example/ADM_PCIE_9V3/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU200/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU250/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU280/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU50/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/ExaNIC_X10/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/ExaNIC_X25/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/VCU108/fpga_axi/rtl/fpga_core.v (+5)

<a id="axis-async-fifov"></a>
### fpga/lib/axis/rtl/axis_async_fifo.v
- 功能：AXI4-Stream asynchronous FIFO
- 定义模块：`axis_async_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error, pause)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_port_tx.v, fpga/lib/eth/example/S10DX_DK/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/lib/axis/rtl/axis_async_fifo.v, fpga/lib/eth/lib/axis/rtl/axis_async_fifo_adapter.v, fpga/lib/eth/lib/axis/tb/test_axis_async_fifo.v, fpga/lib/eth/lib/axis/tb/test_axis_async_fifo_64.v, fpga/lib/eth/lib/axis/tb/test_axis_async_frame_fifo.v, fpga/lib/eth/lib/axis/tb/test_axis_async_frame_fifo_64.v, fpga/lib/eth/rtl/eth_mac_10g_fifo.v, fpga/lib/eth/rtl/eth_mac_phy_10g_fifo.v

<a id="axis-async-fifo-adapterv"></a>
### fpga/lib/axis/rtl/axis_async_fifo_adapter.v
- 功能：AXI4-Stream asynchronous FIFO with width converter
- 定义模块：`axis_async_fifo_adapter`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error)
- 内含模块（实例化类型，节选）：`axis_adapter`, `axis_async_fifo`
- 被引用（文件，节选）：fpga/common/rtl/mqnic_port_rx.v, fpga/common/rtl/mqnic_port_tx.v, fpga/lib/eth/lib/axis/rtl/axis_async_fifo_adapter.v, fpga/lib/eth/lib/axis/tb/test_axis_async_fifo_adapter_64_8.v, fpga/lib/eth/lib/axis/tb/test_axis_async_fifo_adapter_8_64.v, fpga/lib/eth/rtl/eth_mac_10g_fifo.v, fpga/lib/eth/rtl/eth_mac_1g_fifo.v, fpga/lib/eth/rtl/eth_mac_1g_gmii_fifo.v, fpga/lib/eth/rtl/eth_mac_1g_rgmii_fifo.v, fpga/lib/eth/rtl/eth_mac_mii_fifo.v, fpga/lib/eth/rtl/eth_mac_phy_10g_fifo.v

<a id="axis-broadcastv"></a>
### fpga/lib/axis/rtl/axis_broadcast.v
- 功能：AXI4-Stream broadcaster
- 定义模块：`axis_broadcast`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_broadcast.v, fpga/lib/eth/lib/axis/tb/test_axis_broadcast_4.v

<a id="axis-cobs-decodev"></a>
### fpga/lib/axis/rtl/axis_cobs_decode.v
- 功能：AXI4-Stream consistent overhead byte stuffing (COBS) decoder
- 定义模块：`axis_cobs_decode`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tvalid, tready, tlast, tuser); 输出 AXIS(m_axis: tdata, tvalid, tready, tlast, tuser)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/tb/test_axis_cobs_decode.v

<a id="axis-cobs-encodev"></a>
### fpga/lib/axis/rtl/axis_cobs_encode.v
- 功能：AXI4-Stream consistent overhead byte stuffing (COBS) encoder
- 定义模块：`axis_cobs_encode`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：`axis_fifo`
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_cobs_encode.v, fpga/lib/eth/lib/axis/tb/test_axis_cobs_encode.v, fpga/lib/eth/lib/axis/tb/test_axis_cobs_encode_zero_frame.v

<a id="axis-crosspointv"></a>
### fpga/lib/axis/rtl/axis_crosspoint.v
- 功能：AXI4-Stream crosspoint
- 定义模块：`axis_crosspoint`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tlast, tid, tdest, tuser); 控制/状态(select)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/example/HTG640/fpga_cxpt16/rtl/fpga_core.v, fpga/lib/eth/lib/axis/rtl/axis_crosspoint.v, fpga/lib/eth/lib/axis/tb/test_axis_crosspoint_4x4.v, fpga/lib/eth/lib/axis/tb/test_axis_crosspoint_4x4_64.v

<a id="axis-demuxv"></a>
### fpga/lib/axis/rtl/axis_demux.v
- 功能：AXI4-Stream demultiplexer
- 定义模块：`axis_demux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(drop, enable, error, select)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/tx_fifo.v, fpga/lib/eth/lib/axis/rtl/axis_demux.v, fpga/lib/eth/lib/axis/tb/test_axis_demux_4.v, fpga/lib/eth/lib/axis/tb/test_axis_demux_4_64.v

<a id="axis-fifov"></a>
### fpga/lib/axis/rtl/axis_fifo.v
- 功能：AXI4-Stream FIFO
- 定义模块：`axis_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error, pause)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_egress.v, fpga/common/rtl/mqnic_ingress.v, fpga/common/rtl/mqnic_interface.v, fpga/common/rtl/mqnic_interface_rx.v, fpga/common/rtl/mqnic_interface_tx.v, fpga/common/rtl/tx_checksum.v, fpga/common/rtl/tx_scheduler_rr.v, fpga/lib/eth/example/520N_MX/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/ATLYS/fpga/rtl/fpga_core.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/Arty/fpga/rtl/fpga_core.v, fpga/lib/eth/example/C10LP/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DCS7132LB/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/DE2-115/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DE5-Net/fpga/rtl/fpga_core.v (+40)

<a id="axis-fifo-adapterv"></a>
### fpga/lib/axis/rtl/axis_fifo_adapter.v
- 功能：AXI4-Stream FIFO with width converter
- 定义模块：`axis_fifo_adapter`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error)
- 内含模块（实例化类型，节选）：`axis_adapter`, `axis_fifo`
- 被引用（文件，节选）：fpga/common/rtl/rx_fifo.v, fpga/common/rtl/tx_fifo.v, fpga/lib/eth/lib/axis/rtl/axis_fifo_adapter.v, fpga/lib/eth/lib/axis/tb/test_axis_fifo_adapter_64_8.v, fpga/lib/eth/lib/axis/tb/test_axis_fifo_adapter_8_64.v

<a id="axis-frame-joinv"></a>
### fpga/lib/axis/rtl/axis_frame_join.v
- 功能：AXI4-Stream frame joiner
- 定义模块：`axis_frame_join`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tvalid, tready, tlast, tuser); 输出 AXIS(m_axis: tdata, tvalid, tready, tlast, tuser); 控制/状态(busy)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_frame_join.v, fpga/lib/eth/lib/axis/tb/test_axis_frame_join_4.v

<a id="axis-frame-lenv"></a>
### fpga/lib/axis/rtl/axis_frame_len.v
- 功能：AXI4-Stream frame length measurement
- 定义模块：`axis_frame_len`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_frame_len.v, fpga/lib/eth/lib/axis/tb/test_axis_frame_len_64.v, fpga/lib/eth/lib/axis/tb/test_axis_frame_len_8.v

<a id="axis-frame-length-adjustv"></a>
### fpga/lib/axis/rtl/axis_frame_length_adjust.v
- 功能：AXI4-Stream frame length adjuster
- 定义模块：`axis_frame_length_adjust`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_frame_length_adjust.v, fpga/lib/eth/lib/axis/rtl/axis_frame_length_adjust_fifo.v, fpga/lib/eth/lib/axis/tb/test_axis_frame_length_adjust_64.v, fpga/lib/eth/lib/axis/tb/test_axis_frame_length_adjust_8.v

<a id="axis-frame-length-adjust-fifov"></a>
### fpga/lib/axis/rtl/axis_frame_length_adjust_fifo.v
- 功能：AXI4-Stream frame length adjuster with FIFO
- 定义模块：`axis_frame_length_adjust_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：`axis_frame_length_adjust`, `axis_fifo`
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_frame_length_adjust_fifo.v, fpga/lib/eth/lib/axis/tb/test_axis_frame_length_adjust_fifo.v, fpga/lib/eth/lib/axis/tb/test_axis_frame_length_adjust_fifo_64.v

<a id="axis-ll-bridgev"></a>
### fpga/lib/axis/rtl/axis_ll_bridge.v
- 功能：AXI4-Stream to LocalLink bridge
- 定义模块：`axis_ll_bridge`
- 接口名（聚类）：AXIS(s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tvalid, tready, tlast)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_ll_bridge.v, fpga/lib/eth/lib/axis/tb/test_axis_ll_bridge.v

<a id="axis-muxv"></a>
### fpga/lib/axis/rtl/axis_mux.v
- 功能：AXI4-Stream multiplexer
- 定义模块：`axis_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(enable, select)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_mux.v, fpga/lib/eth/lib/axis/tb/test_axis_mux_4.v, fpga/lib/eth/lib/axis/tb/test_axis_mux_4_64.v

<a id="axis-pipeline-fifov"></a>
### fpga/lib/axis/rtl/axis_pipeline_fifo.v
- 功能：AXI4-Stream pipeline FIFO
- 定义模块：`axis_pipeline_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_port_rx.v, fpga/common/rtl/mqnic_port_tx.v, fpga/lib/eth/lib/axis/rtl/axis_pipeline_fifo.v

<a id="axis-pipeline-registerv"></a>
### fpga/lib/axis/rtl/axis_pipeline_register.v
- 功能：AXI4-Stream pipeline register
- 定义模块：`axis_pipeline_register`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：`axis_register`
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_pipeline_register.v

<a id="axis-ram-switchv"></a>
### fpga/lib/axis/rtl/axis_ram_switch.v
- 功能：AXI4-Stream RAM switch
- 定义模块：`axis_ram_switch`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error)
- 内含模块（实例化类型，节选）：`arbiter`, `axis_adapter`
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_ram_switch.v, fpga/lib/eth/lib/axis/tb/test_axis_ram_switch_1x4_256_64.v, fpga/lib/eth/lib/axis/tb/test_axis_ram_switch_4x1_64_256.v, fpga/lib/eth/lib/axis/tb/test_axis_ram_switch_4x4_64_64.v

<a id="axis-rate-limitv"></a>
### fpga/lib/axis/rtl/axis_rate_limit.v
- 功能：AXI4-Stream rate limiter
- 定义模块：`axis_rate_limit`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(pause)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_rate_limit.v, fpga/lib/eth/lib/axis/tb/test_axis_rate_limit.v, fpga/lib/eth/lib/axis/tb/test_axis_rate_limit_64.v

<a id="axis-registerv"></a>
### fpga/lib/axis/rtl/axis_register.v
- 功能：AXI4-Stream register
- 定义模块：`axis_register`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_pipeline_register.v, fpga/lib/eth/lib/axis/rtl/axis_register.v, fpga/lib/eth/lib/axis/rtl/axis_switch.v, fpga/lib/eth/lib/axis/tb/test_axis_register.v, fpga/lib/eth/lib/axis/tb/test_axis_register_64.v, fpga/lib/pcie/example/ADM_PCIE_9V3/fpga_axi/rtl/axis_register.v, fpga/lib/pcie/example/ADM_PCIE_9V3/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU200/fpga_axi/rtl/axis_register.v, fpga/lib/pcie/example/AU200/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU250/fpga_axi/rtl/axis_register.v, fpga/lib/pcie/example/AU250/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU280/fpga_axi/rtl/axis_register.v, fpga/lib/pcie/example/AU280/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU50/fpga_axi/rtl/axis_register.v, fpga/lib/pcie/example/AU50/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/ExaNIC_X10/fpga_axi/rtl/axis_register.v (+13)

<a id="axis-srl-fifov"></a>
### fpga/lib/axis/rtl/axis_srl_fifo.v
- 功能：AXI4-Stream SRL-based FIFO
- 定义模块：`axis_srl_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_srl_fifo.v, fpga/lib/eth/lib/axis/tb/test_axis_srl_fifo.v, fpga/lib/eth/lib/axis/tb/test_axis_srl_fifo_64.v

<a id="axis-srl-registerv"></a>
### fpga/lib/axis/rtl/axis_srl_register.v
- 功能：AXI4-Stream SRL-based FIFO register
- 定义模块：`axis_srl_register`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_srl_register.v, fpga/lib/eth/lib/axis/tb/test_axis_srl_register.v, fpga/lib/eth/lib/axis/tb/test_axis_srl_register_64.v

<a id="axis-stat-counterv"></a>
### fpga/lib/axis/rtl/axis_stat_counter.v
- 功能：AXI4-Stream statistics counter
- 定义模块：`axis_stat_counter`
- 接口名（聚类）：AXIS(m_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输出 AXIS(m_axis: tdata, tvalid, tready, tlast, tuser); 控制/状态(busy)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_stat_counter.v, fpga/lib/eth/lib/axis/tb/test_axis_stat_counter.v

<a id="axis-switchv"></a>
### fpga/lib/axis/rtl/axis_switch.v
- 功能：AXI4-Stream switch
- 定义模块：`axis_switch`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输入 AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); 控制/状态(error)
- 内含模块（实例化类型，节选）：`axis_register`, `arbiter`
- 被引用（文件，节选）：fpga/lib/eth/example/VCU108/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/VCU118/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/VCU118/fpga_fmc_htg_6qsfp_25g/rtl/fpga_core.v, fpga/lib/eth/lib/axis/rtl/axis_switch.v, fpga/lib/eth/lib/axis/tb/test_axis_switch_4x4.v, fpga/lib/eth/lib/axis/tb/test_axis_switch_4x4_64.v

<a id="axis-tapv"></a>
### fpga/lib/axis/rtl/axis_tap.v
- 功能：AXI4-Stream tap
- 定义模块：`axis_tap`
- 接口名（聚类）：AXIS(m_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输出 AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/axis_tap.v, fpga/lib/eth/lib/axis/tb/test_axis_tap.v, fpga/lib/eth/lib/axis/tb/test_axis_tap_64.v

<a id="ll-axis-bridgev"></a>
### fpga/lib/axis/rtl/ll_axis_bridge.v
- 功能：LocalLink to AXI4-Stream bridge
- 定义模块：`ll_axis_bridge`
- 接口名（聚类）：AXIS(m_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); 输出 AXIS(m_axis: tdata, tvalid, tready, tlast)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/lib/axis/rtl/ll_axis_bridge.v, fpga/lib/eth/lib/axis/tb/test_ll_axis_bridge.v

<a id="priority-encoderv"></a>
### fpga/lib/axis/rtl/priority_encoder.v
- 功能：Priority encoder module
- 定义模块：`priority_encoder`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：（未识别到明显输入输出接口）
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/cpl_write.v, fpga/common/rtl/desc_fetch.v, fpga/lib/axi/rtl/arbiter.v, fpga/lib/axi/rtl/priority_encoder.v, fpga/lib/eth/lib/axis/rtl/arbiter.v, fpga/lib/eth/lib/axis/rtl/priority_encoder.v, fpga/lib/eth/lib/axis/tb/test_priority_encoder.v, fpga/lib/pcie/rtl/arbiter.v, fpga/lib/pcie/rtl/priority_encoder.v

<a id="sync-resetv"></a>
### fpga/lib/axis/rtl/sync_reset.v
- 功能：Synchronizes an active-high asynchronous reset signal to a given clock by
- 定义模块：`sync_reset`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/cmac_gty_ch_wrapper.v, fpga/common/rtl/cmac_gty_wrapper.v, fpga/common/rtl/eth_xcvr_phy_10g_gty_wrapper.v, fpga/lib/eth/example/520N_MX/fpga_10g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/520N_MX/fpga_10g/rtl/fpga.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/fpga.v, fpga/lib/eth/example/ATLYS/fpga/rtl/fpga.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_au200.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_au280.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_au50.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_au55.v, fpga/lib/eth/example/Arty/fpga/rtl/fpga.v, fpga/lib/eth/example/C10LP/fpga/rtl/fpga.v, fpga/lib/eth/example/DCS7132LB/fpga_25g/rtl/eth_xcvr_phy_wrapper.v (+121)
