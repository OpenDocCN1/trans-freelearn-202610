
再次强调，输出为我们提供了有关被扫描主机的重要信息。

现在我们知道了ICS网络上运行的每个Windows操作系统的详细信息。这为我们编制资产清单奠定了良好基础，因为我们可以将这些信息解析到Excel表格或数据库中。现在，我们可以开始填充已安装软件、文件、用户等细节，以及我们能发现的任何其他信息。

我们还可以使用更多的SMB脚本，但其中许多在现代Windows版本上结果有限。我将这些脚本的探索留作课后作业。我们将通过对ICS网络上所有Windows机器执行一次Nmap扫描来结束SMB这一部分，该扫描请求获取用户列表。

## 用户

了解系统上存在哪些用户是攻击者破解凭据的第一步。因此，如果我们的任何机器让这一信息过于容易泄露，我们就要了解这一点。`smb-enum-users.nse` Nmap脚本可以让我们查明我们的任何系统是否暴露了用户详细信息。要对所有Windows机器运行它，可以使用以下命令：

```
pac@KVM0101011:~$ sudo nmap -iL ics_ips.txt -p 445 --open -T3 --script smb-enum-users
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 17:25 MDT
Nmap scan report for 172.25.100.100
Host is up (0.00020s latency).

PORT     STATE SERVICE
445/tcp  open  microsoft-ds
MAC Address: 00:02:BA:D9:A7:41 (Cisco Systems)

Nmap scan report for 172.25.100.105
Host is up (0.00017s latency).

PORT     STATE SERVICE
445/tcp  open  microsoft-ds
MAC Address: 00:13:20:03:3F:B8 (Intel Corporate)
...
Nmap scan report for 172.25.100.220
```

```
Host is up (0.00086s latency).

PORT     STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:FA:9D:2F (VMware)

Host script results:
|_smb-enum-users: ERROR: Script execution failed (use -d to debug)

Nmap done: 16 IP addresses (16 hosts up) scanned in 0.79 seconds
```

我们的系统都没有返回用户账户信息，这是好事。不过，攻击者还有其他方法可以从Windows机器枚举用户。其中一种方法是通过RPC的NULL会话。下面是从我们的Kali Linux机器针对OT-DC1服务器执行该方法的示例：

```
pac@KVM0101011:~$ sudo rpcclient -U "" 172.25.100.100
Enter WORKGROUP\'s password:
rpcclient $> getusername
Could not initialise lsarpc. Error was NT_STATUS_ACCESS_DENIED
rpcclient $> enumdomusers
result was NT_STATUS_ACCESS_DENIED
rpcclient $> lsaquery
Could not initialise lsarpc. Error was NT_STATUS_ACCESS_DENIED
rpcclient $> ...
...
```

如你所见，尽管服务器允许我们匿名连接到RPC服务，但在后续任何查询时都会返回“拒绝访问”错误。RpcClient是一个很棒的工具，可以揭示各种信息，因此我强烈建议你多研究一下它，看看网络上的其他系统是否更愿意透露信息。

接下来，我们将转换方向，看看如何开始探测工业协议/服务，首先是Modbus。

## 探索 Modbus

你可能还记得，在我们的网络扫描中，172.25.100.21显示TCP端口502开放，这表明存在并运行着一个支持Modbus的服务。让我们使用Nmap脚本看看该端口上到底是什么服务在监听。如果我们在Nmap脚本中搜索，就会看到只有一个关于Modbus的脚本：

```
pac@KVM0101011:~$ ls /usr/share/nmap/scripts/ | grep modbus
-rw-r--r-- 1 root 5.9K Jul 13 03:03 modbus-discover.nse
```

这个脚本正是我们需要的，可以弄清开放的TCP端口502上运行的是什么。可以使用以下Nmap命令来揭示该开放端口背后的应用程序：

```
pac@KVM0101011:~$ nmap -p 502 -T3 172.25.100.21 --script modbus-discover
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-31 15:13 MDT
Nmap scan report for 172.25.100.21
Host is up (0.00096s latency).

PORT     STATE SERVICE
502/tcp open  modbus
| modbus-discover:
|   sid 0x1:
|     Slave ID data: Pymodbus-PM-2.3.0\xFF
|_    Device identification: Pymodbus PM 2.3.0
MAC Address: 00:20:4A:64:1D:3F (Pronet Gmbh)

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
```

在这里，我们可以看到该服务是Pymodbus PM 2.3.0，这是我们实验室设备上运行的Python Modbus实现。如果我们查看运行Pymodbus服务的机器上的日志，就能发现Nmap是如何发现上文显示的那些细节的：

```
root@ubuntu:/home/pac/workdir/python/modbus# python3 ./modbus-server.py
2020-08-31 14:13:19,327 MainThread      DEBUG    sync :347    Started thread to serve client at ('172.25.100.222', 60272)
2020-08-31 14:13:19,328 Thread-1         DEBUG    sync
:46          Client Connected [172.25.100.222:60272]
2020-08-31 14:13:19,329 Thread-1          DEBUG      sync
:199          Handling data: 0x0 0x0 0x0 0x0 0x2 0x1 0x11
2020-08-31 14:13:19,329 Thread-1          DEBUG      socket_framer
:147          Processing: 0x0 0x0 0x0 0x0 0x2 0x1 0x11
2020-08-31 14:13:19,329 Thread-1          DEBUG      factory
:137          Factory Request [ReportSlaveIdRequest: 17]
2020-08-31 14:13:19,330 Thread-1          DEBUG      sync      :229      send:
[ResportSlaveIdResponse(17, b'Pymodbus-PM-2.3.0', True)]-b'00000000001501111250796d6f646275732d504d2d322e332e30ff'
2020-08-31 14:13:19,332 Thread-1          DEBUG      sync
:199          Handling data:
2020-08-31 14:13:19,332 Thread-1          DEBUG      socket_framer
:147          Processing:
2020-08-31 14:13:19,332 Thread-1          DEBUG      sync
:54           Client Disconnected [172.25.100.222:60272]
2020-08-31 14:13:19,333 MainThread          DEBUG      sync
:347          Started thread to serve client at ('172.25.100.222', 60274)
2020-08-31 14:13:19,334 Thread-2          DEBUG      sync
:46           Client Connected [172.25.100.222:60274]
2020-08-31 14:13:19,334 Thread-2          DEBUG      sync
:199          Handling data: 0x0 0x0 0x0 0x0 0x5 0x1 0x2b 0xe 0x1 0x0
2020-08-31 14:13:19,335 Thread-2          DEBUG      socket_framer
:147          Processing: 0x0 0x0 0x0 0x0 0x5 0x1 0x2b 0xe 0x1 0x0
2020-08-31 14:13:19,335 Thread-2          DEBUG      factory
:137          Factory Request [ReadDeviceInformationRequest: 43]
2020-08-31 14:13:19,335 Thread-2          DEBUG      sync
:229          send: [ReadDeviceInformationResponse(1)]- b'00000000 001d012b0e0183000003000850796d6f646275730102504d0205322e332e30'
2020-08-31 14:13:19,337 Thread-2          DEBUG      sync
:199          Handling data:
2020-08-31 14:13:19,337 Thread-2          DEBUG      socket_framer
:147          Processing:
2020-08-31 14:13:19,338 Thread-2          DEBUG      sync
:54           Client Disconnected [172.25.100.222:60274]
```

我们的Kali Linux机器（**172.25.100.222**）两次连接到Modbus服务。第一次，我们请求了一个值为17的`SlaveIDRequest`，随后又请求了一个值为43的`DeviceInformationRequest`。这些数据足以让Nmap给出我们之前读到的结果。

尽管Nmap返回的信息（供应商、类型、版本和修订号）足以让我们判断该系统是否存在已知漏洞，但我想指出，Modbus协议本身还存在一些固有的漏洞。这些漏洞源于Modbus协议的两个架构缺陷：不提供传输加密，也不要求通信身份验证。由于这两个根本性设计缺陷，攻击者可以嗅探网络，了解（关键的）内存寄存器，并对这些寄存器进行读写。为了说明这些漏洞的影响，下面的示例展示了一个攻击者通过Wireshark在工业网络上嗅探某个寄存器（标记字）的读取过程：

![](img/c8ffa9959721097701584fcc95101362_279_0.png)

注意在上述截图中，可以看到**172.25.100.222**在第4个数据包中请求一个寄存器值，而Modbus服务（**172.25.100.21**）在第*6个数据包*中发送响应（在上述截图中已高亮显示）。在数据包详情中，我们可以看到寄存器编号（**10**）及其值（**34**）。

有了这些信息，攻击者现在可以指示Modbus服务按照自己的意愿更改这个值。我们可以用来执行此操作的一个可行工具是**modbus-cli**，接下来将对其进行说明。
