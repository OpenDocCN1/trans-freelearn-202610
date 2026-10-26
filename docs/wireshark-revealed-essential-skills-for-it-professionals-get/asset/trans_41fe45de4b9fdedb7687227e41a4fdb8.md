
## HTTP 调试器

HTTP 调试器是提供有关 HTTP 的统计和详细数据的工具。以下是一些可用于此目的的工具：

| 供应商 | 软件名称 | 下载地址 | 备注 |
|---|---|---|---|
| Eric Lawrence 和 Telerik | Fiddler | [http://fiddler2.com/](http://fiddler2.com/) | 最常见的免费 HTTP 调试工具，它是一款独立软件，用于捕获数据包并对其进行分析（如 Wireshark）。 |
| Simtec Limited | HTTPWatch | [http://www.httpwatch.com/](http://www.httpwatch.com/) | 分为基础版、受限版和商业版。可作为 Firefox 或 Internet Explorer 的附加组件使用。文件可用 HTTP Watch Studio 打开。也适用于 iPhone iOS。 |

使用这些工具，您将获得 HTTP 统计和性能信息，例如打开网页所需的时间、延迟原因以及错误摘要。

## Syslog

Syslog（https://tools.ietf.org/html/rfc5424）是一种用于消息日志记录的协议。通信设备上有许多可以配置的参数，因此当出现问题时，会向 Syslog 服务器发送消息。这些问题通常是硬件和软件方面的，SNMP 并不总是能覆盖到。

一个优秀的 Syslog 服务器（用于接收消息并呈现它们）可以在 http://www.kiwisyslog.com/free-edition.aspx 找到。还有许多其他工具，它们在许多管理平台中都是免费提供的。

## 其他工具

您可能需要的一些其他网络工作工具如下：

- 一个简洁的工具 **Xplico**，用于提取捕获文件中包含的应用程序数据：[http://www.xplico.org/about](http://www.xplico.org/about)（免费软件）
- **Nmap** 安全扫描器：[http://nmap.org/](http://nmap.org/)
- 适用于 Linux 的 **Netcat**（nc）：[http://nc110.sourceforge.net/](http://nc110.sourceforge.net/)

## 有趣的网站

以下是我经常使用的一些有趣的网站：

- 首先也是最有用的网站是 http://www.cisco.com，您可以在其中学习相关技术以及如何在 Cisco 设备中进行配置。
- 可以在 http://www.honeynet.org/ 找到许多 Wireshark 文件、练习和挑战。
- 可以在 http://packetlife.net/media/library/13/Wireshark_Display_Filters.pdf 找到 Wireshark 过滤器摘要表。
- 可以在 http://wiki.wireshark.org/SampleCaptures 找到捕获和归档的示例。
- 另一个有趣的 Wireshark 过滤器页面是 http://www.packetlevel.ch/html/tcpdumpf.html
- 可以在 http://perldoc.perl.org/perlre.html 找到一些 Perl 相关内容，Perl 正则表达式可以在 http://perldoc.perl.org/perlre.html#Regular-Expressions 和 http://www.regular-expressions.info/perl.html 找到。

## 书籍

以下是我多年来使用过的一些书籍列表：

- 要了解 TCP/IP 基础知识：《TCP/IP 详解（卷 1）：协议（第二版）》（Addison-Wesley Professional Computing Series 系列）
- 一本全面的、带插图的互联网协议参考书，互联网上免费提供：《TCP/IP 指南》（http://www.tcpipguide.com/）
- Cisco Press 的许多书籍都值得一读，包括技术和认证类书籍：http://www.ciscopress.com/
- Cisco 设计指南：只需在 Google 上搜索您要查找的主题。

## 第3部分 模块3

### 精通 Wireshark

通过精通 Wireshark，像专业人士一样分析数据网络——从 0 到 1337

### 第1章 欢迎来到使用 Wireshark 进行数据包分析的世界

本章向您介绍 TCP/IP 模型的基础知识，并通过一个示例数据包捕获让您熟悉 Wireshark 的图形用户界面（GUI）。您将了解以下主题：

- 什么是 Wireshark？
- 它是如何工作的？
- TCP/IP 模型简要概述
- 数据包分析简介
- 为什么使用 Wireshark？
- 了解 Wireshark 的 GUI
- 首次数据包捕获

### Wireshark 简介

Wireshark 是最先进的数据包捕获软件之一，它让系统/网络管理员的工作变得轻松，并证明了它在安全倡导者群体中的价值。Wireshark 又被称为协议分析器，可帮助 IT 专业人员调试网络层问题。该工具在优化网络性能方面非常有用。

Wireshark 的核心工作是剖析网络层数据包，并根据用户的需求向其展示数据包详细信息。如果您每天都需要处理数据包级网络问题，那么 Wireshark 非常适合您，它可用于多种故障排查目的。

### TCP/IP 模型简要概述

接下来，我们来讨论网络世界中最重要的主题。为了理解所有这些内容如何联系在一起，我们需要了解 TCP/IP 模型的基础知识。即使是计算机世界也需要一套通信规则和条例，而这由网络协议负责处理，这些协议管理数据包/段/帧在主机之间专用信道上的传输。

TCP/IP 模型最初被称为 DoD 模型，该项目由美国国防部监管。TCP/IP 模型负责数据包生命周期的方方面面，即：数据包如何生成，单个数据包如何附加所需的信息集合（PDU），数据包如何传输，如何开始运作，如何通过中间节点路由到目的地，如何与其他数据包重新整合以得到完整信息，等等。

如果您对网络协议的基础知识有任何疑惑，我建议您先快速回顾一下再继续阅读，因为本书需要您熟悉 TCP/UDP 协议。等您回来后，您将能够自己设想并回答所有这些问题。

第二层是**传输层**。该层的唯一目的是创建套接字，使两台主机能够通过它们进行通信（您可能已经知道网络套接字的重要性），这对于在两台设备之间建立单独连接至关重要。

同一时刻，两台主机之间可能存在多个连接。IP 地址和端口号共同实现了这一点。当我们讨论基于 WAN 的通信时，需要 IP 地址（在基于 LAN 的通信中，实际数据传输通过 MAC 地址进行）。如今，单个系统可以借助端口号通过多个信道与多个设备进行通信。除了受限制的端口号范围外，每个系统都可以自由指定一个随机端口用于通信。

该层也是两台主机之间通信的骨干。工作在该层的最常见协议是 TCP 和 UDP，具体说明如下：

- **TCP：**这是一种面向连接的协议，通常被称为可靠协议。首先，在两台主机之间建立专用信道，然后传输数据。接着，发送方通过专用信道发送均分的数据块，接收方对收到的每个数据块发送确认。最常见的情况是，发送方会等待特定时间，然后再次发送相同的数据块以确保传输。例如，如果您正在下载某些内容，TCP 会负责确保每个比特都成功传输。
- **UDP：**这是一种无连接协议，通常被称为不可靠的通信形式。但它很简单，因为没有创建专用信道，发送方只关心将数据块发送到目的地，无论是否被接收。这种通信形式实际上不会影响通信质量；将比特从发送方传输到接收方这一唯一目的已经实现。例如，如果您正在玩基于局域网的游戏，丢失几个字节不会破坏您的游戏体验，因此用户体验不会受到损害。

第三层是**互联网层**，负责数据的往返传输。该层工作的主要协议是 **IP（互联网协议）**，这是该层最重要的协议。IP 提供路由功能，使得特定数据包能够到达其目的地。该层包含的其他协议还有 ICMP 和 IGMP。
