# Corundum `fpga/lib/pcie/rtl` 索引

- 范围：仅覆盖 `fpga/lib/pcie/rtl` 目录下 Verilog 文件（`.v`）
- 输出：每文件给出接口名聚类（按类似 `axi/axis` 的前缀+后缀风格启发式归类）、功能、输入输出摘要、内含模块（实例化类型）
- 说明：为导航用途做的启发式解析，generate/宏/多行实例化可能导致漏判或误判

## 目录
- [fpga/lib/pcie/rtl/arbiter.v](#arbiterv)
- [fpga/lib/pcie/rtl/axis_arb_mux.v](#axis-arb-muxv)
- [fpga/lib/pcie/rtl/dma_client_axis_sink.v](#dma-client-axis-sinkv)
- [fpga/lib/pcie/rtl/dma_client_axis_source.v](#dma-client-axis-sourcev)
- [fpga/lib/pcie/rtl/dma_if_axi.v](#dma-if-axiv)
- [fpga/lib/pcie/rtl/dma_if_axi_rd.v](#dma-if-axi-rdv)
- [fpga/lib/pcie/rtl/dma_if_axi_wr.v](#dma-if-axi-wrv)
- [fpga/lib/pcie/rtl/dma_if_desc_mux.v](#dma-if-desc-muxv)
- [fpga/lib/pcie/rtl/dma_if_mux.v](#dma-if-muxv)
- [fpga/lib/pcie/rtl/dma_if_mux_rd.v](#dma-if-mux-rdv)
- [fpga/lib/pcie/rtl/dma_if_mux_wr.v](#dma-if-mux-wrv)
- [fpga/lib/pcie/rtl/dma_if_pcie.v](#dma-if-pciev)
- [fpga/lib/pcie/rtl/dma_if_pcie_rd.v](#dma-if-pcie-rdv)
- [fpga/lib/pcie/rtl/dma_if_pcie_us.v](#dma-if-pcie-usv)
- [fpga/lib/pcie/rtl/dma_if_pcie_us_rd.v](#dma-if-pcie-us-rdv)
- [fpga/lib/pcie/rtl/dma_if_pcie_us_wr.v](#dma-if-pcie-us-wrv)
- [fpga/lib/pcie/rtl/dma_if_pcie_wr.v](#dma-if-pcie-wrv)
- [fpga/lib/pcie/rtl/dma_psdpram.v](#dma-psdpramv)
- [fpga/lib/pcie/rtl/dma_psdpram_async.v](#dma-psdpram-asyncv)
- [fpga/lib/pcie/rtl/dma_ram_demux.v](#dma-ram-demuxv)
- [fpga/lib/pcie/rtl/dma_ram_demux_rd.v](#dma-ram-demux-rdv)
- [fpga/lib/pcie/rtl/dma_ram_demux_wr.v](#dma-ram-demux-wrv)
- [fpga/lib/pcie/rtl/irq_rate_limit.v](#irq-rate-limitv)
- [fpga/lib/pcie/rtl/pcie_axi_dma_desc_mux.v](#pcie-axi-dma-desc-muxv)
- [fpga/lib/pcie/rtl/pcie_axi_master.v](#pcie-axi-masterv)
- [fpga/lib/pcie/rtl/pcie_axi_master_rd.v](#pcie-axi-master-rdv)
- [fpga/lib/pcie/rtl/pcie_axi_master_wr.v](#pcie-axi-master-wrv)
- [fpga/lib/pcie/rtl/pcie_axil_master.v](#pcie-axil-masterv)
- [fpga/lib/pcie/rtl/pcie_axil_master_minimal.v](#pcie-axil-master-minimalv)
- [fpga/lib/pcie/rtl/pcie_msix.v](#pcie-msixv)
- [fpga/lib/pcie/rtl/pcie_ptile_cfg.v](#pcie-ptile-cfgv)
- [fpga/lib/pcie/rtl/pcie_ptile_fc_counter.v](#pcie-ptile-fc-counterv)
- [fpga/lib/pcie/rtl/pcie_ptile_if.v](#pcie-ptile-ifv)
- [fpga/lib/pcie/rtl/pcie_ptile_if_rx.v](#pcie-ptile-if-rxv)
- [fpga/lib/pcie/rtl/pcie_ptile_if_tx.v](#pcie-ptile-if-txv)
- [fpga/lib/pcie/rtl/pcie_s10_cfg.v](#pcie-s10-cfgv)
- [fpga/lib/pcie/rtl/pcie_s10_if.v](#pcie-s10-ifv)
- [fpga/lib/pcie/rtl/pcie_s10_if_rx.v](#pcie-s10-if-rxv)
- [fpga/lib/pcie/rtl/pcie_s10_if_tx.v](#pcie-s10-if-txv)
- [fpga/lib/pcie/rtl/pcie_s10_msi.v](#pcie-s10-msiv)
- [fpga/lib/pcie/rtl/pcie_tlp_demux.v](#pcie-tlp-demuxv)
- [fpga/lib/pcie/rtl/pcie_tlp_demux_bar.v](#pcie-tlp-demux-barv)
- [fpga/lib/pcie/rtl/pcie_tlp_fc_count.v](#pcie-tlp-fc-countv)
- [fpga/lib/pcie/rtl/pcie_tlp_fifo.v](#pcie-tlp-fifov)
- [fpga/lib/pcie/rtl/pcie_tlp_fifo_mux.v](#pcie-tlp-fifo-muxv)
- [fpga/lib/pcie/rtl/pcie_tlp_fifo_raw.v](#pcie-tlp-fifo-rawv)
- [fpga/lib/pcie/rtl/pcie_tlp_mux.v](#pcie-tlp-muxv)
- [fpga/lib/pcie/rtl/pcie_us_axi_dma.v](#pcie-us-axi-dmav)
- [fpga/lib/pcie/rtl/pcie_us_axi_dma_rd.v](#pcie-us-axi-dma-rdv)
- [fpga/lib/pcie/rtl/pcie_us_axi_dma_wr.v](#pcie-us-axi-dma-wrv)
- [fpga/lib/pcie/rtl/pcie_us_axi_master.v](#pcie-us-axi-masterv)
- [fpga/lib/pcie/rtl/pcie_us_axi_master_rd.v](#pcie-us-axi-master-rdv)
- [fpga/lib/pcie/rtl/pcie_us_axi_master_wr.v](#pcie-us-axi-master-wrv)
- [fpga/lib/pcie/rtl/pcie_us_axil_master.v](#pcie-us-axil-masterv)
- [fpga/lib/pcie/rtl/pcie_us_axis_cq_demux.v](#pcie-us-axis-cq-demuxv)
- [fpga/lib/pcie/rtl/pcie_us_axis_rc_demux.v](#pcie-us-axis-rc-demuxv)
- [fpga/lib/pcie/rtl/pcie_us_cfg.v](#pcie-us-cfgv)
- [fpga/lib/pcie/rtl/pcie_us_if.v](#pcie-us-ifv)
- [fpga/lib/pcie/rtl/pcie_us_if_cc.v](#pcie-us-if-ccv)
- [fpga/lib/pcie/rtl/pcie_us_if_cq.v](#pcie-us-if-cqv)
- [fpga/lib/pcie/rtl/pcie_us_if_rc.v](#pcie-us-if-rcv)
- [fpga/lib/pcie/rtl/pcie_us_if_rq.v](#pcie-us-if-rqv)
- [fpga/lib/pcie/rtl/pcie_us_msi.v](#pcie-us-msiv)
- [fpga/lib/pcie/rtl/priority_encoder.v](#priority-encoderv)
- [fpga/lib/pcie/rtl/pulse_merge.v](#pulse-mergev)

## 逐文件索引

<a id="arbiterv"></a>
### fpga/lib/pcie/rtl/arbiter.v
- 功能：Arbiter module
- 定义模块：`arbiter`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：`priority_encoder`

<a id="axis-arb-muxv"></a>
### fpga/lib/pcie/rtl/axis_arb_mux.v
- 功能：AXI4-Stream arbitrated multiplexer
- 定义模块：`axis_arb_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser); AXIS(m_axis: tdata, tkeep, tvalid, tready, tlast, tid, tdest, tuser)
- 内含模块（实例化类型，节选）：`arbiter`

<a id="dma-client-axis-sinkv"></a>
### fpga/lib/pcie/rtl/dma_client_axis_sink.v
- 功能：AXI stream sink DMA client
- 定义模块：`dma_client_axis_sink`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-client-axis-sourcev"></a>
### fpga/lib/pcie/rtl/dma_client_axis_source.v
- 功能：AXI stream source DMA client
- 定义模块：`dma_client_axis_source`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-if-axiv"></a>
### fpga/lib/pcie/rtl/dma_if_axi.v
- 功能：AXI DMA interface
- 定义模块：`dma_if_axi`
- 接口名（聚类）：AXIS(m_axis, s_axis); AXI(m_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); AXI(s_axi/m_axi); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`dma_if_axi_rd`, `dma_if_axi_wr`

<a id="dma-if-axi-rdv"></a>
### fpga/lib/pcie/rtl/dma_if_axi_rd.v
- 功能：AXI DMA read interface
- 定义模块：`dma_if_axi_rd`
- 接口名（聚类）：AXIS(m_axis, s_axis); AXI(m_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); AXI(s_axi/m_axi); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-if-axi-wrv"></a>
### fpga/lib/pcie/rtl/dma_if_axi_wr.v
- 功能：AXI DMA write interface
- 定义模块：`dma_if_axi_wr`
- 接口名（聚类）：AXIS(m_axis, s_axis); AXI(m_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); AXI(s_axi/m_axi); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`endcase`

<a id="dma-if-desc-muxv"></a>
### fpga/lib/pcie/rtl/dma_if_desc_mux.v
- 功能：DMA interface descriptor mux
- 定义模块：`dma_if_desc_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`arbiter`

<a id="dma-if-muxv"></a>
### fpga/lib/pcie/rtl/dma_if_mux.v
- 功能：DMA interface mux
- 定义模块：`dma_if_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`dma_if_mux_rd`, `dma_if_mux_wr`

<a id="dma-if-mux-rdv"></a>
### fpga/lib/pcie/rtl/dma_if_mux_rd.v
- 功能：DMA interface mux (read)
- 定义模块：`dma_if_mux_rd`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(dma_ram, ram)
- 内含模块（实例化类型，节选）：`dma_if_desc_mux`, `dma_ram_demux_wr`

<a id="dma-if-mux-wrv"></a>
### fpga/lib/pcie/rtl/dma_if_mux_wr.v
- 功能：DMA interface mux (write)
- 定义模块：`dma_if_mux_wr`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(dma_ram, ram)
- 内含模块（实例化类型，节选）：`dma_if_desc_mux`, `dma_ram_demux_rd`

<a id="dma-if-pciev"></a>
### fpga/lib/pcie/rtl/dma_if_pcie.v
- 功能：PCIe DMA interface
- 定义模块：`dma_if_pcie`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`dma_if_pcie_rd`, `dma_if_pcie_wr`

<a id="dma-if-pcie-rdv"></a>
### fpga/lib/pcie/rtl/dma_if_pcie_rd.v
- 功能：PCIe DMA read interface
- 定义模块：`dma_if_pcie_rd`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-if-pcie-usv"></a>
### fpga/lib/pcie/rtl/dma_if_pcie_us.v
- 功能：Ultrascale PCIe DMA interface
- 定义模块：`dma_if_pcie_us`
- 接口名（聚类）：PCIE-AXIS(m_axis_rq, s_axis_rc, s_axis_rq); AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(s_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`dma_if_pcie_us_rd`, `dma_if_pcie_us_wr`

<a id="dma-if-pcie-us-rdv"></a>
### fpga/lib/pcie/rtl/dma_if_pcie_us_rd.v
- 功能：Ultrascale PCIe DMA read interface
- 定义模块：`dma_if_pcie_us_rd`
- 接口名（聚类）：PCIE-AXIS(m_axis_rq, s_axis_rc, s_axis_rq); AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: signals); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(s_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-if-pcie-us-wrv"></a>
### fpga/lib/pcie/rtl/dma_if_pcie_us_wr.v
- 功能：Ultrascale PCIe DMA write interface
- 定义模块：`dma_if_pcie_us_wr`
- 接口名（聚类）：PCIE-AXIS(m_axis_rq, s_axis_rq); AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`endcase`

<a id="dma-if-pcie-wrv"></a>
### fpga/lib/pcie/rtl/dma_if_pcie_wr.v
- 功能：PCIe DMA write interface
- 定义模块：`dma_if_pcie_wr`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`endcase`

<a id="dma-psdpramv"></a>
### fpga/lib/pcie/rtl/dma_psdpram.v
- 功能：DMA parallel simple dual port RAM
- 定义模块：`dma_psdpram`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-psdpram-asyncv"></a>
### fpga/lib/pcie/rtl/dma_psdpram_async.v
- 功能：DMA parallel simple dual port RAM (asynchronous)
- 定义模块：`dma_psdpram_async`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-ram-demuxv"></a>
### fpga/lib/pcie/rtl/dma_ram_demux.v
- 功能：DMA RAM demux
- 定义模块：`dma_ram_demux`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst); RAM/DMARAM(dma_ram, ram)
- 内含模块（实例化类型，节选）：`dma_ram_demux_wr`, `dma_ram_demux_rd`

<a id="dma-ram-demux-rdv"></a>
### fpga/lib/pcie/rtl/dma_ram_demux_rd.v
- 功能：DMA RAM demux (read)
- 定义模块：`dma_ram_demux_rd`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst); RAM/DMARAM(dma_ram, ram)
- 内含模块（实例化类型，节选）：（无）

<a id="dma-ram-demux-wrv"></a>
### fpga/lib/pcie/rtl/dma_ram_demux_wr.v
- 功能：DMA RAM demux (write)
- 定义模块：`dma_ram_demux_wr`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst); RAM/DMARAM(dma_ram, ram)
- 内含模块（实例化类型，节选）：（无）

<a id="irq-rate-limitv"></a>
### fpga/lib/pcie/rtl/irq_rate_limit.v
- 功能：IRQ rate limit module
- 定义模块：`irq_rate_limit`
- 接口名（聚类）：IRQ(irq)
- 输入输出（摘要）：时钟/复位(clk, rst); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-axi-dma-desc-muxv"></a>
### fpga/lib/pcie/rtl/pcie_axi_dma_desc_mux.v
- 功能：PCIe AXI DMA descriptor mux
- 定义模块：`pcie_axi_dma_desc_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(s_axis: signals); AXIS(m_axis: signals)
- 内含模块（实例化类型，节选）：`arbiter`

<a id="pcie-axi-masterv"></a>
### fpga/lib/pcie/rtl/pcie_axi_master.v
- 功能：PCIe AXI Master
- 定义模块：`pcie_axi_master`
- 接口名（聚类）：AXI(m_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXI(s_axi/m_axi)
- 内含模块（实例化类型，节选）：`pcie_tlp_demux`, `pcie_axi_master_rd`, `pcie_axi_master_wr`, `pulse_merge`

<a id="pcie-axi-master-rdv"></a>
### fpga/lib/pcie/rtl/pcie_axi_master_rd.v
- 功能：PCIe AXI Master (read)
- 定义模块：`pcie_axi_master_rd`
- 接口名（聚类）：AXI(m_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXI(s_axi/m_axi)
- 内含模块（实例化类型，节选）：`endcase`

<a id="pcie-axi-master-wrv"></a>
### fpga/lib/pcie/rtl/pcie_axi_master_wr.v
- 功能：PCIe AXI Master (write)
- 定义模块：`pcie_axi_master_wr`
- 接口名（聚类）：AXI(m_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXI(s_axi/m_axi); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-axil-masterv"></a>
### fpga/lib/pcie/rtl/pcie_axil_master.v
- 功能：PCIe AXI Lite Master
- 定义模块：`pcie_axil_master`
- 接口名（聚类）：AXIL(m_axil)
- 输入输出（摘要）：时钟/复位(clk, rst); AXI-Lite(s_axil/m_axil); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`endcase`

<a id="pcie-axil-master-minimalv"></a>
### fpga/lib/pcie/rtl/pcie_axil_master_minimal.v
- 功能：PCIe AXI Lite Master (minimal version, supports only aligned, 1 DW operations)
- 定义模块：`pcie_axil_master_minimal`
- 接口名（聚类）：AXIL(m_axil)
- 输入输出（摘要）：时钟/复位(clk, rst); AXI-Lite(s_axil/m_axil); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：`endcase`

<a id="pcie-msixv"></a>
### fpga/lib/pcie/rtl/pcie_msix.v
- 功能：PCIe MSI-X module
- 定义模块：`pcie_msix`
- 接口名（聚类）：AXIL(s_axil); IRQ(irq, msix)
- 输入输出（摘要）：时钟/复位(clk, rst); AXI-Lite(s_axil/m_axil); IRQ(msi/msix/irq); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-ptile-cfgv"></a>
### fpga/lib/pcie/rtl/pcie_ptile_cfg.v
- 功能：P-Tile PCIe configuration interface
- 定义模块：`pcie_ptile_cfg`
- 接口名（聚类）：CFG(cfg, cfg_fc); IRQ(irq, msi, msix)
- 输入输出（摘要）：时钟/复位(clk, rst); CFG(cfg/cfg_mgmt); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-ptile-fc-counterv"></a>
### fpga/lib/pcie/rtl/pcie_ptile_fc_counter.v
- 功能：P-Tile PCIe flow control counter
- 定义模块：`pcie_ptile_fc_counter`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-ptile-ifv"></a>
### fpga/lib/pcie/rtl/pcie_ptile_if.v
- 功能：P-Tile PCIe interface adapter
- 定义模块：`pcie_ptile_if`
- 接口名（聚类）：AXIS(m_axis); ST(rx_st, tx_st); CFG(cfg, cfg_fc); IRQ(irq, msi, msix)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(m_axis: signals); ST(rx_st, tx_st); CFG(cfg/cfg_mgmt); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：`pcie_ptile_if_rx`, `pcie_ptile_if_tx`, `pcie_ptile_cfg`

<a id="pcie-ptile-if-rxv"></a>
### fpga/lib/pcie/rtl/pcie_ptile_if_rx.v
- 功能：P-Tile PCIe interface adapter (receive)
- 定义模块：`pcie_ptile_if_rx`
- 接口名（聚类）：ST(rx_st)
- 输入输出（摘要）：时钟/复位(clk, rst); ST(rx_st)
- 内含模块（实例化类型，节选）：`pcie_tlp_demux`

<a id="pcie-ptile-if-txv"></a>
### fpga/lib/pcie/rtl/pcie_ptile_if_tx.v
- 功能：P-Tile PCIe interface adapter (transmit)
- 定义模块：`pcie_ptile_if_tx`
- 接口名（聚类）：AXIS(m_axis); ST(tx_st); IRQ(msi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(m_axis: signals); ST(tx_st); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo_mux`, `pcie_ptile_fc_counter`

<a id="pcie-s10-cfgv"></a>
### fpga/lib/pcie/rtl/pcie_s10_cfg.v
- 功能：Intel Stratix 10 H-Tile/L-Tile PCIe configuration interface
- 定义模块：`pcie_s10_cfg`
- 接口名（聚类）：CFG(cfg); IRQ(irq, msi, msix)
- 输入输出（摘要）：时钟/复位(clk, rst); CFG(cfg/cfg_mgmt); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-s10-ifv"></a>
### fpga/lib/pcie/rtl/pcie_s10_if.v
- 功能：Intel Stratix 10 H-Tile/L-Tile PCIe interface adapter
- 定义模块：`pcie_s10_if`
- 接口名（聚类）：AXIS(m_axis); ST(rx_st, tx_st); CFG(cfg); IRQ(irq, msi, msix)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(m_axis: signals); ST(rx_st, tx_st); CFG(cfg/cfg_mgmt); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：`pcie_s10_if_rx`, `pcie_s10_if_tx`, `pcie_s10_cfg`, `pcie_s10_msi`

<a id="pcie-s10-if-rxv"></a>
### fpga/lib/pcie/rtl/pcie_s10_if_rx.v
- 功能：Intel Stratix 10 H-Tile/L-Tile PCIe interface adapter (receive)
- 定义模块：`pcie_s10_if_rx`
- 接口名（聚类）：ST(rx_st)
- 输入输出（摘要）：时钟/复位(clk, rst); ST(rx_st)
- 内含模块（实例化类型，节选）：`pcie_tlp_demux`

<a id="pcie-s10-if-txv"></a>
### fpga/lib/pcie/rtl/pcie_s10_if_tx.v
- 功能：Intel Stratix 10 H-Tile/L-Tile PCIe interface adapter (transmit)
- 定义模块：`pcie_s10_if_tx`
- 接口名（聚类）：AXIS(m_axis); ST(tx_st); IRQ(msi)
- 输入输出（摘要）：时钟/复位(clk, rst); AXIS(m_axis: signals); ST(tx_st); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo_mux`, `pcie_tlp_fifo_raw`, `pcie_tlp_fc_count`

<a id="pcie-s10-msiv"></a>
### fpga/lib/pcie/rtl/pcie_s10_msi.v
- 功能：Intel Stratix 10 H-Tile/L-Tile PCIe MSI shim
- 定义模块：`pcie_s10_msi`
- 接口名（聚类）：CFG(cfg); IRQ(irq, msi)
- 输入输出（摘要）：时钟/复位(clk, rst); CFG(cfg/cfg_mgmt); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：`arbiter`

<a id="pcie-tlp-demuxv"></a>
### fpga/lib/pcie/rtl/pcie_tlp_demux.v
- 功能：PCIe TLP demultiplexer
- 定义模块：`pcie_tlp_demux`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo`

<a id="pcie-tlp-demux-barv"></a>
### fpga/lib/pcie/rtl/pcie_tlp_demux_bar.v
- 功能：PCIe TLP demultiplexer (BAR ID)
- 定义模块：`pcie_tlp_demux_bar`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：`pcie_tlp_demux`

<a id="pcie-tlp-fc-countv"></a>
### fpga/lib/pcie/rtl/pcie_tlp_fc_count.v
- 功能：PCIe TLP flow control credit counter
- 定义模块：`pcie_tlp_fc_count`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-tlp-fifov"></a>
### fpga/lib/pcie/rtl/pcie_tlp_fifo.v
- 功能：PCIe TLP FIFO
- 定义模块：`pcie_tlp_fifo`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo_raw`

<a id="pcie-tlp-fifo-muxv"></a>
### fpga/lib/pcie/rtl/pcie_tlp_fifo_mux.v
- 功能：PCIe TLP multiplexer with input FIFOs
- 定义模块：`pcie_tlp_fifo_mux`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo_raw`, `pcie_tlp_fc_count`

<a id="pcie-tlp-fifo-rawv"></a>
### fpga/lib/pcie/rtl/pcie_tlp_fifo_raw.v
- 功能：PCIe TLP FIFO (raw output)
- 定义模块：`pcie_tlp_fifo_raw`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-tlp-muxv"></a>
### fpga/lib/pcie/rtl/pcie_tlp_mux.v
- 功能：PCIe TLP multiplexer
- 定义模块：`pcie_tlp_mux`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-axi-dmav"></a>
### fpga/lib/pcie/rtl/pcie_us_axi_dma.v
- 功能：Ultrascale PCIe AXI DMA
- 定义模块：`pcie_us_axi_dma`
- 接口名（聚类）：PCIE-AXIS(m_axis_rq, s_axis_rc, s_axis_rq); AXIS(m_axis, s_axis); AXI(m_axi, s_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(s_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser); AXI(s_axi/m_axi)
- 内含模块（实例化类型，节选）：`pcie_us_axi_dma_rd`, `pcie_us_axi_dma_wr`

<a id="pcie-us-axi-dma-rdv"></a>
### fpga/lib/pcie/rtl/pcie_us_axi_dma_rd.v
- 功能：Ultrascale PCIe AXI DMA Read
- 定义模块：`pcie_us_axi_dma_rd`
- 接口名（聚类）：PCIE-AXIS(m_axis_rq, s_axis_rc, s_axis_rq); AXIS(m_axis, s_axis); AXI(m_axi, s_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: signals); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(s_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser); AXI(s_axi/m_axi); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-axi-dma-wrv"></a>
### fpga/lib/pcie/rtl/pcie_us_axi_dma_wr.v
- 功能：Ultrascale PCIe AXI DMA Write
- 定义模块：`pcie_us_axi_dma_wr`
- 接口名（聚类）：PCIE-AXIS(m_axis_rq, s_axis_rq); AXIS(m_axis, s_axis); AXI(m_axi, s_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); AXI(s_axi/m_axi); RAM/DMARAM(ram)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-axi-masterv"></a>
### fpga/lib/pcie/rtl/pcie_us_axi_master.v
- 功能：Ultrascale PCIe AXI Master
- 定义模块：`pcie_us_axi_master`
- 接口名（聚类）：PCIE-AXIS(m_axis_cc, m_axis_cq, s_axis_cq); AXIS(m_axis, s_axis); AXI(m_axi, s_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_cc: tdata, tkeep, tvalid, tready, tlast, tuser); AXI(s_axi/m_axi)
- 内含模块（实例化类型，节选）：`pcie_us_axis_cq_demux`, `pcie_us_axi_master_rd`, `pcie_us_axi_master_wr`, `pulse_merge`

<a id="pcie-us-axi-master-rdv"></a>
### fpga/lib/pcie/rtl/pcie_us_axi_master_rd.v
- 功能：Ultrascale PCIe AXI Master (read)
- 定义模块：`pcie_us_axi_master_rd`
- 接口名（聚类）：PCIE-AXIS(m_axis_cc, s_axis_cq); AXIS(m_axis, s_axis); AXI(m_axi, s_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_cc: tdata, tkeep, tvalid, tready, tlast, tuser); AXI(s_axi/m_axi)
- 内含模块（实例化类型，节选）：`endcase`

<a id="pcie-us-axi-master-wrv"></a>
### fpga/lib/pcie/rtl/pcie_us_axi_master_wr.v
- 功能：Ultrascale PCIe AXI Master (write)
- 定义模块：`pcie_us_axi_master_wr`
- 接口名（聚类）：PCIE-AXIS(s_axis_cq); AXIS(s_axis); AXI(m_axi, s_axi)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser); AXI(s_axi/m_axi)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-axil-masterv"></a>
### fpga/lib/pcie/rtl/pcie_us_axil_master.v
- 功能：Ultrascale PCIe AXI Lite Master
- 定义模块：`pcie_us_axil_master`
- 接口名（聚类）：PCIE-AXIS(m_axis_cc, s_axis_cq); AXIS(m_axis, s_axis); AXIL(m_axil, s_axil)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_cc: tdata, tkeep, tvalid, tready, tlast, tuser); AXI-Lite(s_axil/m_axil)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-axis-cq-demuxv"></a>
### fpga/lib/pcie/rtl/pcie_us_axis_cq_demux.v
- 功能：Ultrascale PCIe CQ demultiplexer
- 定义模块：`pcie_us_axis_cq_demux`
- 接口名（聚类）：PCIE-AXIS(m_axis_cq, s_axis_cq); AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-axis-rc-demuxv"></a>
### fpga/lib/pcie/rtl/pcie_us_axis_rc_demux.v
- 功能：Ultrascale PCIe RC demultiplexer
- 定义模块：`pcie_us_axis_rc_demux`
- 接口名（聚类）：PCIE-AXIS(m_axis_rc, s_axis_rc); AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-cfgv"></a>
### fpga/lib/pcie/rtl/pcie_us_cfg.v
- 功能：Ultrascale PCIe configuration shim
- 定义模块：`pcie_us_cfg`
- 接口名（聚类）：CFG(cfg, cfg_mgmt)
- 输入输出（摘要）：时钟/复位(clk, rst); CFG(cfg/cfg_mgmt)
- 内含模块（实例化类型，节选）：（无）

<a id="pcie-us-ifv"></a>
### fpga/lib/pcie/rtl/pcie_us_if.v
- 功能：Xilinx UltraScale PCIe interface adapter
- 定义模块：`pcie_us_if`
- 接口名（聚类）：PCIE-AXIS(m_axis_cc, m_axis_rq, s_axis_cq, s_axis_rc, s_axis_rq); AXIS(m_axis, s_axis); CFG(cfg, cfg_fc, cfg_mgmt); IRQ(msi, msix)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: signals); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(s_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(s_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser); PCIE-AXIS(m_axis_cc: tdata, tkeep, tvalid, tready, tlast, tuser); CFG(cfg/cfg_mgmt); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：`pcie_us_if_rc`, `pcie_us_if_rq`, `pcie_us_if_cq`, `pcie_us_if_cc`, `pcie_us_cfg`, `pcie_us_msi`

<a id="pcie-us-if-ccv"></a>
### fpga/lib/pcie/rtl/pcie_us_if_cc.v
- 功能：Xilinx UltraScale PCIe interface adapter (Completer Completion)
- 定义模块：`pcie_us_if_cc`
- 接口名（聚类）：PCIE-AXIS(m_axis_cc); AXIS(m_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(m_axis_cc: tdata, tkeep, tvalid, tready, tlast, tuser)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo_raw`, `endcase`

<a id="pcie-us-if-cqv"></a>
### fpga/lib/pcie/rtl/pcie_us_if_cq.v
- 功能：Xilinx UltraScale PCIe interface adapter (Completer reQuest)
- 定义模块：`pcie_us_if_cq`
- 接口名（聚类）：PCIE-AXIS(s_axis_cq); AXIS(s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_cq: tdata, tkeep, tvalid, tready, tlast, tuser)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo`

<a id="pcie-us-if-rcv"></a>
### fpga/lib/pcie/rtl/pcie_us_if_rc.v
- 功能：Xilinx UltraScale PCIe interface adapter (Requester Completion)
- 定义模块：`pcie_us_if_rc`
- 接口名（聚类）：PCIE-AXIS(s_axis_rc); AXIS(s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rc: tdata, tkeep, tvalid, tready, tlast, tuser)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo`

<a id="pcie-us-if-rqv"></a>
### fpga/lib/pcie/rtl/pcie_us_if_rq.v
- 功能：Xilinx UltraScale PCIe interface adapter (Requester reQuest)
- 定义模块：`pcie_us_if_rq`
- 接口名（聚类）：PCIE-AXIS(m_axis_rq, s_axis_rq); AXIS(m_axis, s_axis)
- 输入输出（摘要）：时钟/复位(clk, rst); PCIE-AXIS(s_axis_rq: signals); PCIE-AXIS(m_axis_rq: tdata, tkeep, tvalid, tready, tlast, tuser)
- 内含模块（实例化类型，节选）：`pcie_tlp_fifo_raw`

<a id="pcie-us-msiv"></a>
### fpga/lib/pcie/rtl/pcie_us_msi.v
- 功能：Ultrascale PCIe MSI shim
- 定义模块：`pcie_us_msi`
- 接口名（聚类）：CFG(cfg); IRQ(irq, msi)
- 输入输出（摘要）：时钟/复位(clk, rst); CFG(cfg/cfg_mgmt); IRQ(msi/msix/irq)
- 内含模块（实例化类型，节选）：`arbiter`

<a id="priority-encoderv"></a>
### fpga/lib/pcie/rtl/priority_encoder.v
- 功能：Priority encoder module
- 定义模块：`priority_encoder`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：（未识别到明显输入输出接口）
- 内含模块（实例化类型，节选）：（无）

<a id="pulse-mergev"></a>
### fpga/lib/pcie/rtl/pulse_merge.v
- 功能：Pulse merge module
- 定义模块：`pulse_merge`
- 接口名（聚类）：（未识别到接口名）
- 输入输出（摘要）：时钟/复位(clk, rst)
- 内含模块（实例化类型，节选）：（无）
