

| 80/tcp | open | http |
| 44818/tcp | open | EtherNetIP-2 |
| MAC Address: 00:00:BC:5A:D0:56 | | (Rockwell Automation) |

Nmap 发现了一个开放端口 44818，这是由于使用了 -p- 标志。许多 ICS 服务运行在奇数端口号上；因此建议始终扫描所有端口。需要注意的是，这会产生大量额外的流量和额外的扫描时间，并且可能导致系统故障，但嘿，我们是在非工作时间进行此操作的，对吧？此外，如果我们不检测我们真正感兴趣的东西，扫描有什么意义呢？最后，恶意软件通常会隐藏在默认情况下不被扫描的端口后面。再次强调，这里推荐使用 -p-。

明确这一点后，让我们运行扫描：

```
pac@KVM0101011:~$ sudo nmap -iL ics_ips.txt -sV -T5 -ON
ics-services.txt -p- --dns-servers 172.25.100.100
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 14:27 MDT
Nmap scan report for 172.25.100.11
...
```

扫描需要一些时间并产生大量信息。幸好我们告诉它将结果保存到文件中，以便我们随时可以检索。

如果我们查看扫描中一些有趣的结果，我们将看到，例如，172.25.100.11 主机的以下扫描结果：

| Nmap scan report for 172.25.100.11 |
| Host is up (0.00084s latency). |
| Not shown: 65533 closed ports |
| PORT | STATE | SERVICE | VERSION |
| 80/tcp | open | http | GoAhead WebServer |
| 44818/tcp | open | EtherNet-IP-2 |
| MAC Address: 00:00:BC:5A:D0:56 | | (Rockwell Automation) |

172.25.100.11 具有所有 Allen-Bradly ControlLogix PLC 的典型特征，因为端口 80 运行着 **GoAhead webserver**，并且端口 44818 是开放的（允许 EtherNet/IP 通信）。

以下是 172.25.100.20 主机的输出：

```
Nmap scan report for 172.25.100.20
Host is up (0.00049s latency).
Not shown: 65534 closed ports
PORT     STATE SERVICE VERSION
102/tcp open  iso-tsap Siemens S7 PLC
MAC Address: 00:1C:06:00:BC:37 (Siemens Numerical Control, Nanjing)
Service Info: Device: specialized
```

172.25.100.20 似乎是一个 S7 PLC，TCP 端口 102（RFC1006）允许设备进行编程以及与其他 PLC 和远程 I/O 卡通信。

以下是 172.25.100.21 主机的输出：

```
Nmap scan report for 172.25.100.21
Host is up (0.00053s latency).
Not shown: 65534 closed ports
PORT     STATE SERVICE VERSION
502/tcp open  mbap?
MAC Address: 00:20:4A:64:1D:3F (Pronet Gmbh)
```

172.25.100.21 的 TCP 端口 502（Modbus Application Protocol 或 mbap）是开放的。结合 Pronet Gmbh 识别出的厂商，这告诉我们这很可能是一个支持 Modbus 的智能设备或通信卡。我们将在本章后面 Modbus 探索部分中查看它是什么，在那里我们将对该设备上的 Modbus 服务器进行 interrogation。

以下是 172.25.100.23 主机的输出：

```
Nmap scan report for 172.25.100.23
Host is up (0.00070s latency).
Not shown: 65534 closed ports
PORT     STATE SERVICE     VERSION
44818/tcp open  EtherNet-IP-2
MAC Address: 00:03:12:0B:02:AC (TRsystems GmbH)
```

172.25.100.23 只有一个 TCP 端口开放：44818（EtherNet/IP）。这很可能是一种通信卡——可能是远程 I/O。

| 9389/tcp | open | mc-nmf | .NET Message Framing |
| 47001/tcp | open | http | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP) |
| 49670/tcp | open | ncacn_http | Microsoft Windows RPC over HTTP 1.0 |

172.25.100.100 显然是一个 Windows 系统。它具有所有典型的开放端口，例如 135、137、139 和 445。此外，它显示出是 Active Directory 服务器的迹象，因为它在 TCP 端口 88、53、389、464 和 3268 上响应。如果我们查看 DNS 名称 OT-DC1，DC 通常代表 Domain Controller，即 Microsoft Windows Active Directory/域结构的管理员。此信息支持了我们对其为 Windows 系统的怀疑。

以下是 172.25.100.105 主机的输出：

```
Nmap scan report for FT-DIR1.ot-domain.local (172.25.100.105)
Host is up (0.00036s latency).
Not shown: 65527 filtered ports
PORT STATE SERVICE VERSION
80/tcp open http Microsoft IIS httpd 10.0
135/tcp open msrpc Microsoft Windows RPC
445/tcp open microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
808/tcp open ccproxy-http?
5985/tcp open http Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
22350/tcp open CodeMeter?
22352/tcp open unknown
49666/tcp open msrpc Microsoft Windows RPC
```

172.25.100.105 从其开放端口来看似乎是一个 Windows 客户端或成员服务器。此外，开放端口的数量表明防火墙配置严重错误或已关闭。通常，Windows 机器不会像这样暴露其高端口。

以下是 172.25.100.201 主机的输出：

```
Nmap scan report for Workstation1.ot-domain.local (172.25.100.201)
Host is up (0.00048s latency).
Not shown: 65508 closed ports
```

| PORT | STATE | SERVICE | VERSION |
|---|---|---|---|
| 80/tcp | open | http | Microsoft IIS httpd 10.0 |
| 135/tcp | open | msrpc | Microsoft Windows RPC |
| 139/tcp | open | netbios-ssn | Microsoft Windows netbios-ssn |
| 403/tcp | open | tcpwrapped |  |
| 445/tcp | open | microsoft-ds | Microsoft Windows 7 - 10 microsoft-ds (workgroup: OT-DOMAIN) |
| 1332/tcp | open | pcia-rxp-b? |  |
| 3060/tcp | open | interserver? |  |
| 3389/tcp | open | ms-wbt-server | Microsoft Terminal Services |
| 4241/tcp | open | vrml-multi-use? |  |
| 4255/tcp | open | vrml-multi-use? |  |
| 5241/tcp | open | unknown |  |
| 6543/tcp | open | mythtv? |  |
| 7153/tcp | open | unknown |  |
| 8082/tcp | open | http | MS .NET Remoting httpd (.NET CLR 4.0.30319.42000) |
| 9111/tcp | open | DragonIDSConsole? |  |
| 22350/tcp | open | CodeMeter? |  |
| 27000/tcp | open | flexlm | FlexLM license manager |
| 49664/tcp | open | msrpc | Microsoft Windows RPC |
| 49665/tcp | open | msrpc | Microsoft Windows RPC |
| 49666/tcp | open | msrpc | Microsoft Windows RPC |
| 49667/tcp | open | msrpc | Microsoft Windows RPC |
| 49668/tcp | open | msrpc | Microsoft Windows RPC |
| 49669/tcp | open | msrpc | Microsoft Windows RPC |
| 49671/tcp | open | msrpc | Microsoft Windows RPC |
| 49685/tcp | open | msrpc | Microsoft Windows RPC |
| 49690/tcp | open | flexlm | license manager |
| 49725/tcp | open | msrpc | Microsoft Windows RPC |

172.25.100.201 似乎是一个 Windows 客户端，其防火墙因与前一系统相同的原因而关闭。

以下是 172.25.100.203 主机的输出。注意 Zebra Technologies MAC 地址的识别：

```
Nmap scan report for Workstation3.ot-domain.local
(172.25.100.203)
Host is up (0.00017s latency).
Not shown: 65532 closed ports
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Microsoft Windows XP microsoft-ds
MAC Address: 40:83:DE:B2:10:48 (Zebra Technologies)
Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp
```

我们的 Zebra 系统变得更有趣了。虽然其 DNS 名称将该系统标识为工作站，但 MAC 厂商查找并不能支持这一点。查看开放端口，这似乎是一个 Windows 客户端，但结果到处都是 Windows XP 的痕迹……也许是一个 Windows 嵌入式手持设备？

我们要查看的最后一个设备有点奇怪：

```
Nmap scan report for Workstation12.OT-Domain.local
(172.25.100.212)
Host is up (0.00076s latency).
Not shown: 65534 filtered ports
PORT      STATE SERVICE VERSION
12345/tcp open  netbus?
MAC Address: 00:0C:29:EE:32:E1 (VMware)
```

172.25.100.212 似乎在端口 12345 上运行着某种服务。Nmap 将此端口识别为 Netbus，这是一种有争议的远程访问程序，很可能是恶意软件，创建于 20 世纪 90 年代末。虽然这很可能被 Nmap 误解，最终证明不是 Netbus，但它仍然是一个重大的危险信号。我们将在即将进行的活动中更密切地调查该系统。

