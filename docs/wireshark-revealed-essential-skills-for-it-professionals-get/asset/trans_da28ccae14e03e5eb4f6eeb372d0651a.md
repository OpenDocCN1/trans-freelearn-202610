
尽可能在工作站上进行。这样做的好处是能够执行以下操作：

-   查看并验证用户所报告的实际问题
-   测量到目标服务器的往返时间
-   在会话建立时捕获并查看 TCP 握手过程
-   捕获并调查登录及任何其他后台进程和流量
-   查找用户设备所经历的网络问题（丢包和重传）的迹象
-   在数据下载期间测量到用户工作站的表观网络吞吐量
-   无需使用捕获过滤器；单个工作站的进出流量不应过大

在用户工作站、服务器或其他设备上进行捕获时，应使用聚合型**测试访问点（TAP）**，而不是使用交换机的 SPAN 端口（如[第 3 章](Chapter 3)“捕获所有正确的数据包”中所述），或者作为最后手段，在用户的工作站或服务器上安装 Wireshark（如果获得授权）。

## 排查连接问题

单个用户或小群体的连接问题可以通过确认用户工作站访问本地和远程网络资源所需的网络功能是否正常运作来解决。需要确认的基本要求或项目包括：

-   启用正确的网络接口（工作站配置）
-   确认第 1 层（物理）连接
-   获取每个接口的 IP 地址、子网掩码和默认网关（DHCP）
-   获取默认网关或其他本地网络服务的 MAC 地址（ARP）
-   获取网络服务的 IP 地址（DNS）
-   连接到网络服务（TCP 握手或 UDP 响应）

我们将按顺序简要讨论每一项；虽然前两步不涉及使用 Wireshark，但它们是故障排除方法中必要的组成部分。如果连接问题影响到一组用户或整个办公室，第一步可能不适用。

### 启用网络接口

虽然启用网络接口似乎是显而易见的，但认为接口在设备启动时默认自动启用（尤其是无线连接）的假设可能是错误的。

在 Windows 上，你可以使用命令行工具 `ipconfig` 查看网络接口的状态和基本配置（IP 地址、子网掩码和默认网关）；在 Linux 或 Mac 设备上，等效命令是 `ifconfig` 或 `ip`。

### 确认物理连接

如果连接问题仅限于单个用户的工作站，则应怀疑物理连接。需要检查几个项目，可以采取的故障排除步骤如下：

-   如果从工作站到墙上插座的以太网电缆有问题，你需要换一根不同的电缆。
-   如果从用户墙上插座到交换机端口的布线有问题，你需要将用户的以太网电缆临时插入另一个（已知正常的）墙上插座。
-   如果交换机、交换机端口或端口配置有问题，你需要将用户的端口电缆临时插入另一个（已知正常的）端口。请注意，某些网络安全策略要求禁用交换机端口，直到需要使用时才启用，或将端口配置为与单个特定 MAC 地址关联。如果是这样，端口在插入时可能无法工作，尽管物理上没有问题。

### 获取工作站 IP 配置

除非工作站是手动配置的，否则它需要从 DHCP 服务器获取 IP 地址、子网掩码、默认网关和 DNS 服务器设置。如果这看起来无法正常工作（在使用 `ipconfig`（Windows）或 `ifconfig`（Linux 或 Mac OS X）检查配置之后），你需要使用 TAP 或 SPAN 端口在工作站初始化/启动过程中执行数据包捕获，并调查 DHCP 请求和响应。

共有八种 DHCP 消息类型（不要与两种引导协议类型混淆，即引导请求和引导应答）：

| 消息类型编号 | 消息类型 | 描述 |
|---|---|---|
| 1 | DHCP 发现 | 客户端广播以定位可用的 DHCP 服务器 |
| 2 | DHCP 提供 | 服务器对 DHCP 发现的响应，以提供配置参数 |
| 3 | DHCP 请求 | 客户端发送给 DHCP 服务器的消息，用于以下任一情况：<br>- 向一台服务器请求提供的参数并拒绝其他 DHCP 服务器的报价<br>- 重新启动后确认先前分配的地址的正确性<br>- 延长 IP 地址的租约 |
| 4 | DHCP 拒绝 | 客户端发送给 DHCP 服务器的消息，表示所提供的地址不可接受 |
| 5 | DHCP 确认 | 服务器向客户端发送配置参数，包括已分配的网络地址 |
| 6 | DHCP 否定确认 | 服务器向客户端表示客户端的地址不正确或已过期 |
| 7 | DHCP 释放 | 客户端向服务器释放网络地址并取消租约 |
| 8 | DHCP 信息 | 客户端向服务器仅请求本地配置参数 |

对于正在启动且之前已在网络上正常工作的工作站，你通常会看到 DHCP 请求和确认数据包，以验证工作站仍可使用先前租用的地址。在完全冷启动时，前两个 DHCP 数据包将是 DHCP 发现和 DHCP 提供数据包，随后是请求和确认数据包。

在 DHCPv6 环境中，典型的数据包序列是 DHCPv6 请求、DHCPv6 通告、DHCPv6 请求和 DHCPv6 应答。

在 DHCP 响应数据包（或 DHCPv6 通告数据包中的类似字段）中需要验证的字段包括以下四个字段：

-   **您的（客户端）IP 地址：** 这是为此工作站提供的 IP 地址
-   **子网掩码：** 这是在此网络上使用的子网掩码
-   **域名服务器：** 这是 DNS 服务器 IP 地址
-   **路由器：** 这是要使用的默认网关的 IP 地址

这是任何网络通信所需的最少数据；下面的屏幕截图说明了在 DHCP 应答数据包中提供这些字段的示例：

```
Bootstrap Protocol (ACK)

Message type: Boot Reply (2)
Hardware type: Ethernet (0x01)
Hardware address length: 6
Hops: 0
Transaction ID: 0x28a0655c

Seconds elapsed:
[Expert Info (Note/Protocol): Seconds elapsed appears to be encoded as little-endian]
[Severity level: Note]
[Group: Protocol]

Bootp flags: 0x8000 (Broadcast)
1... .... .... .... = Broadcast flag: Broadcast

Client IP address: 0.0.0.0 (0.0.0.0)
Your (client) IP address: 192.168.1.115 (192.168.1.115)

Next server IP address: 192.168.1.1 (192.168.1.1)
Relay agent IP address: 0.0.0.0 (0.0.0.0)
Client MAC address: 00:1c:25:99:d8:85 (00:1c:25:99:d8:85)
Client hardware address padding: 00000000000000000000
Server host name: ecosystem.home.cisco.com
Boot file name not given
Magic cookie: DHCP

Option: (53) DHCP Message Type (ACK)
Length: 1
DHCP: ACK (5)

Option: (54) DHCP Server Identifier
Length: 4
DHCP Server Identifier: 192.168.1.1 (192.168.1.1)

Option: (51) IP Address Lease Time
Length: 4
IP Address Lease Time: (86400s) 1 day

Option: (58) Renewal Time Value
Length: 4
Renewal Time Value: (43200s) 12 hours

Option: (59) Rebinding Time Value
Length: 4
Rebinding Time Value: (75600s) 21 hours

Option: (1) Subnet Mask
Length: 4
Subnet Mask: 255.255.255.0 (255.255.255.0)

Option: (28) Broadcast Address
Length: 4
Broadcast Address: 192.168.1.255 (192.168.1.255)

Option: (81) Client Fully Qualified Domain Name
Length: 15
Flags: 0x03
0000 .... = Reserved flags: 0x00
.... 0... = Server DDNS: Some server updates
.... .0.. = Encoding: ASCII encoding
.... ..1. = Server overrides: override
.... ...1 = Server: Server
A-RR result: 255
PTR-RR result: 255
Client name: ThinkPadW500

Option: (6) Domain Name Server
Length: 4
Domain Name Server: 192.168.1.1 (192.168.1.1)

Option: (3) Router
Length: 4
Router: 192.168.1.1 (192.168.1.1)

Option: (255) End
Option End: 255
```
