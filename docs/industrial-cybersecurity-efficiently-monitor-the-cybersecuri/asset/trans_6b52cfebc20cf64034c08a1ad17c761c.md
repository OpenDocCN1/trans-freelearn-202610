

与之前命令的主要区别在于增加了profile声明`--profile=WinXPSP2x86`，告诉Volatility这个内存转储来自32位Windows XP Service Pack 2系统。`pslist` Volatility插件的输出向我们展示了内存转储时系统正在运行哪些进程。它还显示了发现痕迹的内存映像偏移量，以及痕迹特有的详细信息，如进程ID（PID）、父进程ID（PPID）、进程句柄、线程、会话，以及进程启动和退出时间（如果已知）。高亮显示的`telnet.exe`进程是我们在本章前面的`netstat`命令中观察到的那个进程。它仅用作整体说明的示例，对我们的调查结果没有进一步影响。

为了说明进程之间如何关联，考虑一下`telnet.exe`进程，它的PID为1300（稍后用于与其他痕迹关联），PPID为472。从输出中，我们可以看到`telnet`的父进程PID 472与`cmd.exe`进程（在上方一行）关联。而`cmd.exe`进程的PPID为176，指向`explorer.exe`进程，即用户环境所运行的进程（用户shell）。最后要指出的一点是，`explorer.exe`进程的PPID为2020，但不存在具有该PID的进程。这种情况发生在父进程在子进程启动后被终止时。这有时可能表明可疑行为，具体取决于子进程是什么。另一个有助于说明进程间父子关系的Volatility插件是`pstree`，如下所示：

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem --profile=WinXPSP2x86 pstree
Volatility Foundation Volatility Framework 2.6.1
Name                          Pid  PPid  Thds  Hnds  Time
------------------------------ ---- ---- ----- ---- ----
0x823c8830:System                     4     0
57     248 1970-01-01 00:00:00 UTC+0000
. 0x821acda0:smss.exe                 376     4
3      21 2020-09-24 01:00:18 UTC+0000
.. 0x82132020:winlogon.exe            556   376
18     543 2020-09-24 01:00:18 UTC+0000
... 0x82204020:services.exe           696   556
16     271 2020-09-24 01:00:18 UTC+0000
...
```

注意系统进程是`smss.exe`进程的父进程，而`smss.exe`又是`winlogon.exe`进程的父进程，依此类推。

接下来我们要介绍的Volatility插件是用于显示套接字的`sockets`：

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem --profile=WinXPSP2x86 sockets
Volatility Foundation Volatility Framework 2.6.1
Offset(V)      PID  Port  Proto Protocol          Address
Create Time
---------- ---------- ------ ------ ------------ ------------
--- --- 
0x822d4b40      708    500    17 UDP             0.0.0.0
2020-09-24 01:00:26 UTC+0000
0x81b4d9a8     1152   1900    17 UDP
172.25.100.220 2020-09-24 01:00:31 UTC+0000
0x822c4e98        4    445     6 TCP             0.0.0.0
2020-09-24 01:00:17 UTC+0000
0x81b68390      956    135     6 TCP             0.0.0.0
2020-09-24 01:00:18 UTC+0000
0x81f9e6e8        4    139     6 TCP
172.25.100.220 2020-09-24 01:00:18 UTC+0000
...
0x81b5fe98      708   4500    17 UDP             0.0.0.0
2020-09-24 01:00:26 UTC+0000
0x81b62570     1300   1036     6 TCP
172.25.100.250          2020-09-26 17:15:42 UTC+0000
0x821a7128        4    445    17 UDP             0.0.0.0
2020-09-24 01:00:17 UTC+0000
```

该命令显示了HMI-2计算机在我们创建内存镜像时所建立的（活动）网络连接（套接字）。大多数是内部（回环）连接，但我们也可以看到一条到172.25.100.250（Security Onion VM）的TCP端口22（SSH）连接，创建于9月26日17:15:42，由ID为1300的进程（我们之前看到的`telnet.exe`进程）发起。

在内存转储文件中查找活动连接的第二种方法是使用`connections`命令：

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem
--profile=WinXPSP2x86 connections
Volatility Foundation Volatility Framework 2.6.1
Offset(V)  Local Address          Remote Address
Pid
...
```

```
...
Rule: r1
Owner: Process svchost.exe Pid 1040
0x0352732c 6d616c69636f6f75732d7765622736974 malicious-websit
0x0352733c 652e636f6d2f73657276652e68746d6c e.com/serve.html
...
Rule: r1
Owner: Process svchost.exe Pid 1040
0x1000602d 6d616c69636f6f75732d7765622736974 malicious-websit
0x1000603d 652e636f6d000000000000000000000000 e.com..........
...
```

我们在这里看到的是Volatility检测到内存中几处存储“malicious”文本的位置。注意，所有出现位置都在同一个所有者——PID为1040的`svchost.exe`进程——的内存地址范围内。Windows将`svchost.exe`进程用作Windows服务的宿主进程。那么，让我们使用`svcscan` Volatility命令看看能否在内存转储中找到可疑服务：

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem --profile=WinXPSP2x86 svcscan | grep 1040 -B3 -A5
Volatility Foundation Volatility Framework 2.6.1
Offset: 0x662ac8
Order: 23
Start: SERVICE_AUTO_START
Process ID: 1040
Service Name: AudioSrv
Display Name: Windows Audio
Service Type: SERVICE_WIN32_SHARE_PROCESS
Service State: SERVICE_RUNNING
Binary Path: C:\WINDOWS\System32\svchost.exe -k netsvcs
--
Offset: 0x662cf8
Order: 27
Start: SERVICE_AUTO_START
Process ID: 1040
Service Name: Browser
Display Name: Computer Browser
Service Type: SERVICE_WIN32_SHARE_PROCESS
Service State: SERVICE_RUNNING
Binary Path: C:\WINDOWS\System32\svchost.exe -k netsvcs
--
Offset: 0x6634c0
Order: 41
Start: SERVICE_AUTO_START
Process ID: 1040
Service Name: CryptSvc
Display Name: Cryptographic Services
Service Type: SERVICE_WIN32_SHARE_PROCESS
Service State: SERVICE_RUNNING
Binary Path: C:\WINDOWS\System32\svchost.exe -k netsvcs
--
Offset: 0x663678
Order: 44
Start: SERVICE_AUTO_START
Process ID: 1040
Service Name: Dhcp
Display Name: DHCP Client
Service Type: SERVICE_WIN32_SHARE_PROCESS
Service State: SERVICE_RUNNING
Binary Path: C:\WINDOWS\System32\svchost.exe -k netsvcs
...
```

当我们运行此命令时，我们会看到一长串服务，它们都使用PID为1040的宿主进程`svchost.exe`。这是旧版Windows系统中的常见细微差别（Windows 7及更高版本在限制每个宿主进程实例托管的服务数量方面做得更好）。服务通过`svchost`等宿主进程实现的最常见方式是让宿主进程加载一个包含所有服务代码的dll（动态链接库的简称），因此，让我们使用`dlllist` Volatility命令查看进程1040加载的所有dll：

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem --profile=WinXPSP2x86 dlllist -p 1040
Volatility Foundation Volatility Framework 2.6.1
****************************************************************
********
svchost.exe pid: 1040

Command line : C:\WINDOWS\System32\svchost.exe -k netsvcs

Service Pack 2

Base          Size     LoadCount LoadTime
Path
---------- ---------- ---------- -------------------------------------------
-- ----
0x01000000   0x6000     0x1ffff
C:\WINDOWS\System32\svchost.exe
0x7c900000   0xb0000    0x1ffff
C:\WINDOWS\system32\ntdll.dll
0x7c800000   0xf4000    0x1ffff
C:\WINDOWS\system32\kernel32.dll
0x77d00000   0x9b000    0x1ffff
C:\WINDOWS\system32\ADVAPI32.dll
0x77e70000   0x91000    0x1ffff
C:\WINDOWS\system32\RPCRT4.dll
0x774e0000   0x13c000   0x9b
C:\WINDOWS\system32\ole32.dll
0x77c10000   0x58000    0x2b5
C:\WINDOWS\system32\msvcrt.dll
0x77120000   0x8c000    0x68
C:\WINDOWS\system32\OLEAUT32.dll
0x77c00000   0x8000     0x23
C:\WINDOWS\system32\VERSION.dll
0x7c9c0000   0x814000   0xf
C:\WINDOWS\system32\SHELL32.dll
0x77f60000   0x76000    0x40
C:\WINDOWS\system32\SHLWAPI.dll
...
```

同样，我们面对的是一大串命令输出。该进程加载了大量dll；这是一个繁忙的服务宿主进程。如果我们仔细查看输出，可以看到每个dll都加载在各自的内存区域中；并且，回顾之前的字符串搜索，每个出现也都对应一个内存位置。我们将把两者关联起来：在dll列表中搜索加载内存位置与已发现字符串位置（这些位置以0x00、0x01、0x03和0x10开头）重叠的条目：

```
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem
--profile=WinXPSP2x86 dlllist -p 1040 | grep 0x00
Volatility Foundation Volatility Framework 2.6.1
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem
--profile=WinXPSP2x86 dlllist -p 1040 | grep 0x01
Volatility Foundation Volatility Framework 2.6.1
0x01000000    0x6000    0xffff
C:\WINDOWS\System32\svchost.exe
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem
--profile=WinXPSP2x86 dlllist -p 1040 | grep 0x03
Volatility Foundation Volatility Framework 2.6.1
remnux@remnux:~/workdir$ vol.py -f HMI-2.vmem
--profile=WinXPSP2x86 dlllist -p 1040 | grep 0x10
Volatility Foundation Volatility Framework 2.6.1
0x73d0000     0x102000    0x9
C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64
144ccf1df_6.0.2600.2180_x-ww_a84f1ff9\comctl32.dll
0x76360000    0x10000     0x11
C:\WINDOWS\System32\WINSTA.dll
0x76f20000    0x27000     0x10
c:\windows\system32\DNSAPI.dll
0x606b0000    0x10d000    0x4
c:\windows\system32\ESENT.dll
0x10000000   0x13000     0x1
c:\windows\temp\ipripa.dll
0x73030000    0x10000     0x1
c:\windows\system32\WZCSAPI.DLL
remnux@remnux:~/workdir$
```

