# Corundum `fpga/lib/axi/rtl` 索引

- 范围：仅覆盖 `fpga/lib/axi/rtl` 目录下文件
- 输出：每文件给出接口名聚类（按 `*_axi/*_axis` 后缀风格启发式归类；同时会识别常见的 `*_axil`）、功能、内含模块、被引用（反向扫描）
- 说明：为导航用途做的启发式解析，generate/宏/多行实例化可能导致漏判或误判

## 目录
- [fpga/lib/axi/rtl/arbiter.v](#arbiterv)
- [fpga/lib/axi/rtl/axi_adapter.v](#axi-adapterv)
- [fpga/lib/axi/rtl/axi_adapter_rd.v](#axi-adapter-rdv)
- [fpga/lib/axi/rtl/axi_adapter_wr.v](#axi-adapter-wrv)
- [fpga/lib/axi/rtl/axi_axil_adapter.v](#axi-axil-adapterv)
- [fpga/lib/axi/rtl/axi_axil_adapter_rd.v](#axi-axil-adapter-rdv)
- [fpga/lib/axi/rtl/axi_axil_adapter_wr.v](#axi-axil-adapter-wrv)
- [fpga/lib/axi/rtl/axi_cdma.v](#axi-cdmav)
- [fpga/lib/axi/rtl/axi_cdma_desc_mux.v](#axi-cdma-desc-muxv)
- [fpga/lib/axi/rtl/axi_crossbar.v](#axi-crossbarv)
- [fpga/lib/axi/rtl/axi_crossbar_addr.v](#axi-crossbar-addrv)
- [fpga/lib/axi/rtl/axi_crossbar_rd.v](#axi-crossbar-rdv)
- [fpga/lib/axi/rtl/axi_crossbar_wr.v](#axi-crossbar-wrv)
- [fpga/lib/axi/rtl/axi_crossbar_wrap.py](#axi-crossbar-wrappy)
- [fpga/lib/axi/rtl/axi_dma.v](#axi-dmav)
- [fpga/lib/axi/rtl/axi_dma_desc_mux.v](#axi-dma-desc-muxv)
- [fpga/lib/axi/rtl/axi_dma_rd.v](#axi-dma-rdv)
- [fpga/lib/axi/rtl/axi_dma_wr.v](#axi-dma-wrv)
- [fpga/lib/axi/rtl/axi_dp_ram.v](#axi-dp-ramv)
- [fpga/lib/axi/rtl/axi_fifo.v](#axi-fifov)
- [fpga/lib/axi/rtl/axi_fifo_rd.v](#axi-fifo-rdv)
- [fpga/lib/axi/rtl/axi_fifo_wr.v](#axi-fifo-wrv)
- [fpga/lib/axi/rtl/axi_interconnect.v](#axi-interconnectv)
- [fpga/lib/axi/rtl/axi_interconnect_wrap.py](#axi-interconnect-wrappy)
- [fpga/lib/axi/rtl/axi_ram.v](#axi-ramv)
- [fpga/lib/axi/rtl/axi_ram_rd_if.v](#axi-ram-rd-ifv)
- [fpga/lib/axi/rtl/axi_ram_wr_if.v](#axi-ram-wr-ifv)
- [fpga/lib/axi/rtl/axi_ram_wr_rd_if.v](#axi-ram-wr-rd-ifv)
- [fpga/lib/axi/rtl/axi_register.v](#axi-registerv)
- [fpga/lib/axi/rtl/axi_register_rd.v](#axi-register-rdv)
- [fpga/lib/axi/rtl/axi_register_wr.v](#axi-register-wrv)
- [fpga/lib/axi/rtl/axi_vfifo.v](#axi-vfifov)
- [fpga/lib/axi/rtl/axi_vfifo_dec.v](#axi-vfifo-decv)
- [fpga/lib/axi/rtl/axi_vfifo_enc.v](#axi-vfifo-encv)
- [fpga/lib/axi/rtl/axi_vfifo_raw.v](#axi-vfifo-rawv)
- [fpga/lib/axi/rtl/axi_vfifo_raw_rd.v](#axi-vfifo-raw-rdv)
- [fpga/lib/axi/rtl/axi_vfifo_raw_wr.v](#axi-vfifo-raw-wrv)
- [fpga/lib/axi/rtl/axil_adapter.v](#axil-adapterv)
- [fpga/lib/axi/rtl/axil_adapter_rd.v](#axil-adapter-rdv)
- [fpga/lib/axi/rtl/axil_adapter_wr.v](#axil-adapter-wrv)
- [fpga/lib/axi/rtl/axil_cdc.v](#axil-cdcv)
- [fpga/lib/axi/rtl/axil_cdc_rd.v](#axil-cdc-rdv)
- [fpga/lib/axi/rtl/axil_cdc_wr.v](#axil-cdc-wrv)
- [fpga/lib/axi/rtl/axil_crossbar.v](#axil-crossbarv)
- [fpga/lib/axi/rtl/axil_crossbar_addr.v](#axil-crossbar-addrv)
- [fpga/lib/axi/rtl/axil_crossbar_rd.v](#axil-crossbar-rdv)
- [fpga/lib/axi/rtl/axil_crossbar_wr.v](#axil-crossbar-wrv)
- [fpga/lib/axi/rtl/axil_crossbar_wrap.py](#axil-crossbar-wrappy)
- [fpga/lib/axi/rtl/axil_dp_ram.v](#axil-dp-ramv)
- [fpga/lib/axi/rtl/axil_interconnect.v](#axil-interconnectv)
- [fpga/lib/axi/rtl/axil_interconnect_wrap.py](#axil-interconnect-wrappy)
- [fpga/lib/axi/rtl/axil_ram.v](#axil-ramv)
- [fpga/lib/axi/rtl/axil_reg_if.v](#axil-reg-ifv)
- [fpga/lib/axi/rtl/axil_reg_if_rd.v](#axil-reg-if-rdv)
- [fpga/lib/axi/rtl/axil_reg_if_wr.v](#axil-reg-if-wrv)
- [fpga/lib/axi/rtl/axil_register.v](#axil-registerv)
- [fpga/lib/axi/rtl/axil_register_rd.v](#axil-register-rdv)
- [fpga/lib/axi/rtl/axil_register_wr.v](#axil-register-wrv)
- [fpga/lib/axi/rtl/priority_encoder.v](#priority-encoderv)

## 逐文件索引

<a id="arbiterv"></a>
### fpga/lib/axi/rtl/arbiter.v
- 功能：通用仲裁器（多请求选择/授予，常用于 AXI/AXIS 多路复用）
- 定义模块：`arbiter`
- 接口名（聚类）：Other(acknowledge, clk, grant, grant_encoded, grant_valid, request, rst)
- 内含模块（实例化类型，节选）：`priority_encoder`
- 被引用（文件，节选）：fpga/common/rtl/cpl_op_mux.v, fpga/common/rtl/desc_op_mux.v, fpga/common/rtl/event_mux.v, fpga/common/rtl/tx_req_mux.v, fpga/lib/axi/rtl/axi_cdma_desc_mux.v, fpga/lib/axi/rtl/axi_crossbar_rd.v, fpga/lib/axi/rtl/axi_crossbar_wr.v, fpga/lib/axi/rtl/axi_dma_desc_mux.v, fpga/lib/axi/rtl/axi_interconnect.v, fpga/lib/axi/rtl/axil_crossbar_rd.v, fpga/lib/axi/rtl/axil_crossbar_wr.v, fpga/lib/axi/rtl/axil_interconnect.v (+13)

<a id="axi-adapterv"></a>
### fpga/lib/axi/rtl/axi_adapter.v
- 功能：AXI4 协议/宽度/时序适配器（含读写拆分变体）
- 定义模块：`axi_adapter`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：`axi_adapter_wr`, `axi_adapter_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_adapter_16_32.v, fpga/lib/axi/tb/test_axi_adapter_32_16.v, fpga/lib/axi/tb/test_axi_adapter_32_32.v

<a id="axi-adapter-rdv"></a>
### fpga/lib/axi/rtl/axi_adapter_rd.v
- 功能：AXI4 协议/宽度/时序适配器（含读写拆分变体）
- 定义模块：`axi_adapter_rd`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_adapter.v

<a id="axi-adapter-wrv"></a>
### fpga/lib/axi/rtl/axi_adapter_wr.v
- 功能：AXI4 协议/宽度/时序适配器（含读写拆分变体）
- 定义模块：`axi_adapter_wr`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_adapter.v

<a id="axi-axil-adapterv"></a>
### fpga/lib/axi/rtl/axi_axil_adapter.v
- 功能：AXI4 协议/宽度/时序适配器（含读写拆分变体）
- 定义模块：`axi_axil_adapter`
- 接口名（聚类）：AXI(s_axi); AXIL(m_axil)
- 内含模块（实例化类型，节选）：`axi_axil_adapter_wr`, `axi_axil_adapter_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_axil_adapter_16_32.v, fpga/lib/axi/tb/test_axi_axil_adapter_32_16.v, fpga/lib/axi/tb/test_axi_axil_adapter_32_32.v

<a id="axi-axil-adapter-rdv"></a>
### fpga/lib/axi/rtl/axi_axil_adapter_rd.v
- 功能：AXI4 协议/宽度/时序适配器（含读写拆分变体）
- 定义模块：`axi_axil_adapter_rd`
- 接口名（聚类）：AXI(s_axi); AXIL(m_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_axil_adapter.v

<a id="axi-axil-adapter-wrv"></a>
### fpga/lib/axi/rtl/axi_axil_adapter_wr.v
- 功能：AXI4 协议/宽度/时序适配器（含读写拆分变体）
- 定义模块：`axi_axil_adapter_wr`
- 接口名（聚类）：AXI(s_axi); AXIL(m_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_axil_adapter.v

<a id="axi-cdmav"></a>
### fpga/lib/axi/rtl/axi_cdma.v
- 功能：AXI Central DMA（AXI4 到 AXI4 的内存拷贝/搬运引擎）
- 定义模块：`axi_cdma`
- 接口名（聚类）：AXI(m_axi); AXIS(m_axis, s_axis)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_cdma_32.v, fpga/lib/axi/tb/test_axi_cdma_32_unaligned.v

<a id="axi-cdma-desc-muxv"></a>
### fpga/lib/axi/rtl/axi_cdma_desc_mux.v
- 功能：AXI Central DMA（AXI4 到 AXI4 的内存拷贝/搬运引擎）
- 定义模块：`axi_cdma_desc_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：（无）

<a id="axi-crossbarv"></a>
### fpga/lib/axi/rtl/axi_crossbar.v
- 功能：AXI4 crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axi_crossbar`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：`axi_crossbar_wr`, `axi_crossbar_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_crossbar_4x4.v

<a id="axi-crossbar-addrv"></a>
### fpga/lib/axi/rtl/axi_crossbar_addr.v
- 功能：AXI4 crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axi_crossbar_addr`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_crossbar_rd.v, fpga/lib/axi/rtl/axi_crossbar_wr.v

<a id="axi-crossbar-rdv"></a>
### fpga/lib/axi/rtl/axi_crossbar_rd.v
- 功能：AXI4 crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axi_crossbar_rd`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：`axi_crossbar_addr`, `arbiter`, `axi_register_rd`
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_crossbar.v

<a id="axi-crossbar-wrv"></a>
### fpga/lib/axi/rtl/axi_crossbar_wr.v
- 功能：AXI4 crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axi_crossbar_wr`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：`axi_crossbar_addr`, `arbiter`, `axi_register_wr`
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_crossbar.v

<a id="axi-crossbar-wrappy"></a>
### fpga/lib/axi/rtl/axi_crossbar_wrap.py
- 功能：生成/封装可参数化的 AXI/AXI-Lite crossbar 变体（用于代码生成/集成）
- 文件类型：Python 生成/封装脚本
- 接口名（聚类）：（未识别到 AXI/AXIS 风格接口名）
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：（无）

<a id="axi-dmav"></a>
### fpga/lib/axi/rtl/axi_dma.v
- 功能：AXI DMA（基于 AXI4 读写通道搬运数据，含描述符/多路复用变体）
- 定义模块：`axi_dma`
- 接口名（聚类）：AXI(m_axi); AXIS(m_axis, s_axis)
- 内含模块（实例化类型，节选）：`axi_dma_rd`, `axi_dma_wr`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_dma_32_32.v

<a id="axi-dma-desc-muxv"></a>
### fpga/lib/axi/rtl/axi_dma_desc_mux.v
- 功能：AXI DMA（基于 AXI4 读写通道搬运数据，含描述符/多路复用变体）
- 定义模块：`axi_dma_desc_mux`
- 接口名（聚类）：AXIS(m_axis, s_axis)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：（无）

<a id="axi-dma-rdv"></a>
### fpga/lib/axi/rtl/axi_dma_rd.v
- 功能：AXI DMA（基于 AXI4 读写通道搬运数据，含描述符/多路复用变体）
- 定义模块：`axi_dma_rd`
- 接口名（聚类）：AXI(m_axi); AXIS(m_axis, s_axis)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_dma.v, fpga/lib/axi/tb/test_axi_dma_rd_32_32.v, fpga/lib/axi/tb/test_axi_dma_rd_32_32_unaligned.v

<a id="axi-dma-wrv"></a>
### fpga/lib/axi/rtl/axi_dma_wr.v
- 功能：AXI DMA（基于 AXI4 读写通道搬运数据，含描述符/多路复用变体）
- 定义模块：`axi_dma_wr`
- 接口名（聚类）：AXI(m_axi); AXIS(m_axis, s_axis)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_dma.v, fpga/lib/axi/tb/test_axi_dma_wr_32_32.v, fpga/lib/axi/tb/test_axi_dma_wr_32_32_unaligned.v

<a id="axi-dp-ramv"></a>
### fpga/lib/axi/rtl/axi_dp_ram.v
- 功能：AXI4 RAM/存储模型（用于仿真或 FPGA 内存接口封装）
- 定义模块：`axi_dp_ram`
- 接口名（聚类）：AXI(s_axi)
- 内含模块（实例化类型，节选）：`axi_ram_wr_rd_if`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_dp_ram.v

<a id="axi-fifov"></a>
### fpga/lib/axi/rtl/axi_fifo.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_fifo`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：`axi_fifo_wr`, `axi_fifo_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_fifo.v, fpga/lib/axi/tb/test_axi_fifo_delay.v

<a id="axi-fifo-rdv"></a>
### fpga/lib/axi/rtl/axi_fifo_rd.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_fifo_rd`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_fifo.v

<a id="axi-fifo-wrv"></a>
### fpga/lib/axi/rtl/axi_fifo_wr.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_fifo_wr`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_fifo.v

<a id="axi-interconnectv"></a>
### fpga/lib/axi/rtl/axi_interconnect.v
- 功能：AXI4 interconnect（多主多从互联组合封装）
- 定义模块：`axi_interconnect`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_interconnect_4x4.v

<a id="axi-interconnect-wrappy"></a>
### fpga/lib/axi/rtl/axi_interconnect_wrap.py
- 功能：生成/封装可参数化的 AXI/AXI-Lite interconnect 变体（用于代码生成/集成）
- 文件类型：Python 生成/封装脚本
- 接口名（聚类）：（未识别到 AXI/AXIS 风格接口名）
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：（无）

<a id="axi-ramv"></a>
### fpga/lib/axi/rtl/axi_ram.v
- 功能：AXI4 RAM/存储模型（用于仿真或 FPGA 内存接口封装）
- 定义模块：`axi_ram`
- 接口名（聚类）：AXI(s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_ram.v, fpga/lib/pcie/example/ADM_PCIE_9V3/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU200/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU250/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU280/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/AU50/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/ExaNIC_X10/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/ExaNIC_X25/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/VCU108/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/VCU118/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/VCU1525/fpga_axi/rtl/fpga_core.v, fpga/lib/pcie/example/ZCU106/fpga_axi/rtl/fpga_core.v (+2)

<a id="axi-ram-rd-ifv"></a>
### fpga/lib/axi/rtl/axi_ram_rd_if.v
- 功能：AXI4 RAM/存储模型（用于仿真或 FPGA 内存接口封装）
- 定义模块：`axi_ram_rd_if`
- 接口名（聚类）：AXI(s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_ram_wr_rd_if.v

<a id="axi-ram-wr-ifv"></a>
### fpga/lib/axi/rtl/axi_ram_wr_if.v
- 功能：AXI4 RAM/存储模型（用于仿真或 FPGA 内存接口封装）
- 定义模块：`axi_ram_wr_if`
- 接口名（聚类）：AXI(s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_ram_wr_rd_if.v

<a id="axi-ram-wr-rd-ifv"></a>
### fpga/lib/axi/rtl/axi_ram_wr_rd_if.v
- 功能：AXI4 RAM/存储模型（用于仿真或 FPGA 内存接口封装）
- 定义模块：`axi_ram_wr_rd_if`
- 接口名（聚类）：AXI(s_axi)
- 内含模块（实例化类型，节选）：`axi_ram_wr_if`, `axi_ram_rd_if`
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_dp_ram.v

<a id="axi-registerv"></a>
### fpga/lib/axi/rtl/axi_register.v
- 功能：AXI4 register slice（通道寄存/流水化/时序优化，含读写拆分变体）
- 定义模块：`axi_register`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：`axi_register_wr`, `axi_register_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axi_register.v

<a id="axi-register-rdv"></a>
### fpga/lib/axi/rtl/axi_register_rd.v
- 功能：AXI4 register slice（通道寄存/流水化/时序优化，含读写拆分变体）
- 定义模块：`axi_register_rd`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_crossbar_rd.v, fpga/lib/axi/rtl/axi_register.v

<a id="axi-register-wrv"></a>
### fpga/lib/axi/rtl/axi_register_wr.v
- 功能：AXI4 register slice（通道寄存/流水化/时序优化，含读写拆分变体）
- 定义模块：`axi_register_wr`
- 接口名（聚类）：AXI(m_axi, s_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_crossbar_wr.v, fpga/lib/axi/rtl/axi_register.v

<a id="axi-vfifov"></a>
### fpga/lib/axi/rtl/axi_vfifo.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_vfifo`
- 接口名（聚类）：AXI(m_axi); AXIS(m_axis, s_axis)
- 内含模块（实例化类型，节选）：`axi_vfifo_enc`, `axi_vfifo_raw`, `axi_vfifo_dec`
- 被引用（文件，节选）：（无）

<a id="axi-vfifo-decv"></a>
### fpga/lib/axi/rtl/axi_vfifo_dec.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_vfifo_dec`
- 接口名（聚类）：AXIS(m_axis)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_vfifo.v

<a id="axi-vfifo-encv"></a>
### fpga/lib/axi/rtl/axi_vfifo_enc.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_vfifo_enc`
- 接口名（聚类）：AXIS(s_axis)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_vfifo.v

<a id="axi-vfifo-rawv"></a>
### fpga/lib/axi/rtl/axi_vfifo_raw.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_vfifo_raw`
- 接口名（聚类）：AXI(m_axi)
- 内含模块（实例化类型，节选）：`axi_vfifo_raw_wr`, `axi_vfifo_raw_rd`
- 被引用（文件，节选）：fpga/app/dma_bench/rtl/dram_test_ch.v, fpga/lib/axi/rtl/axi_vfifo.v

<a id="axi-vfifo-raw-rdv"></a>
### fpga/lib/axi/rtl/axi_vfifo_raw_rd.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_vfifo_raw_rd`
- 接口名（聚类）：AXI(m_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_vfifo_raw.v

<a id="axi-vfifo-raw-wrv"></a>
### fpga/lib/axi/rtl/axi_vfifo_raw_wr.v
- 功能：AXI4 FIFO/缓冲（解耦时序、改善吞吐，含读写拆分变体）
- 定义模块：`axi_vfifo_raw_wr`
- 接口名（聚类）：AXI(m_axi)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axi_vfifo_raw.v

<a id="axil-adapterv"></a>
### fpga/lib/axi/rtl/axil_adapter.v
- 功能：AXI-Lite 适配器（含读写拆分变体）
- 定义模块：`axil_adapter`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：`axil_adapter_wr`, `axil_adapter_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axil_adapter_16_32.v, fpga/lib/axi/tb/test_axil_adapter_32_16.v, fpga/lib/axi/tb/test_axil_adapter_32_32.v

<a id="axil-adapter-rdv"></a>
### fpga/lib/axi/rtl/axil_adapter_rd.v
- 功能：AXI-Lite 适配器（含读写拆分变体）
- 定义模块：`axil_adapter_rd`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_adapter.v

<a id="axil-adapter-wrv"></a>
### fpga/lib/axi/rtl/axil_adapter_wr.v
- 功能：AXI-Lite 适配器（含读写拆分变体）
- 定义模块：`axil_adapter_wr`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_adapter.v

<a id="axil-cdcv"></a>
### fpga/lib/axi/rtl/axil_cdc.v
- 功能：AXI-Lite CDC（跨时钟域桥接，含读写拆分变体）
- 定义模块：`axil_cdc`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：`axil_cdc_wr`, `axil_cdc_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axil_cdc.v, fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au200.v, fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au280.v, fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au50.v, fpga/mqnic/Alveo/fpga_100g/rtl/fpga_au55.v, fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au200.v, fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au280.v, fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au50.v, fpga/mqnic/Alveo/fpga_25g/rtl/fpga_au55.v

<a id="axil-cdc-rdv"></a>
### fpga/lib/axi/rtl/axil_cdc_rd.v
- 功能：AXI-Lite CDC（跨时钟域桥接，含读写拆分变体）
- 定义模块：`axil_cdc_rd`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_cdc.v

<a id="axil-cdc-wrv"></a>
### fpga/lib/axi/rtl/axil_cdc_wr.v
- 功能：AXI-Lite CDC（跨时钟域桥接，含读写拆分变体）
- 定义模块：`axil_cdc_wr`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_cdc.v

<a id="axil-crossbarv"></a>
### fpga/lib/axi/rtl/axil_crossbar.v
- 功能：AXI-Lite crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axil_crossbar`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：`axil_crossbar_wr`, `axil_crossbar_rd`
- 被引用（文件，节选）：fpga/common/rtl/mqnic_core.v, fpga/common/rtl/mqnic_interface.v, fpga/common/rtl/mqnic_tx_scheduler_block_rr_tdma.v

<a id="axil-crossbar-addrv"></a>
### fpga/lib/axi/rtl/axil_crossbar_addr.v
- 功能：AXI-Lite crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axil_crossbar_addr`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_crossbar_rd.v, fpga/lib/axi/rtl/axil_crossbar_wr.v

<a id="axil-crossbar-rdv"></a>
### fpga/lib/axi/rtl/axil_crossbar_rd.v
- 功能：AXI-Lite crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axil_crossbar_rd`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：`axil_crossbar_addr`, `axil_register_rd`, `arbiter`
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_crossbar.v

<a id="axil-crossbar-wrv"></a>
### fpga/lib/axi/rtl/axil_crossbar_wr.v
- 功能：AXI-Lite crossbar（地址译码/路由/仲裁，包含读写通路）
- 定义模块：`axil_crossbar_wr`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：`axil_crossbar_addr`, `axil_register_wr`, `arbiter`
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_crossbar.v

<a id="axil-crossbar-wrappy"></a>
### fpga/lib/axi/rtl/axil_crossbar_wrap.py
- 功能：生成/封装可参数化的 AXI/AXI-Lite crossbar 变体（用于代码生成/集成）
- 文件类型：Python 生成/封装脚本
- 接口名（聚类）：（未识别到 AXI/AXIS 风格接口名）
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：（无）

<a id="axil-dp-ramv"></a>
### fpga/lib/axi/rtl/axil_dp_ram.v
- 功能：AXI-Lite RAM/存储模型
- 定义模块：`axil_dp_ram`
- 接口名（聚类）：AXIL(s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axil_dp_ram.v

<a id="axil-interconnectv"></a>
### fpga/lib/axi/rtl/axil_interconnect.v
- 功能：AXI-Lite interconnect（多主多从互联组合封装）
- 定义模块：`axil_interconnect`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：fpga/common/rtl/tdma_ber.v, fpga/lib/axi/tb/test_axil_interconnect_4x4.v

<a id="axil-interconnect-wrappy"></a>
### fpga/lib/axi/rtl/axil_interconnect_wrap.py
- 功能：生成/封装可参数化的 AXI/AXI-Lite interconnect 变体（用于代码生成/集成）
- 文件类型：Python 生成/封装脚本
- 接口名（聚类）：（未识别到 AXI/AXIS 风格接口名）
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：（无）

<a id="axil-ramv"></a>
### fpga/lib/axi/rtl/axil_ram.v
- 功能：AXI-Lite RAM/存储模型
- 定义模块：`axil_ram`
- 接口名（聚类）：AXIL(s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/app/template/rtl/mqnic_app_block.v, fpga/lib/axi/tb/test_axil_ram.v

<a id="axil-reg-ifv"></a>
### fpga/lib/axi/rtl/axil_reg_if.v
- 功能：AXI-Lite register interface（把 AXI-Lite 转换成简化寄存器读写握手）
- 定义模块：`axil_reg_if`
- 接口名（聚类）：AXIL(s_axil)
- 内含模块（实例化类型，节选）：`axil_reg_if_wr`, `axil_reg_if_rd`
- 被引用（文件，节选）：fpga/app/dma_bench/rtl/mqnic_app_block_dma_bench.v, fpga/common/rtl/mqnic_core.v, fpga/common/rtl/mqnic_interface.v, fpga/mqnic/ZCU102/fpga/rtl/fpga_core_custom_port_demo.v

<a id="axil-reg-if-rdv"></a>
### fpga/lib/axi/rtl/axil_reg_if_rd.v
- 功能：AXI-Lite register interface（把 AXI-Lite 转换成简化寄存器读写握手）
- 定义模块：`axil_reg_if_rd`
- 接口名（聚类）：AXIL(s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_reg_if.v

<a id="axil-reg-if-wrv"></a>
### fpga/lib/axi/rtl/axil_reg_if_wr.v
- 功能：AXI-Lite register interface（把 AXI-Lite 转换成简化寄存器读写握手）
- 定义模块：`axil_reg_if_wr`
- 接口名（聚类）：AXIL(s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_reg_if.v

<a id="axil-registerv"></a>
### fpga/lib/axi/rtl/axil_register.v
- 功能：AXI-Lite register slice（通道寄存/流水化/时序优化，含读写拆分变体）
- 定义模块：`axil_register`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：`axil_register_wr`, `axil_register_rd`
- 被引用（文件，节选）：fpga/lib/axi/tb/test_axil_register.v

<a id="axil-register-rdv"></a>
### fpga/lib/axi/rtl/axil_register_rd.v
- 功能：AXI-Lite register slice（通道寄存/流水化/时序优化，含读写拆分变体）
- 定义模块：`axil_register_rd`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_crossbar_rd.v, fpga/lib/axi/rtl/axil_register.v

<a id="axil-register-wrv"></a>
### fpga/lib/axi/rtl/axil_register_wr.v
- 功能：AXI-Lite register slice（通道寄存/流水化/时序优化，含读写拆分变体）
- 定义模块：`axil_register_wr`
- 接口名（聚类）：AXIL(m_axil, s_axil)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/axi/rtl/axil_crossbar_wr.v, fpga/lib/axi/rtl/axil_register.v

<a id="priority-encoderv"></a>
### fpga/lib/axi/rtl/priority_encoder.v
- 功能：通用优先级编码器（onehot/priority 选择相关逻辑）
- 定义模块：`priority_encoder`
- 接口名（聚类）：Other(input_unencoded, output_encoded, output_unencoded, output_valid)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/cpl_write.v, fpga/common/rtl/desc_fetch.v, fpga/lib/axi/rtl/arbiter.v, fpga/lib/eth/lib/axis/rtl/arbiter.v, fpga/lib/eth/lib/axis/tb/test_priority_encoder.v, fpga/lib/pcie/rtl/arbiter.v
