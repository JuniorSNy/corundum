# Corundum `fpga/lib/eth/rtl` 索引

- 范围：仅覆盖 `fpga/lib/eth/rtl` 目录下文件
- 输出：每文件给出接口名聚类（按类似 `axi/axis` 的前缀或 `*_axi/*_axis/*_axil` 风格启发式归类）、功能、内含模块、被引用（反向扫描）
- 说明：为导航用途做的启发式解析，generate/宏/多行实例化可能导致漏判或误判

## 目录
- [fpga/lib/eth/rtl/arp.v](#arpv)
- [fpga/lib/eth/rtl/arp_cache.v](#arp-cachev)
- [fpga/lib/eth/rtl/arp_eth_rx.v](#arp-eth-rxv)
- [fpga/lib/eth/rtl/arp_eth_tx.v](#arp-eth-txv)
- [fpga/lib/eth/rtl/axis_baser_rx_64.v](#axis-baser-rx-64v)
- [fpga/lib/eth/rtl/axis_baser_tx_64.v](#axis-baser-tx-64v)
- [fpga/lib/eth/rtl/axis_eth_fcs.v](#axis-eth-fcsv)
- [fpga/lib/eth/rtl/axis_eth_fcs_check.v](#axis-eth-fcs-checkv)
- [fpga/lib/eth/rtl/axis_eth_fcs_check_64.v](#axis-eth-fcs-check-64v)
- [fpga/lib/eth/rtl/axis_eth_fcs_insert.v](#axis-eth-fcs-insertv)
- [fpga/lib/eth/rtl/axis_eth_fcs_insert_64.v](#axis-eth-fcs-insert-64v)
- [fpga/lib/eth/rtl/axis_gmii_rx.v](#axis-gmii-rxv)
- [fpga/lib/eth/rtl/axis_gmii_tx.v](#axis-gmii-txv)
- [fpga/lib/eth/rtl/axis_xgmii_rx_32.v](#axis-xgmii-rx-32v)
- [fpga/lib/eth/rtl/axis_xgmii_rx_64.v](#axis-xgmii-rx-64v)
- [fpga/lib/eth/rtl/axis_xgmii_tx_32.v](#axis-xgmii-tx-32v)
- [fpga/lib/eth/rtl/axis_xgmii_tx_64.v](#axis-xgmii-tx-64v)
- [fpga/lib/eth/rtl/eth_arb_mux.v](#eth-arb-muxv)
- [fpga/lib/eth/rtl/eth_axis_rx.v](#eth-axis-rxv)
- [fpga/lib/eth/rtl/eth_axis_tx.v](#eth-axis-txv)
- [fpga/lib/eth/rtl/eth_demux.v](#eth-demuxv)
- [fpga/lib/eth/rtl/eth_mac_10g.v](#eth-mac-10gv)
- [fpga/lib/eth/rtl/eth_mac_10g_fifo.v](#eth-mac-10g-fifov)
- [fpga/lib/eth/rtl/eth_mac_1g.v](#eth-mac-1gv)
- [fpga/lib/eth/rtl/eth_mac_1g_fifo.v](#eth-mac-1g-fifov)
- [fpga/lib/eth/rtl/eth_mac_1g_gmii.v](#eth-mac-1g-gmiiv)
- [fpga/lib/eth/rtl/eth_mac_1g_gmii_fifo.v](#eth-mac-1g-gmii-fifov)
- [fpga/lib/eth/rtl/eth_mac_1g_rgmii.v](#eth-mac-1g-rgmiiv)
- [fpga/lib/eth/rtl/eth_mac_1g_rgmii_fifo.v](#eth-mac-1g-rgmii-fifov)
- [fpga/lib/eth/rtl/eth_mac_mii.v](#eth-mac-miiv)
- [fpga/lib/eth/rtl/eth_mac_mii_fifo.v](#eth-mac-mii-fifov)
- [fpga/lib/eth/rtl/eth_mac_phy_10g.v](#eth-mac-phy-10gv)
- [fpga/lib/eth/rtl/eth_mac_phy_10g_fifo.v](#eth-mac-phy-10g-fifov)
- [fpga/lib/eth/rtl/eth_mac_phy_10g_rx.v](#eth-mac-phy-10g-rxv)
- [fpga/lib/eth/rtl/eth_mac_phy_10g_tx.v](#eth-mac-phy-10g-txv)
- [fpga/lib/eth/rtl/eth_mux.v](#eth-muxv)
- [fpga/lib/eth/rtl/eth_phy_10g.v](#eth-phy-10gv)
- [fpga/lib/eth/rtl/eth_phy_10g_rx.v](#eth-phy-10g-rxv)
- [fpga/lib/eth/rtl/eth_phy_10g_rx_ber_mon.v](#eth-phy-10g-rx-ber-monv)
- [fpga/lib/eth/rtl/eth_phy_10g_rx_frame_sync.v](#eth-phy-10g-rx-frame-syncv)
- [fpga/lib/eth/rtl/eth_phy_10g_rx_if.v](#eth-phy-10g-rx-ifv)
- [fpga/lib/eth/rtl/eth_phy_10g_rx_watchdog.v](#eth-phy-10g-rx-watchdogv)
- [fpga/lib/eth/rtl/eth_phy_10g_tx.v](#eth-phy-10g-txv)
- [fpga/lib/eth/rtl/eth_phy_10g_tx_if.v](#eth-phy-10g-tx-ifv)
- [fpga/lib/eth/rtl/gmii_phy_if.v](#gmii-phy-ifv)
- [fpga/lib/eth/rtl/iddr.v](#iddrv)
- [fpga/lib/eth/rtl/ip.v](#ipv)
- [fpga/lib/eth/rtl/ip_64.v](#ip-64v)
- [fpga/lib/eth/rtl/ip_arb_mux.v](#ip-arb-muxv)
- [fpga/lib/eth/rtl/ip_complete.v](#ip-completev)
- [fpga/lib/eth/rtl/ip_complete_64.v](#ip-complete-64v)
- [fpga/lib/eth/rtl/ip_demux.v](#ip-demuxv)
- [fpga/lib/eth/rtl/ip_eth_rx.v](#ip-eth-rxv)
- [fpga/lib/eth/rtl/ip_eth_rx_64.v](#ip-eth-rx-64v)
- [fpga/lib/eth/rtl/ip_eth_tx.v](#ip-eth-txv)
- [fpga/lib/eth/rtl/ip_eth_tx_64.v](#ip-eth-tx-64v)
- [fpga/lib/eth/rtl/ip_mux.v](#ip-muxv)
- [fpga/lib/eth/rtl/lfsr.v](#lfsrv)
- [fpga/lib/eth/rtl/mac_ctrl_rx.v](#mac-ctrl-rxv)
- [fpga/lib/eth/rtl/mac_ctrl_tx.v](#mac-ctrl-txv)
- [fpga/lib/eth/rtl/mac_pause_ctrl_rx.v](#mac-pause-ctrl-rxv)
- [fpga/lib/eth/rtl/mac_pause_ctrl_tx.v](#mac-pause-ctrl-txv)
- [fpga/lib/eth/rtl/mii_phy_if.v](#mii-phy-ifv)
- [fpga/lib/eth/rtl/oddr.v](#oddrv)
- [fpga/lib/eth/rtl/ptp_clock.v](#ptp-clockv)
- [fpga/lib/eth/rtl/ptp_clock_cdc.v](#ptp-clock-cdcv)
- [fpga/lib/eth/rtl/ptp_perout.v](#ptp-peroutv)
- [fpga/lib/eth/rtl/ptp_tag_insert.v](#ptp-tag-insertv)
- [fpga/lib/eth/rtl/ptp_td_leaf.v](#ptp-td-leafv)
- [fpga/lib/eth/rtl/ptp_td_phc.v](#ptp-td-phcv)
- [fpga/lib/eth/rtl/ptp_td_rel2tod.v](#ptp-td-rel2todv)
- [fpga/lib/eth/rtl/ptp_ts_extract.v](#ptp-ts-extractv)
- [fpga/lib/eth/rtl/rgmii_phy_if.v](#rgmii-phy-ifv)
- [fpga/lib/eth/rtl/ssio_ddr_in.v](#ssio-ddr-inv)
- [fpga/lib/eth/rtl/ssio_ddr_in_diff.v](#ssio-ddr-in-diffv)
- [fpga/lib/eth/rtl/ssio_ddr_out.v](#ssio-ddr-outv)
- [fpga/lib/eth/rtl/ssio_ddr_out_diff.v](#ssio-ddr-out-diffv)
- [fpga/lib/eth/rtl/ssio_sdr_in.v](#ssio-sdr-inv)
- [fpga/lib/eth/rtl/ssio_sdr_in_diff.v](#ssio-sdr-in-diffv)
- [fpga/lib/eth/rtl/ssio_sdr_out.v](#ssio-sdr-outv)
- [fpga/lib/eth/rtl/ssio_sdr_out_diff.v](#ssio-sdr-out-diffv)
- [fpga/lib/eth/rtl/udp.v](#udpv)
- [fpga/lib/eth/rtl/udp_64.v](#udp-64v)
- [fpga/lib/eth/rtl/udp_arb_mux.v](#udp-arb-muxv)
- [fpga/lib/eth/rtl/udp_checksum_gen.v](#udp-checksum-genv)
- [fpga/lib/eth/rtl/udp_checksum_gen_64.v](#udp-checksum-gen-64v)
- [fpga/lib/eth/rtl/udp_complete.v](#udp-completev)
- [fpga/lib/eth/rtl/udp_complete_64.v](#udp-complete-64v)
- [fpga/lib/eth/rtl/udp_demux.v](#udp-demuxv)
- [fpga/lib/eth/rtl/udp_ip_rx.v](#udp-ip-rxv)
- [fpga/lib/eth/rtl/udp_ip_rx_64.v](#udp-ip-rx-64v)
- [fpga/lib/eth/rtl/udp_ip_tx.v](#udp-ip-txv)
- [fpga/lib/eth/rtl/udp_ip_tx_64.v](#udp-ip-tx-64v)
- [fpga/lib/eth/rtl/udp_mux.v](#udp-muxv)
- [fpga/lib/eth/rtl/xgmii_baser_dec_64.v](#xgmii-baser-dec-64v)
- [fpga/lib/eth/rtl/xgmii_baser_enc_64.v](#xgmii-baser-enc-64v)
- [fpga/lib/eth/rtl/xgmii_deinterleave.v](#xgmii-deinterleavev)
- [fpga/lib/eth/rtl/xgmii_interleave.v](#xgmii-interleavev)

## 逐文件索引

<a id="arpv"></a>
### fpga/lib/eth/rtl/arp.v
- 功能：ARP 协议处理（解析/应答/请求生成）
- 定义模块：`arp`
- 接口名（聚类）：ETH(eth); IP(ip); ARP(arp); MAC(mac)
- 内含模块（实例化类型，节选）：`arp_eth_rx`, `arp_eth_tx`, `arp_cache`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip_complete.v, fpga/lib/eth/rtl/ip_complete_64.v, fpga/lib/eth/tb/test_arp.v, fpga/lib/eth/tb/test_arp_64.v

<a id="arp-cachev"></a>
### fpga/lib/eth/rtl/arp_cache.v
- 功能：ARP 缓存/表项管理
- 定义模块：`arp_cache`
- 接口名（聚类）：IP(ip); ARP(arp); MAC(mac)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/arp.v, fpga/lib/eth/tb/test_arp_cache.v

<a id="arp-eth-rxv"></a>
### fpga/lib/eth/rtl/arp_eth_rx.v
- 功能：ARP over Ethernet：接收通路
- 定义模块：`arp_eth_rx`
- 接口名（聚类）：ETH(eth); ARP(arp); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/arp.v, fpga/lib/eth/tb/test_arp_eth_rx.v, fpga/lib/eth/tb/test_arp_eth_rx_64.v

<a id="arp-eth-txv"></a>
### fpga/lib/eth/rtl/arp_eth_tx.v
- 功能：ARP over Ethernet：发送通路
- 定义模块：`arp_eth_tx`
- 接口名（聚类）：ETH(eth); ARP(arp); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/arp.v, fpga/lib/eth/tb/test_arp_eth_tx.v, fpga/lib/eth/tb/test_arp_eth_tx_64.v

<a id="axis-baser-rx-64v"></a>
### fpga/lib/eth/rtl/axis_baser_rx_64.v
- 功能：BASE-R -> AXI-stream 接收适配
- 定义模块：`axis_baser_rx_64`
- 接口名（聚类）：AXIS(m_axis); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`, `endcase`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_phy_10g_rx.v, fpga/lib/eth/tb/test_axis_baser_rx_64.v

<a id="axis-baser-tx-64v"></a>
### fpga/lib/eth/rtl/axis_baser_tx_64.v
- 功能：AXI-stream -> BASE-R 发送适配
- 定义模块：`axis_baser_tx_64`
- 接口名（聚类）：AXIS(m_axis, s_axis); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_phy_10g_tx.v, fpga/lib/eth/tb/test_axis_baser_tx_64.v

<a id="axis-eth-fcsv"></a>
### fpga/lib/eth/rtl/axis_eth_fcs.v
- 功能：AXI-stream 以太网相关适配
- 定义模块：`axis_eth_fcs`
- 接口名（聚类）：AXIS(s_axis); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/tb/test_axis_eth_fcs.v, fpga/lib/eth/tb/test_axis_eth_fcs_64.v

<a id="axis-eth-fcs-checkv"></a>
### fpga/lib/eth/rtl/axis_eth_fcs_check.v
- 功能：以太网帧 FCS 校验（AXIS 通路）
- 定义模块：`axis_eth_fcs_check`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/tb/test_axis_eth_fcs_check.v

<a id="axis-eth-fcs-check-64v"></a>
### fpga/lib/eth/rtl/axis_eth_fcs_check_64.v
- 功能：以太网帧 FCS 校验（AXIS 通路）
- 定义模块：`axis_eth_fcs_check_64`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/tb/test_axis_eth_fcs_check_64.v

<a id="axis-eth-fcs-insertv"></a>
### fpga/lib/eth/rtl/axis_eth_fcs_insert.v
- 功能：以太网帧 FCS 插入（AXIS 通路）
- 定义模块：`axis_eth_fcs_insert`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/tb/test_axis_eth_fcs_insert.v, fpga/lib/eth/tb/test_axis_eth_fcs_insert_pad.v

<a id="axis-eth-fcs-insert-64v"></a>
### fpga/lib/eth/rtl/axis_eth_fcs_insert_64.v
- 功能：以太网帧 FCS 插入（AXIS 通路）
- 定义模块：`axis_eth_fcs_insert_64`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/tb/test_axis_eth_fcs_insert_64.v, fpga/lib/eth/tb/test_axis_eth_fcs_insert_64_pad.v

<a id="axis-gmii-rxv"></a>
### fpga/lib/eth/rtl/axis_gmii_rx.v
- 功能：GMII -> AXI-stream 接收适配
- 定义模块：`axis_gmii_rx`
- 接口名（聚类）：AXIS(m_axis); GMII(gmii); MII(mii); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_1g.v, fpga/lib/eth/tb/test_axis_gmii_rx.v

<a id="axis-gmii-txv"></a>
### fpga/lib/eth/rtl/axis_gmii_tx.v
- 功能：AXI-stream -> GMII 发送适配
- 定义模块：`axis_gmii_tx`
- 接口名（聚类）：AXIS(m_axis, s_axis); GMII(gmii); MII(mii); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`, `endcase`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_1g.v, fpga/lib/eth/tb/test_axis_gmii_tx.v

<a id="axis-xgmii-rx-32v"></a>
### fpga/lib/eth/rtl/axis_xgmii_rx_32.v
- 功能：XGMII -> AXI-stream 接收适配
- 定义模块：`axis_xgmii_rx_32`
- 接口名（聚类）：AXIS(m_axis); XGMII(xgmii); GMII(gmii); MII(mii); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/tb/test_axis_xgmii_rx_32.v

<a id="axis-xgmii-rx-64v"></a>
### fpga/lib/eth/rtl/axis_xgmii_rx_64.v
- 功能：XGMII -> AXI-stream 接收适配
- 定义模块：`axis_xgmii_rx_64`
- 接口名（聚类）：AXIS(m_axis); XGMII(xgmii); GMII(gmii); MII(mii); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/tb/test_axis_xgmii_rx_64.v

<a id="axis-xgmii-tx-32v"></a>
### fpga/lib/eth/rtl/axis_xgmii_tx_32.v
- 功能：AXI-stream -> XGMII 发送适配
- 定义模块：`axis_xgmii_tx_32`
- 接口名（聚类）：AXIS(m_axis, s_axis); XGMII(xgmii); GMII(gmii); MII(mii); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/tb/test_axis_xgmii_tx_32.v

<a id="axis-xgmii-tx-64v"></a>
### fpga/lib/eth/rtl/axis_xgmii_tx_64.v
- 功能：AXI-stream -> XGMII 发送适配
- 定义模块：`axis_xgmii_tx_64`
- 接口名（聚类）：AXIS(m_axis, s_axis); XGMII(xgmii); GMII(gmii); MII(mii); PTP(ptp); ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/tb/test_axis_xgmii_tx_64.v

<a id="eth-arb-muxv"></a>
### fpga/lib/eth/rtl/eth_arb_mux.v
- 功能：以太网帧仲裁/多路复用（arb+mux）
- 定义模块：`eth_arb_mux`
- 接口名（聚类）：ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip_complete.v, fpga/lib/eth/rtl/ip_complete_64.v, fpga/lib/eth/tb/test_eth_arb_mux_4.v, fpga/lib/eth/tb/test_eth_arb_mux_64_4.v

<a id="eth-axis-rxv"></a>
### fpga/lib/eth/rtl/eth_axis_rx.v
- 功能：以太网帧（header/payload）-> AXI-stream 接收通路
- 定义模块：`eth_axis_rx`
- 接口名（聚类）：AXIS(s_axis); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/example/520N_MX/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/ATLYS/fpga/rtl/fpga_core.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/Arty/fpga/rtl/fpga_core.v, fpga/lib/eth/example/C10LP/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DCS7132LB/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/DE2-115/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DE5-Net/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X10/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X25/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/HTG640/fpga/rtl/fpga_core.v (+24)

<a id="eth-axis-txv"></a>
### fpga/lib/eth/rtl/eth_axis_tx.v
- 功能：AXI-stream -> 以太网帧（header/payload）发送通路
- 定义模块：`eth_axis_tx`
- 接口名（聚类）：AXIS(m_axis); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/example/520N_MX/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/ATLYS/fpga/rtl/fpga_core.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/Arty/fpga/rtl/fpga_core.v, fpga/lib/eth/example/C10LP/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DCS7132LB/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/DE2-115/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DE5-Net/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X10/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X25/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/HTG640/fpga/rtl/fpga_core.v (+23)

<a id="eth-demuxv"></a>
### fpga/lib/eth/rtl/eth_demux.v
- 功能：以太网帧多路复用（mux）
- 定义模块：`eth_demux`
- 接口名（聚类）：ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/tb/test_eth_demux_4.v, fpga/lib/eth/tb/test_eth_demux_64_4.v

<a id="eth-mac-10gv"></a>
### fpga/lib/eth/rtl/eth_mac_10g.v
- 功能：10G 以太网 MAC
- 定义模块：`eth_mac_10g`
- 接口名（聚类）：AXIS(m_axis, s_axis); XGMII(xgmii); GMII(gmii); MII(mii); PTP(ptp); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`axis_xgmii_rx_64`, `axis_xgmii_tx_64`, `axis_xgmii_rx_32`, `axis_xgmii_tx_32`, `mac_ctrl_tx`, `mac_ctrl_rx`, `mac_pause_ctrl_tx`, `mac_pause_ctrl_rx`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_10g_fifo.v, fpga/lib/eth/tb/test_eth_mac_10g_32.v, fpga/lib/eth/tb/test_eth_mac_10g_64.v, fpga/mqnic/250_SoC/fpga_25g/rtl/fpga_core.v, fpga/mqnic/520N_MX/fpga_25g/rtl/fpga_core.v, fpga/mqnic/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v, fpga/mqnic/Alveo/fpga_25g/rtl/fpga_core.v, fpga/mqnic/DK_DEV_1SMX_H_A/fpga_25g/rtl/fpga_core.v, fpga/mqnic/DNPCIe_40G_KU_LL_2QSFP/fpga/rtl/fpga_core.v, fpga/mqnic/KR260/fpga/rtl/fpga_core.v, fpga/mqnic/NetFPGA_SUME/fpga/rtl/fpga_core.v, fpga/mqnic/Nexus_K3P_Q/fpga_25g/rtl/fpga_core.v (+10)

<a id="eth-mac-10g-fifov"></a>
### fpga/lib/eth/rtl/eth_mac_10g_fifo.v
- 功能：10G 以太网 MAC（带 FIFO）
- 定义模块：`eth_mac_10g_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis); XGMII(xgmii); GMII(gmii); MII(mii); PTP(ptp); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`ptp_clock_cdc`, `axis_async_fifo`, `eth_mac_10g`, `axis_async_fifo_adapter`
- 被引用（文件，节选）：fpga/lib/eth/example/520N_MX/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/DCS7132LB/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/DE5-Net/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X10/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X25/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/HTG640/fpga/rtl/fpga_core.v, fpga/lib/eth/example/HTG640/fpga_cxpt16/rtl/fpga_core.v, fpga/lib/eth/example/HTG9200/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/HTG9200/fpga_fmc_htg_6qsfp_25g/rtl/fpga_core.v, fpga/lib/eth/example/NetFPGA_SUME/fpga/rtl/fpga_core.v (+11)

<a id="eth-mac-1gv"></a>
### fpga/lib/eth/rtl/eth_mac_1g.v
- 功能：1G 以太网 MAC
- 定义模块：`eth_mac_1g`
- 接口名（聚类）：AXIS(m_axis, s_axis); GMII(gmii); MII(mii); PTP(ptp); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`axis_gmii_rx`, `axis_gmii_tx`, `mac_ctrl_tx`, `mac_ctrl_rx`, `mac_pause_ctrl_tx`, `mac_pause_ctrl_rx`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_1g_fifo.v, fpga/lib/eth/rtl/eth_mac_1g_gmii.v, fpga/lib/eth/rtl/eth_mac_1g_rgmii.v, fpga/lib/eth/rtl/eth_mac_mii.v, fpga/lib/eth/tb/test_eth_mac_1g.v

<a id="eth-mac-1g-fifov"></a>
### fpga/lib/eth/rtl/eth_mac_1g_fifo.v
- 功能：1G 以太网 MAC（带 FIFO）
- 定义模块：`eth_mac_1g_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis); GMII(gmii); MII(mii); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`eth_mac_1g`, `axis_async_fifo_adapter`
- 被引用（文件，节选）：fpga/lib/eth/example/KC705/fpga_sgmii/rtl/fpga_core.v, fpga/lib/eth/example/ML605/fpga_sgmii/rtl/fpga_core.v, fpga/lib/eth/example/VCU108/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/VCU108/fpga_1g/rtl/fpga_core.v, fpga/lib/eth/example/VCU118/fpga_1g/rtl/fpga_core.v, fpga/lib/eth/example/VCU118/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/VCU118/fpga_fmc_htg_6qsfp_25g/rtl/fpga_core.v, fpga/lib/eth/tb/test_eth_mac_1g_fifo.v

<a id="eth-mac-1g-gmiiv"></a>
### fpga/lib/eth/rtl/eth_mac_1g_gmii.v
- 功能：1G 以太网 MAC：GMII 物理接口变体
- 定义模块：`eth_mac_1g_gmii`
- 接口名（聚类）：GMII(gmii); MII(mii); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`gmii_phy_if`, `eth_mac_1g`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_1g_gmii_fifo.v, fpga/lib/eth/tb/test_eth_mac_1g_gmii.v

<a id="eth-mac-1g-gmii-fifov"></a>
### fpga/lib/eth/rtl/eth_mac_1g_gmii_fifo.v
- 功能：1G 以太网 MAC：GMII 物理接口变体（带 FIFO）
- 定义模块：`eth_mac_1g_gmii_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis); GMII(gmii); MII(mii); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`eth_mac_1g_gmii`, `axis_async_fifo_adapter`
- 被引用（文件，节选）：fpga/lib/eth/example/ATLYS/fpga/rtl/fpga_core.v, fpga/lib/eth/example/KC705/fpga_gmii/rtl/fpga_core.v, fpga/lib/eth/example/ML605/fpga_gmii/rtl/fpga_core.v, fpga/lib/eth/tb/test_eth_mac_1g_gmii_fifo.v

<a id="eth-mac-1g-rgmiiv"></a>
### fpga/lib/eth/rtl/eth_mac_1g_rgmii.v
- 功能：1G 以太网 MAC：GMII 物理接口变体
- 定义模块：`eth_mac_1g_rgmii`
- 接口名（聚类）：GMII(gmii); MII(mii); RGMII(rgmii); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`rgmii_phy_if`, `eth_mac_1g`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_1g_rgmii_fifo.v, fpga/lib/eth/tb/test_eth_mac_1g_rgmii.v

<a id="eth-mac-1g-rgmii-fifov"></a>
### fpga/lib/eth/rtl/eth_mac_1g_rgmii_fifo.v
- 功能：1G 以太网 MAC：GMII 物理接口变体（带 FIFO）
- 定义模块：`eth_mac_1g_rgmii_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis); GMII(gmii); MII(mii); RGMII(rgmii); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`eth_mac_1g_rgmii`, `axis_async_fifo_adapter`
- 被引用（文件，节选）：fpga/lib/eth/example/C10LP/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DE2-115/fpga/rtl/fpga_core.v, fpga/lib/eth/example/KC705/fpga_rgmii/rtl/fpga_core.v, fpga/lib/eth/example/ML605/fpga_rgmii/rtl/fpga_core.v, fpga/lib/eth/example/NexysVideo/fpga/rtl/fpga_core.v, fpga/lib/eth/example/RV901T/fpga/rtl/fpga_core.v, fpga/lib/eth/tb/test_eth_mac_1g_rgmii_fifo.v

<a id="eth-mac-miiv"></a>
### fpga/lib/eth/rtl/eth_mac_mii.v
- 功能：MII 以太网 MAC
- 定义模块：`eth_mac_mii`
- 接口名（聚类）：GMII(gmii); MII(mii); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`mii_phy_if`, `eth_mac_1g`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_mii_fifo.v, fpga/lib/eth/tb/test_eth_mac_mii.v

<a id="eth-mac-mii-fifov"></a>
### fpga/lib/eth/rtl/eth_mac_mii_fifo.v
- 功能：MII 以太网 MAC（带 FIFO）
- 定义模块：`eth_mac_mii_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis); MII(mii); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`eth_mac_mii`, `axis_async_fifo_adapter`
- 被引用（文件，节选）：fpga/lib/eth/example/Arty/fpga/rtl/fpga_core.v, fpga/lib/eth/tb/test_eth_mac_mii_fifo.v

<a id="eth-mac-phy-10gv"></a>
### fpga/lib/eth/rtl/eth_mac_phy_10g.v
- 功能：10G MAC+PHY 组合封装
- 定义模块：`eth_mac_phy_10g`
- 接口名（聚类）：AXIS(m_axis, s_axis); PTP(ptp); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`eth_mac_phy_10g_rx`, `eth_mac_phy_10g_tx`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_phy_10g_fifo.v, fpga/lib/eth/tb/test_eth_mac_phy_10g.v

<a id="eth-mac-phy-10g-fifov"></a>
### fpga/lib/eth/rtl/eth_mac_phy_10g_fifo.v
- 功能：10G MAC+PHY 组合封装：带 FIFO 缓冲
- 定义模块：`eth_mac_phy_10g_fifo`
- 接口名（聚类）：AXIS(m_axis, s_axis); PTP(ptp); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`ptp_clock_cdc`, `axis_async_fifo`, `eth_mac_phy_10g`, `axis_async_fifo_adapter`
- 被引用（文件，节选）：fpga/lib/eth/tb/test_eth_mac_phy_10g_fifo.v, fpga/lib/eth/tb/test_eth_mac_phy_10g_fifo_ptp.v

<a id="eth-mac-phy-10g-rxv"></a>
### fpga/lib/eth/rtl/eth_mac_phy_10g_rx.v
- 功能：10G MAC+PHY 组合封装：接收通路
- 定义模块：`eth_mac_phy_10g_rx`
- 接口名（聚类）：AXIS(m_axis); PTP(ptp); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`eth_phy_10g_rx_if`, `axis_baser_rx_64`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_phy_10g.v

<a id="eth-mac-phy-10g-txv"></a>
### fpga/lib/eth/rtl/eth_mac_phy_10g_tx.v
- 功能：10G MAC+PHY 组合封装：发送通路
- 定义模块：`eth_mac_phy_10g_tx`
- 接口名（聚类）：AXIS(m_axis, s_axis); PTP(ptp); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：`axis_baser_tx_64`, `eth_phy_10g_tx_if`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_phy_10g.v

<a id="eth-muxv"></a>
### fpga/lib/eth/rtl/eth_mux.v
- 功能：以太网帧多路复用（mux）
- 定义模块：`eth_mux`
- 接口名（聚类）：ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/tb/test_eth_mux_4.v, fpga/lib/eth/tb/test_eth_mux_64_4.v

<a id="eth-phy-10gv"></a>
### fpga/lib/eth/rtl/eth_phy_10g.v
- 功能：10G PHY（64b/66b、XGMII 相关）
- 定义模块：`eth_phy_10g`
- 接口名（聚类）：XGMII(xgmii); GMII(gmii); MII(mii); ETH(eth)
- 内含模块（实例化类型，节选）：`eth_phy_10g_rx`, `eth_phy_10g_tx`
- 被引用（文件，节选）：fpga/common/rtl/eth_xcvr_phy_10g_gty_wrapper.v, fpga/lib/eth/example/520N_MX/fpga_10g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/DCS7132LB/fpga_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/ExaNIC_X10/fpga/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/ExaNIC_X25/fpga_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/HTG9200/fpga_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/HTG9200/fpga_fmc_htg_6qsfp_25g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/S10MX_DK/fpga_10g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/VCU108/fpga_10g/rtl/eth_xcvr_phy_wrapper.v, fpga/lib/eth/example/VCU118/fpga_25g/rtl/eth_xcvr_phy_wrapper.v (+7)

<a id="eth-phy-10g-rxv"></a>
### fpga/lib/eth/rtl/eth_phy_10g_rx.v
- 功能：10G PHY：接收通路
- 定义模块：`eth_phy_10g_rx`
- 接口名（聚类）：XGMII(xgmii); GMII(gmii); MII(mii); ETH(eth)
- 内含模块（实例化类型，节选）：`eth_phy_10g_rx_if`, `xgmii_baser_dec_64`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_phy_10g.v, fpga/lib/eth/tb/test_eth_phy_10g_rx_64.v

<a id="eth-phy-10g-rx-ber-monv"></a>
### fpga/lib/eth/rtl/eth_phy_10g_rx_ber_mon.v
- 功能：10G PHY：误码率监测（BER monitor）
- 定义模块：`eth_phy_10g_rx_ber_mon`
- 接口名（聚类）：ETH(eth)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_phy_10g_rx_if.v

<a id="eth-phy-10g-rx-frame-syncv"></a>
### fpga/lib/eth/rtl/eth_phy_10g_rx_frame_sync.v
- 功能：10G PHY：帧对齐/同步（64b/66b 相关）
- 定义模块：`eth_phy_10g_rx_frame_sync`
- 接口名（聚类）：ETH(eth); IP(ip)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_phy_10g_rx_if.v

<a id="eth-phy-10g-rx-ifv"></a>
### fpga/lib/eth/rtl/eth_phy_10g_rx_if.v
- 功能：10G PHY：接收接口适配
- 定义模块：`eth_phy_10g_rx_if`
- 接口名（聚类）：ETH(eth); IP(ip)
- 内含模块（实例化类型，节选）：`lfsr`, `eth_phy_10g_rx_frame_sync`, `eth_phy_10g_rx_ber_mon`, `eth_phy_10g_rx_watchdog`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_phy_10g_rx.v, fpga/lib/eth/rtl/eth_phy_10g_rx.v

<a id="eth-phy-10g-rx-watchdogv"></a>
### fpga/lib/eth/rtl/eth_phy_10g_rx_watchdog.v
- 功能：10G PHY：接收链路监视/超时恢复
- 定义模块：`eth_phy_10g_rx_watchdog`
- 接口名（聚类）：ETH(eth)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_phy_10g_rx_if.v

<a id="eth-phy-10g-txv"></a>
### fpga/lib/eth/rtl/eth_phy_10g_tx.v
- 功能：10G PHY：发送通路
- 定义模块：`eth_phy_10g_tx`
- 接口名（聚类）：XGMII(xgmii); GMII(gmii); MII(mii); ETH(eth)
- 内含模块（实例化类型，节选）：`xgmii_baser_enc_64`, `eth_phy_10g_tx_if`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_phy_10g.v, fpga/lib/eth/tb/test_eth_phy_10g_tx_64.v

<a id="eth-phy-10g-tx-ifv"></a>
### fpga/lib/eth/rtl/eth_phy_10g_tx_if.v
- 功能：10G PHY：发送接口适配
- 定义模块：`eth_phy_10g_tx_if`
- 接口名（聚类）：ETH(eth)
- 内含模块（实例化类型，节选）：`lfsr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_phy_10g_tx.v, fpga/lib/eth/rtl/eth_phy_10g_tx.v

<a id="gmii-phy-ifv"></a>
### fpga/lib/eth/rtl/gmii_phy_if.v
- 功能：GMII PHY 接口（I/O 采样/时序适配）
- 定义模块：`gmii_phy_if`
- 接口名（聚类）：GMII(gmii); MII(mii); MAC(mac)
- 内含模块（实例化类型，节选）：`ssio_sdr_in`, `ssio_sdr_out`, `BUFGMUX`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_1g_gmii.v

<a id="iddrv"></a>
### fpga/lib/eth/rtl/iddr.v
- 功能：通用 DDR 输入/输出寄存单元封装
- 定义模块：`iddr`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`IDDR`, `IDDR2`, `altddio_in`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ssio_ddr_in.v

<a id="ipv"></a>
### fpga/lib/eth/rtl/ip.v
- 功能：IPv4 相关模块
- 定义模块：`ip`
- 接口名（聚类）：ETH(eth); IP(ip); ARP(arp); MAC(mac)
- 内含模块（实例化类型，节选）：`ip_eth_rx`, `ip_eth_tx`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip_complete.v, fpga/lib/eth/tb/test_ip.v

<a id="ip-64v"></a>
### fpga/lib/eth/rtl/ip_64.v
- 功能：IPv4 相关模块
- 定义模块：`ip_64`
- 接口名（聚类）：ETH(eth); IP(ip); ARP(arp); MAC(mac)
- 内含模块（实例化类型，节选）：`ip_eth_rx_64`, `ip_eth_tx_64`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip_complete_64.v, fpga/lib/eth/tb/test_ip_64.v

<a id="ip-arb-muxv"></a>
### fpga/lib/eth/rtl/ip_arb_mux.v
- 功能：IP 仲裁/多路复用（arb+mux）
- 定义模块：`ip_arb_mux`
- 接口名（聚类）：ETH(eth); IP(ip); MAC(mac)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_complete.v, fpga/lib/eth/rtl/udp_complete_64.v, fpga/lib/eth/tb/test_ip_arb_mux_4.v, fpga/lib/eth/tb/test_ip_arb_mux_64_4.v

<a id="ip-completev"></a>
### fpga/lib/eth/rtl/ip_complete.v
- 功能：IP 完整处理封装（含收发/校验/仲裁/路由）
- 定义模块：`ip_complete`
- 接口名（聚类）：ETH(eth); IP(ip); ARP(arp)
- 内含模块（实例化类型，节选）：`eth_arb_mux`, `arp`, `ip`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_complete.v, fpga/lib/eth/tb/test_ip_complete.v

<a id="ip-complete-64v"></a>
### fpga/lib/eth/rtl/ip_complete_64.v
- 功能：IP 完整处理封装（含收发/校验/仲裁/路由）
- 定义模块：`ip_complete_64`
- 接口名（聚类）：ETH(eth); IP(ip); ARP(arp)
- 内含模块（实例化类型，节选）：`eth_arb_mux`, `arp`, `ip_64`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_complete_64.v, fpga/lib/eth/tb/test_ip_complete_64.v

<a id="ip-demuxv"></a>
### fpga/lib/eth/rtl/ip_demux.v
- 功能：IP 多路复用（mux）
- 定义模块：`ip_demux`
- 接口名（聚类）：ETH(eth); IP(ip); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/tb/test_ip_demux_4.v, fpga/lib/eth/tb/test_ip_demux_64_4.v

<a id="ip-eth-rxv"></a>
### fpga/lib/eth/rtl/ip_eth_rx.v
- 功能：IP over Ethernet：接收通路
- 定义模块：`ip_eth_rx`
- 接口名（聚类）：ETH(eth); IP(ip); MAC(mac)
- 内含模块（实例化类型，节选）：`endcase`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip.v, fpga/lib/eth/tb/test_ip_eth_rx.v

<a id="ip-eth-rx-64v"></a>
### fpga/lib/eth/rtl/ip_eth_rx_64.v
- 功能：IP over Ethernet：接收通路
- 定义模块：`ip_eth_rx_64`
- 接口名（聚类）：ETH(eth); IP(ip); MAC(mac)
- 内含模块（实例化类型，节选）：`endcase`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip_64.v, fpga/lib/eth/tb/test_ip_eth_rx_64.v

<a id="ip-eth-txv"></a>
### fpga/lib/eth/rtl/ip_eth_tx.v
- 功能：IP over Ethernet：发送通路
- 定义模块：`ip_eth_tx`
- 接口名（聚类）：ETH(eth); IP(ip); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip.v, fpga/lib/eth/tb/test_ip_eth_tx.v

<a id="ip-eth-tx-64v"></a>
### fpga/lib/eth/rtl/ip_eth_tx_64.v
- 功能：IP over Ethernet：发送通路
- 定义模块：`ip_eth_tx_64`
- 接口名（聚类）：ETH(eth); IP(ip); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/ip_64.v, fpga/lib/eth/tb/test_ip_eth_tx_64.v

<a id="ip-muxv"></a>
### fpga/lib/eth/rtl/ip_mux.v
- 功能：IP 多路复用（mux）
- 定义模块：`ip_mux`
- 接口名（聚类）：ETH(eth); IP(ip); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/tb/test_ip_mux_4.v, fpga/lib/eth/tb/test_ip_mux_64_4.v

<a id="lfsrv"></a>
### fpga/lib/eth/rtl/lfsr.v
- 功能：LFSR 伪随机序列/CRC 相关基础模块
- 定义模块：`lfsr`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/app/dma_bench/rtl/dram_test_ch.v, fpga/lib/eth/rtl/arp_cache.v, fpga/lib/eth/rtl/axis_baser_rx_64.v, fpga/lib/eth/rtl/axis_baser_tx_64.v, fpga/lib/eth/rtl/axis_eth_fcs.v, fpga/lib/eth/rtl/axis_eth_fcs_check.v, fpga/lib/eth/rtl/axis_eth_fcs_check_64.v, fpga/lib/eth/rtl/axis_eth_fcs_insert.v, fpga/lib/eth/rtl/axis_eth_fcs_insert_64.v, fpga/lib/eth/rtl/axis_gmii_rx.v, fpga/lib/eth/rtl/axis_gmii_tx.v, fpga/lib/eth/rtl/axis_xgmii_rx_32.v (+5)

<a id="mac-ctrl-rxv"></a>
### fpga/lib/eth/rtl/mac_ctrl_rx.v
- 功能：MAC 控制帧处理：接收
- 定义模块：`mac_ctrl_rx`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_l2_ingress.v, fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/rtl/eth_mac_1g.v

<a id="mac-ctrl-txv"></a>
### fpga/lib/eth/rtl/mac_ctrl_tx.v
- 功能：MAC 控制帧处理：发送
- 定义模块：`mac_ctrl_tx`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_l2_egress.v, fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/rtl/eth_mac_1g.v

<a id="mac-pause-ctrl-rxv"></a>
### fpga/lib/eth/rtl/mac_pause_ctrl_rx.v
- 功能：MAC Pause 控制（IEEE 802.3x PAUSE）：接收
- 定义模块：`mac_pause_ctrl_rx`
- 接口名（聚类）：ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_l2_ingress.v, fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/rtl/eth_mac_1g.v

<a id="mac-pause-ctrl-txv"></a>
### fpga/lib/eth/rtl/mac_pause_ctrl_tx.v
- 功能：MAC Pause 控制（IEEE 802.3x PAUSE）：发送
- 定义模块：`mac_pause_ctrl_tx`
- 接口名（聚类）：ETH(eth); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_l2_egress.v, fpga/lib/eth/rtl/eth_mac_10g.v, fpga/lib/eth/rtl/eth_mac_1g.v

<a id="mii-phy-ifv"></a>
### fpga/lib/eth/rtl/mii_phy_if.v
- 功能：MII PHY 接口（I/O 采样/时序适配）
- 定义模块：`mii_phy_if`
- 接口名（聚类）：MII(mii); MAC(mac)
- 内含模块（实例化类型，节选）：`ssio_sdr_in`, `BUFG`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_mii.v

<a id="oddrv"></a>
### fpga/lib/eth/rtl/oddr.v
- 功能：通用 DDR 输入/输出寄存单元封装
- 定义模块：`oddr`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`ODDR`, `ODDR2`, `altddio_out`
- 被引用（文件，节选）：fpga/lib/eth/rtl/rgmii_phy_if.v, fpga/lib/eth/rtl/ssio_ddr_out.v, fpga/lib/eth/rtl/ssio_sdr_out.v

<a id="ptp-clockv"></a>
### fpga/lib/eth/rtl/ptp_clock.v
- 功能：PTP 时钟/时间基准（PHC）
- 定义模块：`ptp_clock`
- 接口名（聚类）：PTP(ptp)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/tb/test_ptp_clock.v

<a id="ptp-clock-cdcv"></a>
### fpga/lib/eth/rtl/ptp_clock_cdc.v
- 功能：PTP 时钟跨时钟域同步（CDC）
- 定义模块：`ptp_clock_cdc`
- 接口名（聚类）：PTP(ptp)
- 内含模块（实例化类型，节选）：`endcase`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_10g_fifo.v, fpga/lib/eth/rtl/eth_mac_phy_10g_fifo.v, fpga/lib/eth/tb/test_ptp_clock_cdc_64.v, fpga/lib/eth/tb/test_ptp_clock_cdc_96.v

<a id="ptp-peroutv"></a>
### fpga/lib/eth/rtl/ptp_perout.v
- 功能：PTP perout（周期输出/对时输出）
- 定义模块：`ptp_perout`
- 接口名（聚类）：PTP(ptp)
- 内含模块（实例化类型，节选）：`endcase`
- 被引用（文件，节选）：fpga/common/rtl/mqnic_ptp_perout.v, fpga/lib/eth/tb/test_ptp_perout.v

<a id="ptp-tag-insertv"></a>
### fpga/lib/eth/rtl/ptp_tag_insert.v
- 功能：PTP 时间戳 tag 插入（帧标记/回传关联）
- 定义模块：`ptp_tag_insert`
- 接口名（聚类）：AXIS(m_axis, s_axis); PTP(ptp)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：（无）

<a id="ptp-td-leafv"></a>
### fpga/lib/eth/rtl/ptp_td_leaf.v
- 功能：PTP time distribution（时域/时间分发相关模块）
- 定义模块：`ptp_td_leaf`
- 接口名（聚类）：PTP(ptp)
- 内含模块（实例化类型，节选）：`endcase`
- 被引用（文件，节选）：fpga/common/rtl/mqnic_core.v, fpga/common/rtl/mqnic_ptp_clock.v

<a id="ptp-td-phcv"></a>
### fpga/lib/eth/rtl/ptp_td_phc.v
- 功能：PTP time distribution（时域/时间分发相关模块）
- 定义模块：`ptp_td_phc`
- 接口名（聚类）：PTP(ptp)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/mqnic_ptp_clock.v

<a id="ptp-td-rel2todv"></a>
### fpga/lib/eth/rtl/ptp_td_rel2tod.v
- 功能：PTP time distribution（时域/时间分发相关模块）
- 定义模块：`ptp_td_rel2tod`
- 接口名（聚类）：PTP(ptp)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/common/rtl/rx_engine.v, fpga/common/rtl/tx_engine.v

<a id="ptp-ts-extractv"></a>
### fpga/lib/eth/rtl/ptp_ts_extract.v
- 功能：PTP（时间戳/时钟/时域转换）
- 定义模块：`ptp_ts_extract`
- 接口名（聚类）：AXIS(m_axis, s_axis); PTP(ptp)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：（无）

<a id="rgmii-phy-ifv"></a>
### fpga/lib/eth/rtl/rgmii_phy_if.v
- 功能：RGMII PHY 接口（DDR I/O 采样/时序适配）
- 定义模块：`rgmii_phy_if`
- 接口名（聚类）：GMII(gmii); MII(mii); RGMII(rgmii); MAC(mac)
- 内含模块（实例化类型，节选）：`ssio_ddr_in`, `oddr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_mac_1g_rgmii.v

<a id="ssio-ddr-inv"></a>
### fpga/lib/eth/rtl/ssio_ddr_in.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_ddr_in`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`BUFR`, `iddr`, `BUFG`, `BUFIO`
- 被引用（文件，节选）：fpga/lib/eth/rtl/rgmii_phy_if.v, fpga/lib/eth/rtl/ssio_ddr_in_diff.v

<a id="ssio-ddr-in-diffv"></a>
### fpga/lib/eth/rtl/ssio_ddr_in_diff.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_ddr_in_diff`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`ssio_ddr_in`, `IBUFDS`, `ALT_INBUF_DIFF`
- 被引用（文件，节选）：（无）

<a id="ssio-ddr-outv"></a>
### fpga/lib/eth/rtl/ssio_ddr_out.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_ddr_out`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`oddr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/ssio_ddr_out_diff.v

<a id="ssio-ddr-out-diffv"></a>
### fpga/lib/eth/rtl/ssio_ddr_out_diff.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_ddr_out_diff`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`ssio_ddr_out`, `OBUFDS`, `ALT_OUTBUF_DIFF`
- 被引用（文件，节选）：（无）

<a id="ssio-sdr-inv"></a>
### fpga/lib/eth/rtl/ssio_sdr_in.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_sdr_in`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`BUFR`, `BUFIO2`, `BUFG`, `BUFIO`
- 被引用（文件，节选）：fpga/lib/eth/rtl/gmii_phy_if.v, fpga/lib/eth/rtl/mii_phy_if.v, fpga/lib/eth/rtl/ssio_sdr_in_diff.v

<a id="ssio-sdr-in-diffv"></a>
### fpga/lib/eth/rtl/ssio_sdr_in_diff.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_sdr_in_diff`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`ssio_sdr_in`, `IBUFDS`, `ALT_INBUF_DIFF`
- 被引用（文件，节选）：（无）

<a id="ssio-sdr-outv"></a>
### fpga/lib/eth/rtl/ssio_sdr_out.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_sdr_out`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`oddr`
- 被引用（文件，节选）：fpga/lib/eth/rtl/gmii_phy_if.v, fpga/lib/eth/rtl/ssio_sdr_out_diff.v

<a id="ssio-sdr-out-diffv"></a>
### fpga/lib/eth/rtl/ssio_sdr_out_diff.v
- 功能：通用串行 I/O（SDR/DDR、单端/差分）采样/驱动单元
- 定义模块：`ssio_sdr_out_diff`
- 接口名（聚类）：（未识别到接口名）
- 内含模块（实例化类型，节选）：`ssio_sdr_out`, `OBUFDS`, `ALT_OUTBUF_DIFF`
- 被引用（文件，节选）：（无）

<a id="udpv"></a>
### fpga/lib/eth/rtl/udp.v
- 功能：UDP 相关模块
- 定义模块：`udp`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp)
- 内含模块（实例化类型，节选）：`udp_checksum_gen`, `udp_ip_rx`, `udp_ip_tx`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_complete.v, fpga/lib/eth/tb/test_udp.v

<a id="udp-64v"></a>
### fpga/lib/eth/rtl/udp_64.v
- 功能：UDP 相关模块
- 定义模块：`udp_64`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp)
- 内含模块（实例化类型，节选）：`udp_checksum_gen_64`, `udp_ip_rx_64`, `udp_ip_tx_64`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_complete_64.v, fpga/lib/eth/tb/test_udp_64.v

<a id="udp-arb-muxv"></a>
### fpga/lib/eth/rtl/udp_arb_mux.v
- 功能：UDP 仲裁/多路复用（arb+mux）
- 定义模块：`udp_arb_mux`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：`arbiter`
- 被引用（文件，节选）：fpga/lib/eth/tb/test_udp_arb_mux_4.v, fpga/lib/eth/tb/test_udp_arb_mux_64_4.v

<a id="udp-checksum-genv"></a>
### fpga/lib/eth/rtl/udp_checksum_gen.v
- 功能：UDP 校验和生成
- 定义模块：`udp_checksum_gen`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：`axis_fifo`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp.v, fpga/lib/eth/tb/test_udp_checksum_gen.v

<a id="udp-checksum-gen-64v"></a>
### fpga/lib/eth/rtl/udp_checksum_gen_64.v
- 功能：UDP 校验和生成
- 定义模块：`udp_checksum_gen_64`
- 接口名（聚类）：AXIS(m_axis, s_axis); ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：`axis_fifo`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_64.v, fpga/lib/eth/tb/test_udp_checksum_gen_64.v

<a id="udp-completev"></a>
### fpga/lib/eth/rtl/udp_complete.v
- 功能：UDP 完整处理封装（含收发/校验/仲裁/路由）
- 定义模块：`udp_complete`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); ARP(arp)
- 内含模块（实例化类型，节选）：`ip_arb_mux`, `ip_complete`, `udp`
- 被引用（文件，节选）：fpga/lib/eth/example/ATLYS/fpga/rtl/fpga_core.v, fpga/lib/eth/example/Arty/fpga/rtl/fpga_core.v, fpga/lib/eth/example/C10LP/fpga/rtl/fpga_core.v, fpga/lib/eth/example/DE2-115/fpga/rtl/fpga_core.v, fpga/lib/eth/example/KC705/fpga_gmii/rtl/fpga_core.v, fpga/lib/eth/example/KC705/fpga_rgmii/rtl/fpga_core.v, fpga/lib/eth/example/KC705/fpga_sgmii/rtl/fpga_core.v, fpga/lib/eth/example/ML605/fpga_gmii/rtl/fpga_core.v, fpga/lib/eth/example/ML605/fpga_rgmii/rtl/fpga_core.v, fpga/lib/eth/example/ML605/fpga_sgmii/rtl/fpga_core.v, fpga/lib/eth/example/NexysVideo/fpga/rtl/fpga_core.v, fpga/lib/eth/example/RV901T/fpga/rtl/fpga_core.v (+3)

<a id="udp-complete-64v"></a>
### fpga/lib/eth/rtl/udp_complete_64.v
- 功能：UDP 完整处理封装（含收发/校验/仲裁/路由）
- 定义模块：`udp_complete_64`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); ARP(arp)
- 内含模块（实例化类型，节选）：`ip_arb_mux`, `ip_complete_64`, `udp_64`
- 被引用（文件，节选）：fpga/lib/eth/example/520N_MX/fpga_10g/rtl/fpga_core.v, fpga/lib/eth/example/ADM_PCIE_9V3/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/Alveo/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/DCS7132LB/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/DE5-Net/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X10/fpga/rtl/fpga_core.v, fpga/lib/eth/example/ExaNIC_X25/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/HTG640/fpga/rtl/fpga_core.v, fpga/lib/eth/example/HTG9200/fpga_25g/rtl/fpga_core.v, fpga/lib/eth/example/HTG9200/fpga_fmc_htg_6qsfp_25g/rtl/fpga_core.v, fpga/lib/eth/example/NetFPGA_SUME/fpga/rtl/fpga_core.v, fpga/lib/eth/example/S10DX_DK/fpga_10g/rtl/fpga_core.v (+8)

<a id="udp-demuxv"></a>
### fpga/lib/eth/rtl/udp_demux.v
- 功能：UDP 多路复用（mux）
- 定义模块：`udp_demux`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/tb/test_udp_demux_4.v, fpga/lib/eth/tb/test_udp_demux_64_4.v

<a id="udp-ip-rxv"></a>
### fpga/lib/eth/rtl/udp_ip_rx.v
- 功能：UDP over IP：接收通路
- 定义模块：`udp_ip_rx`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：`endcase`
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp.v, fpga/lib/eth/tb/test_udp_ip_rx.v

<a id="udp-ip-rx-64v"></a>
### fpga/lib/eth/rtl/udp_ip_rx_64.v
- 功能：UDP over IP：接收通路
- 定义模块：`udp_ip_rx_64`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_64.v, fpga/lib/eth/tb/test_udp_ip_rx_64.v

<a id="udp-ip-txv"></a>
### fpga/lib/eth/rtl/udp_ip_tx.v
- 功能：UDP over IP：发送通路
- 定义模块：`udp_ip_tx`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp.v, fpga/lib/eth/tb/test_udp_ip_tx.v

<a id="udp-ip-tx-64v"></a>
### fpga/lib/eth/rtl/udp_ip_tx_64.v
- 功能：UDP over IP：发送通路
- 定义模块：`udp_ip_tx_64`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/udp_64.v, fpga/lib/eth/tb/test_udp_ip_tx_64.v

<a id="udp-muxv"></a>
### fpga/lib/eth/rtl/udp_mux.v
- 功能：UDP 多路复用（mux）
- 定义模块：`udp_mux`
- 接口名（聚类）：ETH(eth); IP(ip); UDP(udp); MAC(mac)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/tb/test_udp_mux_4.v, fpga/lib/eth/tb/test_udp_mux_64_4.v

<a id="xgmii-baser-dec-64v"></a>
### fpga/lib/eth/rtl/xgmii_baser_dec_64.v
- 功能：XGMII <-> 64b/66b BASE-R：解码
- 定义模块：`xgmii_baser_dec_64`
- 接口名（聚类）：XGMII(xgmii); GMII(gmii); MII(mii)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_phy_10g_rx.v, fpga/lib/eth/tb/test_xgmii_baser_dec_64.v

<a id="xgmii-baser-enc-64v"></a>
### fpga/lib/eth/rtl/xgmii_baser_enc_64.v
- 功能：XGMII <-> 64b/66b BASE-R：编码
- 定义模块：`xgmii_baser_enc_64`
- 接口名（聚类）：XGMII(xgmii); GMII(gmii); MII(mii)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/rtl/eth_phy_10g_tx.v, fpga/lib/eth/tb/test_xgmii_baser_enc_64.v

<a id="xgmii-deinterleavev"></a>
### fpga/lib/eth/rtl/xgmii_deinterleave.v
- 功能：XGMII lane interleave（通道交织）
- 定义模块：`xgmii_deinterleave`
- 接口名（聚类）：XGMII(xgmii); GMII(gmii); MII(mii)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/example/DE5-Net/fpga/rtl/fpga.v

<a id="xgmii-interleavev"></a>
### fpga/lib/eth/rtl/xgmii_interleave.v
- 功能：XGMII lane interleave（通道交织）
- 定义模块：`xgmii_interleave`
- 接口名（聚类）：XGMII(xgmii); GMII(gmii); MII(mii)
- 内含模块（实例化类型，节选）：（无）
- 被引用（文件，节选）：fpga/lib/eth/example/DE5-Net/fpga/rtl/fpga.v
