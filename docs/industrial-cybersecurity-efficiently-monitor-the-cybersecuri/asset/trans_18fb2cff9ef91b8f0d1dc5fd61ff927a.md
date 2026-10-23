

## 命令 vol.py -f HMI-2.vmem --profile=WinXpSP2x86 dlllist -p 1040 | grep 0x00

命令 `vol.py -f HMI-2.vmem --profile=WinXpSP2x86 dlllist -p 1040 | grep 0x00` 使用 Volatility 的 `dlllist` 插件来显示从 HMI-2 内存转储中 PID 为 1040 的进程 `-p 1040` 加载的所有 DLL。我们随后使用 grep（Linux 工具，用于按特定模式过滤输出）来搜索输出中 DLL 加载位置的内存位置前四位数字，并将其与发现 **恶意** 字符串出现的位置的内存位置进行比较。例如，`grep 0x01` 只会显示 Volatility 命令的输出中包含字符串 `0x01` 的部分。这将向我们展示在恶意字符串 `0x011cee57d` 内存位置附近加载的 DLL。

只有一个位置，字符串的地址落在 `svc host.exe` 进程加载的 DLL 的地址空间内。相关 DLL 为 **ipripa.dll**，其加载地址为 `0x10000000`，大小为 `0x13000`：

```text
0x10000000  0x13000  0x1
C:\Windows\temp\ipripa.dll
```

恶意字符串出现位置的内存位置完全落在 DLL 的地址空间内：

| Rule: r1 | |
| :--- | :--- |
| Owner: Process svc host.exe Pid 1040 | |
| **0x1000602d** | 6d616c6963696f75732d776562736974 | malicious-websit |
| **0x1000603d** | 652e636f6d000000000000000000000000000 | e.com... |

此外，DLL 的加载位置也非常可疑：`c:\windows\temp\ipripa.dll`。通常，DLL 不会从临时文件夹加载；这总是可疑的！

那么，让我们获取 DLL 的副本以便进一步分析。这可以通过 Volatility `dlldump` 插件实现：

```text
remux@remux:~/workdir$ vol.py -f HMI-2.vmem --profile=WinXpSP2x86 dlldump -p 1040 -b 0x10000000 --dump-dir ./

Volatility Foundation Volatility Framework 2.6.1
Process(V) Name Module Base Module Name Result
----  -------------  ----------------  ----------
--  ----
0x820deb88 svc host.exe  0x01000000 ipripa.dll OK: module.1040.22deb88.10000000.dll
```

有了它，我们就有了可疑的可执行代码的副本供我们进一步探索。作为快速检查以验证我们是否提取了正确的内存部分，让我们看看恶意字符串是否出现在 DLL 中：

```text
remnux@remnux:~/workdir$ strings module.1040.22deb88.10000000.dll | grep malicious -b
1445:very-malicious-website.com
```

这是一个很好的迹象，表明我们获得了正确的 DLL。`strings` 命令显示 `very-malicious-website.com` URL 在偏移量 1445 处出现（位置由 `grep` 命令的 `-b` 标志添加 – 字节偏移）。为了了解我们在这里处理的是什么，让我们接下来更仔细地看看这个可执行文件在做什么。让我们进行下一步的恶意软件分析。

## 上传可执行文件到 VirusTotal

我们将通过将 DLL 发送到 VirusTotal 来开始恶意软件分析，以查看可执行文件是否有任何已知的检测结果。打开网络浏览器，导航到 https://www.virustotal.com/gui/ 并上传 DLL 进行分析：

![](img/c8ffa9959721097701584fcc95101362_474_0.png)

图 11.28 – 提取的 DLL 的 VirusTotal 结果

我认为这是一个相当好的迹象，表明 DLL 是恶意的。我们现在已经有了入侵者的几个名称和分类，可以利用这些信息找到具体细节并发现任何额外的 IOC，以帮助我们找到其他可能受感染的机器。相反，我想带你进入恶意软件分析的其余过程，以展示如何自行发现任何（额外的）入侵指标。

## 对可疑可执行文件进行初步检查 – 恶意软件分析 101

请记住，以下步骤和程序仅用于说明恶意软件分析的整体过程，绝不意味着用作详细的恶意软件分析教程。本书无意深入到如此程度，但对学习恶意软件逆向工程这一迷人学科感兴趣的人应购买 Michael Sikorski 所著的《Practical Malware Analysis: The Hands-On Guide to Dissecting Malicious Software》一书。

从根本上说，恶意软件分析有两种方法或阶段：静态分析和动态分析。前者让我们在不运行恶意软件的情况下查看它，只是为了看看是否能找到有关恶意软件行为的线索或提示。后者是一种更危险的方法，让我们运行恶意软件并查看恶意软件运行的环境，以查看恶意软件所做的更改或留下的痕迹。让我们从静态分析之旅开始。我们将使用 REMnux VM 进行恶意软件分析之旅。

### 静态分析

通过静态分析，我们旨在发现有关恶意软件的细节、信息和具体内容，从而洞察其预期用途。有许多方法可以从恶意软件中获取这些数据；我们将查看三种常用的技术：转储恶意软件的字符串、查看其导入和导出表，以及对恶意软件可执行文件（字节或二进制）代码进行反汇编以显示其程序流程。

## Strings

可执行文件通常使用文本片段（字符串）来执行其功能。无论是按钮的名称还是 **图形用户界面 (GUI)** 的标题，甚至是正在检索的 URL 或文件位置，所有这些字符串都会为我们提供有关可执行文件可以做什么的线索。大多数 Linux 发行版都配备了一个用于从文件中提取字符串的工具，恰好命名为 `strings`。让我们在用 Volatility 提取的 DLL 上运行 `strings` 工具：

```text
remnux@remnux:~/workdir$ strings module.1040.22deb88.10000000.dll

!This program cannot be run in DOS mode.
...
Sleep
RegisterServiceCtrlHandlerA
RegSetValueExA
RegCreateKeyA
CloseServiceHandle
CreateServiceA
OpenSCManagerA
RegCloseKey
RegQueryValueExA
RegOpenKeyExA
DeleteService
OpenServiceA
WS2_32.dll
InternetReadFile
HttpQueryInfoA
HttpSendRequestA
HttpOpenRequestA
InternetConnectA
InternetOpenA
WININET.dll
very-malicious-website.com
serve.html
Windows XP 6.11
CreateProcessA
Depends IN+\ Collects and stores network configuration and
location information, and notifies applications when this information changes.
OpenSCManager()
RegQueryValueEx(Svchost\netsvcs)
netsvcs
RegOpenKeyEx(%s) KEY_QUERY_VALUE success.
RegOpenKeyEx(%s) KEY_QUERY_VALUE error.
SOFTWARE\Microsoft\Windows NT\CurrentVersion\Svchost
IPRIP
...
```

前面的输出被删减为仅显示有趣的字符串，并为简洁起见而剪裁。我们可以看到引用 Windows API 调用的字符串，这些调用指示服务控制（创建、删除、启动和停止）：

- CreateServiceA
- OpenSCManagerA
- DeleteService
- OpenServiceA

有指示 Windows 注册表操作的字符串：

- RegSetValueExA
- RegCreateKeyA
- RegCloseKey
- RegQueryValueExA
- RegOpenKeyExA

我们还可以看到与互联网交互的迹象：

- WS2_32.dll
- InternetReadFile
- HttpQueryInfoA
- HttpSendRequestA
- HttpOpenRequestA
- InternetConnectA
- InternetOpenA

我们将再次使用 `pedump` 实用程序，这次使用 `-E` 标志列出导出：

```text
remnux@remnux:~/workdir$ pedump -E module.1040.22deb88.10000000.dll

=== EXPORTS ===
# module "ipripa.dll"
# flags=0x0  ts="2010-09-28 01:00:25"  version=0.0  ord_base=1
# nFuncs=5  nNames=5

  ORD ENTRY VA   NAME
      1     4706 Install
      2     3196 ServiceMain
      3     4b18 UninstallService
      4     4b0b installA
      5     4c2b uninstallA
```

导出列表比此 DLL 的导入列表小得多。这里有一些有趣的条目。首先，我们在内存中运行时观察到的模块名称 `ipripa.dll`。我们还可以看到模块的编译日期和版本号。然后实际的导出条目显示序数号（函数可以通过名称或序数号调用）、模块中函数代码开始的偏移量（VA 或虚拟地址）以及名称。我们将在下一节中使用导出详细信息，当我们使用 IDA Pro 反汇编器——任何认真逆向工程师或恶意软件分析师的首选工具——仔细查看代码时。

## 使用 IDA Pro 反汇编恶意软件代码

在前面的部分中，我们查看了从 HMI-2 中提取的恶意软件 DLL 的一些静态工件。现在我们将把事情提升一个档次，使用反汇编器尝试检索恶意软件的部分原始代码。

以下是对计算机程序如何从文本文件转换为可执行文件的非常高层次的解释。它并非旨在对该主题进行彻底讨论，只是为了指出理解本节最重要的部分。

