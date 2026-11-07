
我可以使用表达式按单个主机 IP 或主机名进行过滤：

```
tcpdump host 192.168.34.56
```

或：

```
tcpdump host www.thisismydomain.com
```

如果我们只想查找特定方向的流量，也可以指定 SRC 和 DST 主机：

```
tcpdump src 192.168.13.34
```

或：

```
tcpdump 'src 192.168.13.242 and dst 192.168.31.150'
```

![](img/095df2cd1930baecbcecaa51ee3417aa_112_0.png)

注意 AND 这个词的使用。如果要组合表达式，可以使用这些典型关键字：AND、OR 和 NOT。您也可以使用括号进行运算顺序。我在组合表达式时也倾向于使用单引号，尽管并非总是必需的。这取决于表达式的复杂程度。然而，这是一个值得养成的良好习惯。

我还可以指定整个网络。如果我不太确定要查看哪些主机或哪些主机是主要通信方，这非常方便。这是通过 CIDR 记法完成的。

```
tcpdump net 192.168.30.0/24
```

您也可以使用 /32 指定此主机：

```
tcpdump net 192.168.33.3/32
```

指定端口或协议非常重要。大多数情况下，您是在主机上查找非常具体的某样东西，或查找流量的特定端口。在这种情况下，您将使用如下过滤器：

```
tcpdump tcp
tcpdump port 22
tcpdump dst port 22
tcpdump 'tcp and dst port 22'
tcpdump 'tcp and dst port 22 and src port 53029'
```

现在，端口、协议和 IP 地址都已了解，让我们将它们全部组合成一个单一过滤器，以便更具体地进行过滤。此过滤器将查找 192.168.1.0/24 网络中 TCP 端口 22 的 SRC：

```
tcpdump 'tcp and src port 22 and net 192.168.1.0/24'
```

有时您可能不想查看流量，只是想记录下来以便稍后查看或转储到您的 C&C 服务器，该服务器我们在第 3 章《设置和维护命令与控制服务器》中进行了设置。如果是这种情况，您可以使用 -w 开关并指定文件名。我倾向于使用 .pcap 扩展名，但 .cap 和 .dmp 也比较常见：

```
tcpdump -w SSHcap.pcap 'tcp and dst port 22 and src port 53029
net 192.168.1.0/24'
```

由于我们将流量转储到文件中以便以后读取，肯定需要一种读取这些预先捕获文件的方法。您可以使用 -r 开关后跟文件名将其读入 tcpdump：

```
tcpdump -r SSHcap.pcap
```

以下是我在我的网络上运行 tcpdump 的示例。我倾向于使用 -n 标志，因为我不想解析 IP 或端口：

![](img/095df2cd1930baecbcecaa51ee3417aa_113_0.png)

## WinDump

WinDump 是 Windows 版的 tcpdump。我认为它是任何 Microsoft 环境的必备工具。有了 WinDump，您就可以直接在 Windows 机器上分析数据包，使用与您在开源端使用 tcpdump 熟悉的类似过滤器和语法。您甚至可以将 WinDump 捕获到文件中并通过 tcpdump 读取。如果您需要直接在 Windows 机器上捕获流量，此实用程序非常方便。

运行 WinDump 时，需要确保已安装 WinPcap。如果您没有 WinPcap，可以从 WinDump 获取（ http://www.windump.com ）。

WinDump 命令语法的输出如下：

```
C:\Users\jason\Downloads>windump -?
windump version 3.9.5, based on tcpdump version 3.9.5
WinPcap version 4.1.3 <packet.dll version 4.1.0.2980>, based on libpcap version 1.0 branch 1_0_rel0b <20091008>
Usage: windump [-aAdDefFlnNOpqRSStuUvxX] [ -B size ] [ -c count ] [ -C file_size ]
        -E algo::secret ] [ -F file ] [ -i interface ] [ -M secret ]
        [ -r file ] [ -s snaplen ] [ -T type ] [ -w file ]
        [ -W filecount ] [ -y datalinktype ] [ -Z user ]
        [ expression ]
```

以下屏幕截图显示了从 CLI 运行 WinDump 的示例，其过滤器与 tcpdump 非常相似：

```
C:\Users\jason\Downloads>windump -n not port 3389
windump: listening on \Device\NPF_{B04619C4-77A8-48B9-861C-5013A13CF992}
15:39:39.752131 802.1d unknown version
15:39:40.033668 IP 192.168.1.2 > 224.0.0.22: igmp v3 report, 1 group record(s)
15:39:40.035245 IP6 FE80::9C2D:65D4:B8FB:4342 > FF02::16: HBH ICMP6, multicast listener report v2, 1 group record(s), length 28
15:39:40.035348 IP 192.168.1.2 > 224.0.0.22: igmp v3 report, 1 group record(s)
15:39:40.164943 IP6 FE80::9C2D:65D4:B8FB:4342 > FF02::16: HBH ICMP6, multicast listener report v2, 1 group record(s), length 28
15:39:40.165147 IP 192.168.1.2 > 224.0.0.22: igmp v3 report, 1 group record(s)
15:39:40.283228 IP6 FE80::9C2D:65D4:B8FB:4342.51522 > FF02::1:3.5355: UDP, length 22
15:39:40.283349 IP 192.168.1.2.51522 > 224.0.0.252.5355: UDP, length 22
15:39:40.284470 IP 192.168.1.2 > 224.0.0.22: igmp v3 report, 2 group record(s)
15:39:40.284592 IP6 FE80::9C2D:65D4:B8FB:4342 > FF02::16: HBH ICMP6, multicast listener report v2, 1 group record(s), length 28
15:39:40.297034 IP 192.168.1.2.52647 > 239.255.255.250.1900: UDP, length 133
15:39:40.309121 IP 192.168.1.2.62647 > 239.255.255.250.3702: UDP, length 1079
15:39:40.309267 IP6 FE80::9C2D:65D4:B8FB:4342.62648 > FF02::C.3702: UDP, length 1095
15:39:40.432504 IP 192.168.1.2.62647 > 239.255.255.250.3702: UDP, length 1079
15:39:40.441845 IP6 FE80::9C2D:65D4:B8FB:4342.62648 > FF02::C.3702: UDP, length 1095
15:39:40.701386 IP6 FE80::9C2D:65D4:B8FB:4342.51522 > FF02::1:3.5355: UDP, length 22
15:39:40.701492 IP 192.168.1.2.51522 > 224.0.0.252.5355: UDP, length 22
17 packets captured
38 packets received by filter
0 packets dropped by kernel
```

在此示例中，我将捕获写入 win8.pcap 文件。然后，我将此文件移动到 OS X 机器上，并使用 tcpdump 的原生版本读取同一文件。这展示了不同平台之间的兼容性。

这里，我将流量写入 Windows 机器上的文件：

```
C:\Users\jason\Downloads>windump -w Win8.pcap
windump: listening on \Device\NPF_{B04619C4-77A8-48B9-861C-5013A13CF992}
137 packets captured
145 packets received by filter
0 packets dropped by kernel
```

最后，这里我们在将文件传输过来后，能够在我的 OS X 机器上读取相同的文件：

```
JABELTRA-M-V085:~ jabeltra$ tcpdump -n -r win8.pcap
reading from file win8.pcap, link-type EN10MB (Ethernet)
19:50:26.801396 IP 192.168.1.2.3389 > 192.168.33.3.64496: Flags [P.], seq 3815094950:3815094981, ack 4159911605, win 63652, options [nop,nop,TS val 4468774 ecr 1869191882], length 31
19:50:26.801671 IP 192.168.1.2.3389 > 192.168.33.3.64496: Flags [P.], seq 31:1955, ack 1, win 63652, options [nop,nop,TS val 4468774 ecr 1869191882], length 1924
19:50:26.806182 IP 192.168.33.3.64496 > 192.168.1.2.3389: Flags [.], ack 31, win 4095, options [nop,nop,TS val 1869191928 ack 4468774], length 0
19:50:26.806575 IP 192.168.33.3.64496 > 192.168.1.2.3389: Flags [.], ack 1955, win 4034, options [nop,nop,TS val 1869191928 ecr 4468774], length 0
```

## Wireshark

Wireshark 是您可用于查看网络上发生情况的最重要工具之一。它允许您查看有关此数据包的每一条您可能想要看到的信息。它从物理层开始，然后向上进行。Wireshark 最初名为 Ethereal，后来改名为 Wireshark。

Wireshark 与 tcpdump 非常相似，但围绕它有一个图形界面，并且具有以下一些附加功能：

- 可以更改和自定义的彩色编码数据包类型
- 使用插件查看新协议的能力
- 也可以捕获语音 over IP 呼叫
- 可以点击数据包并跟随流的能力

以下是 Wireshark 启动时的屏幕截图。您可以直接在过滤器框中输入捕获过滤器。其语法与 tcpdump 非常不同，但有一个自动填充功能可以帮助您构建过滤器。初始页面上的另一个非常有用的功能是它会显示可用接口以及它在这些接口上看到的图形流量。在以下示例中，只有 Ethernet0 可用：

![](img/095df2cd1930baecbcecaa51ee3417aa_116_0.png)

进入捕获屏幕后，您可以看到彩色编码如何使阅读变得更容易。在这里，我可以看到所有 RST 数据包都被着色为红色。我还可以看到 ICMPv6 数据包为粉红色，而 IGMPv3 数据包为黄色。这是一种快速读取是否有异常发生的好方法：
