
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49156/tcp open  unknown
49167/tcp open  unknown
49175/tcp open  unknown

这些开放端口大多是典型的 Windows 服务，例如 3389 号端口上的远程桌面服务，以及 135、139 和 445 号端口上的文件和打印机共享。看到较高的端口号（49152 及以上）开放很有趣，但并不罕见。它们是远程过程调用端点服务，通常在大多数 Windows 计算机上都能找到。像这样将它们全部暴露出来，表明这台 Windows 机器上的防火墙要么已关闭，要么添加了过多的端口例外。

现在我们可以将这些结果与我们执行过的基线扫描进行比较（实际上我们没有，但我们可以假设我们在 2019 年该计算机首次部署时进行过一次扫描）。如果在 Notepad++ 中比较这两组结果，我们会看到以下差异：

![](img/c8ffa9959721097701584fcc95101362_535_0.png)

查看比较结果，我们应该注意到当前暴露的网络端口与基线扫描结果之间存在以下差异：

8082/tcp  open  blackice-alerts
12345/tcp  open  netbus
27000/tcp  open  flexlm0
49152/tcp  open  unknown
49153/tcp  open  unknown
49154/tcp  open  unknown

此时，我们会获取可执行文件的副本，查找详细信息，并开始在整个环境中搜索其他运行了此服务/进程/可执行文件的系统。这正是我们将在接下来的章节中要做的事情：*利用已发现的失陷指标搜索环境中其他可疑系统。* 不过，鉴于我们实际上没有可供比较的基线，我们现在将讨论一些在端点上查找恶意可执行文件的不同方法。正如比较系统清单快照一样，如果你有基线——即对已知安全（干净）系统进行的端口扫描——那么比较系统的开放端口最为有效。如果没有，你将不得不筛选掉所有已知正常（安全）的端口，然后调查剩余的端口。这个过程依赖于大量经验、大量网络搜索以及大量上传到**VirusTotal**的操作。

现在，让我们看看另一种查找不需要的或恶意可执行文件的方法，即通过（远程）查询系统当前正在运行的应用程序和启动应用程序。

## 盘点 ICS 环境中当前正在运行的进程

在本示例中，我们将在域控制器（**OT-DC1**）上使用 PowerShell 命令。我们将结合使用 psexec.exe 和简单的 PowerShell 命令，从所有域主机检索进程信息。我选择使用 psexec.exe 而不是完全依靠 PowerShell（例如 Invoke-Command），是因为我们的域中有 Windows XP 计算机，它们不支持 PowerShell 远程功能。

让我们从 PowerShell 命令的小教程开始。以下 PowerShell 命令通过查询 **Active Directory (AD)** 并处理输出以仅显示计算机名称，从而列出所有域计算机：

```
PS C:\workdir> get-adcomputer -filter * | select
-ExpandProperty Name
OT-DC1
FT-DIR1
FT-DIR2
WORKSTATION2
WORKSTATION10
WORKSTATION1
WORKSTATION12
HMI-1
HMI-2
```

在此命令中，get-adcomputer -filter * 会查询 AD 数据库中所有域计算机；我们将该命令的输出通过管道（|）传递给 select ExpandProperty Name，以剥离除计算机名称外的所有数据。

前一个 PowerShell 命令的输出可用于 foreach 循环，在循环内我们可以运行以远程计算机名称作为变量的命令，例如 psexec.exe 工具。为了说明这一点，下面是一个 PowerShell 示例命令，通过 psesec.exe 工具在所有域计算机上运行 Windows 自带的 hostname.exe 命令：

```
PS C:\workdir> get-adcomputer -filter * | select
-ExpandProperty Name | foreach { .\PsExec.exe -nobanner \\$_
hostname }

FT-DIR1
hostname exited on FT-DIR1 with error code 0.
FT-DIR2
hostname exited on FT-DIR2 with error code 0.
Workstation2
hostname exited on WORKSTATION2 with error code 0.
Workstation10
hostname exited on WORKSTATION10 with error code 0.
Workstation1
hostname exited on WORKSTATION1 with error code 0.
Workstation12
hostname exited on WORKSTATION12 with error code 0.
HMI-1
hostname exited on HMI-1 with error code 0.
HMI-2
hostname exited on HMI-2 with error code 0.
PS C:\workdir>
```

正如你所见，这种方法适用于域中所有 Windows 版本，从 Windows XP 到 Windows 10 和 Server 2016。psexe.exe 命令使用了 -nobanner 标志，以抑制每次迭代时显示标题文本。在 PowerShell 的 foreach 循环中，$_ 用于遍历 foreach 循环之前命令的管道输出，即 get-adcomputer -filter * | select -ExpandProperty Name，从而使 psexec 命令遍历所有域计算机名称。

你可以看到此输出中有一些有用的信息。我们可以通过添加 /v 标志来扩展详细信息，以显示进程所有者等信息：

```
PS C:\workdir> get-adcomputer -filter * | select
-ExpandProperty Name | foreach { write $_ ; .\PsExec.exe
-nobanner \\$__ tasklist /v}
FT-DIR1

Image Name PID Session Name Session# Mem Usage Status
User Name CPU Time Window Title
===== ===== ============= ====== ============= ======= =========
System Idle Process 0 Services 0 4 K Unknown NT
AUTHORITY\SYSTEM 2:31:10 N/A
System 4 Services 0 136 K Unknown N/A
0:00:14 N/A
smss.exe 296 Services 0 1,204 K Unknown NT
AUTHORITY\SYSTEM 0:00:00 N/A
csrss.exe 396 Services 0 5,004 K Unknown NT
AUTHORITY\SYSTEM 0:00:01 N/A
...
```

我们甚至可以添加 /svc 标志，以显示与正在运行的进程关联的服务，这对于查看那些恼人的 svchost.exe 进程中运行了什么非常方便：

```
...
FT-DIR1

Image Name PID Services
===== ===== =====
...
svchost.exe 708 RpcEptMapper, RpcSs
svchost.exe 844 BITS, DsmSvc, gpsvc,
IKEEXT, iphlpsvc,
ProfSvc, Schedule, SENS,
ShellHWDetection
svchost.exe 928 Dhcp, EventLog, lmhosts
...
```

这些都是非常有用且可以作为基线或快照保存的信息。不过，为了在追踪恶意可执行文件时更轻松，我们需要一条关键信息：进程启动的位置，即路径。要获得该详细信息，可以在目标计算机上运行 wmic，这将在接下来讨论。

## 搜索可疑的进程可执行文件路径

正如我们之前讨论过的，查看进程的启动位置（可执行文件的位置/路径）可以暴露恶意应用程序。我们可以使用 Windows 自带工具——Windows 管理和规范命令（wmic.exe），通过 PowerShell 迭代命令（foreach）获取进程及其路径的列表：

```
PS C:\workdir> get-adcomputer -filter * | select -ExpandProperty Name | foreach { write $_ ; .\PsExec.exe -nobanner \\$_ wmic process get ProcessID,ExecutablePath }
FT-DIR1
```

| 可执行文件路径 | 进程 ID |
|---|---|
| C:\Windows\system32\winlogon.exe | 488 |
| C:\Windows\system32\lsass.exe | 576 |
| C:\Windows\system32\svchost.exe | 656 |
| C:\Windows\system32\svchost.exe | 708 |
| C:\Windows\system32\LogonUI.exe | 792 |
| C:\Windows\system32\dwm.exe | 804 |
| C:\Windows\system32\svchost.exe | 844 |
| ... |  |

结果应粘贴（或通过管道输出）到文本文件中（> Domain-Computers_Running-processes_20201212.txt）。

是时候挖掘可疑内容了。首先，我们将使用 grep 搜索进程文件，查找可执行文件从非常见位置启动的情况。我们执行的第一项搜索是针对 temp 字符串，以查看环境中是否有从临时目录中的可执行文件启动的正在运行的进程：

```
pac@KVM0101011:~/book$ grep -i temp Domain-Computers_Running-processes_20201212.txt
pac@KVM0101011:~/book$
```

没有结果。那么 tmp 呢？

```
pac@KVM0101011:~/book$ grep -i tmp Domain-Computers_Running-processes_20201212.txt
pac@KVM0101011:~/book$
```

仍然没有发现。让我们改变策略，转而查找已知位置以外的任何位置：

```
pac@KVM0101011:~/book$ grep -n -i -v 'program files\|windows\\system\|Windows Defender\|syswow\|framework64' Domain-Computers_Running-processes_20201212.txt
...
3:FT-DIR1
43:C:\Windows\Sysmon64.exe          2104
66:C:\PROGRA~2\ROCKWE~1\RSLinx\RSLINX.EXE  3552
85:C:\Windows\PSEXESVC.exe          2084
...
91:FT-DIR2
133:C:\Windows\Sysmon64.exe         2464
165:C:\Windows\Explorer.EXE         5400
179:C:\Windows\PSEXESVC.exe         2312
...
185:WORKSTATION2
273:C:\Windows\Sysmon64.exe         3604
317:C:\Windows\PSEXESVC.exe         8876
...
323:WORKSTATION10
367:C:\Windows\Sysmon64.exe         2248
398:C:\Windows\Explorer.EXE         3740
406:C:\Windows\PSEXESVC.exe         6540
...
412:WORKSTATION1
478:C:\Windows\Sysmon64.exe         3100
546:C:\Windows\Explorer.EXE         5956
568:C:\Users\engineer-1\AppData\Local\Microsoft\OneDrive\
OneDrive.exe  11704
588:C:\Windows\PSEXESVC.exe         9984
```

