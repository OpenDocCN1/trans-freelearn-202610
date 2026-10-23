

![](img/b10cb271ed113c7ba5bfded0a75d13e0_594_0.png)

Figure 13.21 – Modbus TCP/IP network with servers and clients

Note

开放协议是指任何公司或组织都可以使用的一种协议。开放协议的制造商提供该协议并允许任何人自由使用，无需任何费用或使用条件，而专有协议则是由特定制造商开发并拥有，并由其制造的设备使用的协议。

现在，让我们来学习 HART 协议。

## 常见工业网络协议 – HART

**Highway Addressable Remote Transducer (HART)** 协议是世界标准协议，提供智能/智能现场设备与控制器（PLC、DCS 等）或手持通信器之间的数据交换。你可以使用 HART 协议控制和监控 HART 启用的现场设备（传感器、变送器和执行器）。在 HART 通信中，数字信号叠加在传统的 4-20mA 模拟信号之上，使得 4-20mA 模拟信号能够与数字信号在同一双线回路上共存而不会失真。因此，同时传输两种信号——即 4-20mA 信号和数字信号。4-20mA 模拟信号表示关于温度、压力、液位或流量的测量变量，而数字信号则携带其他设备详细信息，例如设备的健康状况、状态、诊断警报等。

网络（高速公路）中的每个传感器/变送器和执行器（换能器）都有一个唯一地址，可以使用该唯一地址远程访问，因此得名 HART。我们不必在传感器位置进行配置或访问其数据。可以从控制室或 HART 数据可访问的任何地方远程完成——即从网络中任何位置用于配置和其他目的的编程 PC 或手持设备（HART 通信器）。

它是全球安装量最大的协议，其优势不言而喻。它提供了一种可靠且简单的方式与现场设备通信。它可用于各种目的，例如以下内容：

- 配置和重新配置 HART 启用的 SMART 现场设备
- 使用手持设备 SMART communicator 对 HART 启用的 SMART 现场仪表进行调试和校准
- 故障排除控制回路
- 获取有关设备健康状况和状态的详细信息
- 在线更改设备 ID 或标签

当 HART 启用的 SMART 现场设备或仪表与控制室中的 HART 兼容控制器一起使用时，可以从现场仪表获得更多益处。

除了这里解释的有线 HART 外，还有 WirelessHART。它是一种无线通信协议，可为 HART 技术增加无线能力。WirelessHART 提供了一种经济有效的方式，可在无需铺设额外线路的情况下将新的测量值传送到控制系统。

### Note

SMART 现场设备是包含处理器和其他组件的变送器或执行器。它们比传统的模拟现场设备提供更多益处，包括诊断和控制室主控制器中本应需要的其他控制功能。智能现场设备的诊断功能可以指示超出规格的操作、通信链路故障，并预测可能出现的故障。大多数 SMART 现场仪表支持 HART、Foundation Fieldbus 或 PROFIBUS 协议。SMART 是 Single Modular Auto-ranging Remote Transducer 的缩写。

HART 网络有两种类型：

- **Point-to-point**：点对点连接可在下图中看到。这里，数字信号叠加在 4-20mA 模拟信号上。测量或过程变量以 4-20mA 模拟信号传输，所有附加信息则使用数字信号传输：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_597_0.png)

Figure 13.22 – HART transmitter connected point-to-point with the HART communicator

- Multi-drop：这允许在同一对线上连接多个设备，如下图所示。Multi-drop 模式下的通信完全是数字的：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_598_0.png)

Figure 13.23 – HART transmitter connected in a multi-point mode with a PC for configuration

现在，让我们来学习最常见的工业网络。

## 常见工业网络协议 – PROFINET

PROFINET 是一种工业以太网协议，旨在工业自动化中在设备和控制器之间交换数据。它是最广泛采用的工业以太网协议。由于基于以太网，其运行速度比 PROFIBUS 更快。用于 PROFINET 的连接器是标准的 RJ45 连接器（图 13.13），网络电缆可以是标准以太网电缆，例如办公室中常见的双绞线 CAT 6 电缆（图 13.9）。标准以太网电缆

由于工业环境条件苛刻，容易受损。因此，工业以太网电缆适用于 PROFINET 使用。其特点是具有屏蔽保护、加固和适当的护套。标准以太网交换机（图 13.14）也可以使用，因为它使用与以太网相同的物理连接标准。PROFINET 现在正成为一种流行的协议，在工业中得到实施，因为其运行速度高（100 Mbps）且灵活。PROFINET 协议可使用点对点、总线或星型拓扑实现。

PROFINET 与 PROFIBUS 的一些区别如下：

- PROFINET 电缆是四芯电缆，尽管可以使用标准以太网电缆。PROFIBUS 电缆通常为紫色且有两芯。
- PROFINET 使用标准 RJ45 连接器，而 PROFIBUS 使用 PROFIBUS 连接器，看起来像标准的 DB-9 串行连接器。
- PROFINET 可以有无限设备，而 PROFIBUS 网络中的每个设备都有唯一的地址，范围为 1 到 127。因此，PROFIBUS 网络中最多可连接 127 台设备。
- PROFINET 的数据传输速率为 100 Mbps，而 PROFIBUS 的最大数据传输速率为 12 Mbps。

下图显示了使用点对点拓扑的 PLC 和 HMI 连接：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_600_0.png)

Figure 13.24 – PLC and HMI connections using the point-to-point topology

如果网络需要多于两个节点，我们可以实现星型拓扑，其中每个节点（PLC 1、PLC2、PLC 3、HMI 1、HMI 2 和任何其他设备）将连接到中央组件（交换机）。PROFINET 还支持总线拓扑。

PROFINET 网络中的设备有三个地址（IP地址、MAC 地址和设备名称）。我们在 *Chapter 10, Understanding Human Machine Interfaces (HMIs)* 中使用 PROFINET 协议实现了一个非常简单的网络，其中我们将 Siemens S7 1200 PLC（CPU 1211C）连接到 Siemens HMI（KTP 400）用于监控和控制。在这里，PLC 和 HMI，即网络中的节点，具有唯一的设备名称——即 PLC_1 和 HMI_1（*Chapter 10, Understanding Human Machine Interfaces (HMIs)*，在 *Figure 10.24* 中）。网络中的每个设备都被分配了不同的 IP 地址（PLC 为 192.168.0.4，HMI 为 192.168.0.5）。

下图显示了使用 PROFINET 连接到 HMI 用于监控和控制的 PLC。请参阅 *Chapter 10, Understanding Human Machine Interfaces (HMIs)*，以了解有关此配置的更多信息：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_601_0.png)

Siemens HMI (KTP 400)
Device name: HMI_1
IP Address: 192.168.0.5

RJ45 Connector

Siemens S7 1200 PLC (CPU 1211C)
Device name: PLC_1
IP Address: 192.168.0.4

Ethernet Cable

RJ45 Connector

Figure 13.25 – PLC connected to an HMI (point-to-point using the PROFINET protocol) (credit for this image goes to Showlight Technologies LTD: www.showlight.com.ng)

S7 1200 PLC 和 KTP400 HMI 已内置 PROFINET 端口，使得它们之间以及与其他设备使用 PROFINET 协议建立通信变得容易。配置可使用 TIA portal 轻松完成。请参阅 Chapter 10, Understanding Human Machine Interfaces (HMIs)，了解更多信息。

下图显示了通过标准以太网交换机使用 PROFINET 协议连接的两个以上设备。每个设备将具有唯一的设备名称和唯一的 IP 地址：

