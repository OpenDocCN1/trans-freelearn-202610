

## tcpdump

**tcpdump** 是一款基于 Unix 的免费软件，可在 Unix/Linux 命令行下运行。
下面是一些有用的资源：

-   tcpdump 官方网站：[http://www.tcpdump.org/](http://www.tcpdump.org/)
-   tcpdump 的 Windows 版本（Windump）：[http://www.wincap.org/windump/default.htm](http://www.wincap.org/windump/default.htm)
-   一份友好的教程：[http://danielmiessler.com/study/tcpdump/](http://danielmiessler.com/study/tcpdump/)
-   官方手册页：[http://www.tcpdump.org/tcpdump_man.html](http://www.tcpdump.org/tcpdump_man.html)

Wireshark 可以打开 tcpdump 抓取的文件，因此当你使用 tcpdump 抓包后，之后可以用 Wireshark 或任何其他图形化工具打开它。

## SNMP 工具

我通常用来解决问题的第一批工具是 SNMP 工具。其中有些工具具有强大的映射能力，有些具有出色的统计能力，还有一些具有良好的日志记录和事件处理能力。

首先，如果只想监控 SNMP 计数器，可以使用简单的免费 MIB 浏览器和图形化工具，例如：

| 厂商 | 软件名称 | 下载地址 | 说明 | 许可证 |
|---|---|---|---|---|
| Manage engine | MibBrowser | http://www.manageengine.com/products/mibbrowser-free-tool/ | 使用非常友好，只需极少的配置。 | 免费 |
| Open source | MRTG | http://oss.oetiker.ch/mrtg/ | 安装和配置需要时间和相关知识。适合长期统计。ISP 常用它作为面向客户的展示控制台。 | 10 个传感器以内免费 (*1)；11 个传感器起商业授权 |
| SolarWinds | Network device monitor | http://www.solarwinds.com/products/freetools/network-device-monitor/ | SolarWinds 是网络管理工具领域的领导者之一，除了商业产品外，你还可以找到许多免费工具。 | 免费 |
| SolarWinds Engineering toolset | Engineer's Toolset | http://www.solarwinds.com/engineers-toolset.aspx | 提供多种工具，用于网络监控、发现、SNMP、配置、基础扫描等。 | 功能受限版免费；完整功能版商业授权 |

## SNMP 平台

SNMP 平台是一类软件，提供中央控制台，可显示网络拓扑图、收集信息并生成统计报告，同时收集 SNMP 事件并按严重程度和其他参数进行展示。

这类工具中的常见产品有：

| 厂商 | 软件名称 | 下载地址 | 说明 | 许可证 |
|---|---|---|---|---|
| Castlerock Computing | SNMPc | [http://www.castlerock.com/](http://www.castlerock.com/) | 这是我十多年来用过的最友好的 SNMP 工具之一。这个 SNMP 管理平台非常易用，非常适合网络调试。 | 商业授权 |
| SolarWinds | 各类产品 | [http://www.solarwinds.com/network-management-software.aspx](http://www.solarwinds.com/network-management-software.aspx) | SolarWinds 提供各种工具，具备监控、拓扑映射、配置管理和其他网络管理功能。这些是现有可用的最佳选择之一，但价格昂贵。 | 商业授权 |
| Manageengine | 各类产品 | http://www.manageengine.com/network-performance-management.html | 提供各种工具，具备监控、拓扑映射、配置管理和其他网络管理功能。最佳选择之一，但价格昂贵。 | 商业授权 |
| HP | IMC、NNM 等 | http://h17007.www1.hp.com/us/en/networking/solutions/network-management/index.aspx#.UkqgGT8YhyI | 这是一个很棒的平台。HP 使它比之前的 Network Node Manager (NNM) 软件友好得多。绝对值得一试。 | 商业授权 |
| OpenNMS | OpenNMS | http://www.opennms.org/ | 它是开源的，但需要掌握配置方法。 | 免费 |
| Nagios | Nagios | http://www.nagios.org/ | 它是开源的，但需要具备配置知识。 | 免费 |

还有很多其他工具，例如：

-   开源工具 Cacti（[http://www.cacti.net/](http://www.cacti.net/)）
-   Zabbix（[http://www.zabbix.com/](http://www.zabbix.com/)）
-   MRTG（[http://oss.oetiker.ch/mrtg/](http://oss.oetiker.ch/mrtg/)）
-   其他一些工具（其中部分采用 GNU 公共许可证）。

还有一些“重量级”套件，例如：

-   HP OpenView 管理应用套件（[http://www8.hp.com/us/en/software-solutions/software.html?compURI=1174702#tab=TAB1](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1174702#tab=TAB1)）
-   CA Unicenter（[http://www.ca.com/us/network-performance-management.aspx](http://www.ca.com/us/network-performance-management.aspx)）

还有其他中型平台，例如 Plixer 的各种工具（[http://www.plixer.com/](http://www.plixer.com/)）等众多选择。

对于网络监控和故障排查，你需要最基本的工具；而作为平台，则需要更复杂的产品。你可以在 [http://en.wikipedia.org/wiki/Comparison_of_network_monitoring_systems](http://en.wikipedia.org/wiki/Comparison_of_network_monitoring_systems) 找到关于管理平台不错的对比。

## NetFlow、JFlow 和 SFlow 分析器

**Cisco** 的 NetFlow（[www.cisco.com/go/netflow](http://www.cisco.com/go/netflow)）和 **Juniper** 的 JFlow（[http://www.juniper.net/techpubs/software/erx/junose82/swconfig-ip-services/html/ip-jflow-stats-config2.html](http://www.juniper.net/techpubs/software/erx/junose82/swconfig-ip-services/html/ip-jflow-stats-config2.html)）提供了一种在路由设备上收集 TCP/IP 流量统计数据的方法。

SFlow（[http://en.wikipedia.org/wiki/Sflow](http://en.wikipedia.org/wiki/Sflow) 和 [http://www.sflow.org/index.php](http://www.sflow.org/index.php)）是一种用于监控高速交换网络的行业标准技术。

它们之间的区别如下：

-   **NetFlow** 适用于 Cisco 路由器和三层交换机。在三层交换机上，请确保它们支持 NetFlow（这取决于软件版本和硬件）。在某些情况下，你需要额外的软件/硬件才能实现。它由 RFC3954 标准化（[http://www.ietf.org/rfc/rfc3954.txt](http://www.ietf.org/rfc/rfc3954.txt)）。
-   **JFlow** 适用于 Juniper 路由器和三层交换机。
-   **SFlow** 是用于监控局域网交换机的标准，由 RFC3176 标准化（[http://tools.ietf.org/html/rfc3176](http://tools.ietf.org/html/rfc3176)）。
-   **PFIX**（RFC 5101 和 5102）是基于 **NetFlow v9** 发展而来的标准，由 **IETF** 标准化。

所有 Flow/IPFIX 技术都依赖于通信设备从接口收集流量数据，并将其发送到管理站。它们需要在路由器或交换机上进行简单配置，并需要软件来收集和呈现数据。

这类软件可用于监控哪些用户对网络造成了负载（按 IP 地址或 DNS 名称显示）、哪些应用（HTTP、SMTP 等，按端口号显示）、哪些网页（按其 IP 地址显示，并可转换为 DNS 名称），以及其他类似维度。Wireshark 通常用于短期监控（**Conversations** 功能），而这些工具同样可用于长期监控。

一些常见的软件选项包括：

-   [http://www.plixer.com/Scrutinizer-Netflow-Sflow/scrutinizer-flow-analyzer.html](http://www.plixer.com/Scrutinizer-Netflow-Sflow/scrutinizer-flow-analyzer.html)（来自 **Plixer**）
-   [http://www.sevone.com/technologies/NetFlow-analysis](http://www.sevone.com/technologies/NetFlow-analysis)（来自 **SevOne**）

有免费软件工具，也有提供功能受限免费版本的商业工具（通常受可监控接口数量的限制）；在商业 SNMP 平台中，你通常可以获得两到五个接口的免费许可。

