

## 使用入侵指标来发现额外的可疑系统

我们现在将使用迄今为止发现的入侵指标，看看是否能在我们的网络上发现其他可能存在入侵迹象的系统。根据 IOC 的类型，我们将使用适当的工具在工业环境中进行搜索。

## 迄今为止发现的 IOC

为了回顾迄今为止发现的所有入侵指标，请考虑以下列表：

- **IOC-1 – IP 地址：** 222.222.222.222
- **IOC-2 – URL：** very-malicious-website.com
- **IOC-3 – URI：** /serve.html
- **IOC-4 – 用户代理：** <host name> <OS version>
- **IOC-5 – 服务名称：** IPRIP
- **IOC-6 – 恶意软件安装命令：** rundll32.exe <dll-name>Install(A)

此外，我们还可以将 IPRIPa.dll 文件的位置添加到列表中，并从我们提取的 dll 文件（或 HMI-2 上 c:\windows\temp 文件夹中的文件）创建 MD5 和 SHA256 哈希值以添加到列表中：

```
remnux@remnux:~/workdir$ md5sum module.1040.22deb88.10000000.dll

ebcf387056a5ae4e1c62f5b595b65250  module.1040.22deb88.10000000.dll

remnux@remnux:~/workdir$ sha256sum module.1040.22deb88.10000000.dll

4614e2d216493b6f3da26700032e33113c8cbfc3f9b34ff77c3b8671d01f1c18  module.1040.22deb88.10000000.dll
```

这使得我们发现的 IOC 总数达到八个：

- **IOC-7 – 恶意软件文件位置：** c:\windows\temp\IPRIPa.dll
- **IOC-8 – 恶意软件文件哈希：** ebc f387056a5ae4e1c62f5b595b65250; 4614e2d216493b6f3da26700032e33113c8cbfc3f9b34ff77c3b8671d01f1c18

有了这个列表，让我们来看看环境中是否还有其他入侵。

## 搜索网络特定的入侵指标

我们可以使用 Security Onion Kibana 门户来搜索 IP 地址、URL 以及其他网络（数据包）入侵指标。因此，让我们打开 Kibana 门户并从第一个 IOC 开始，即我们发现为恶意软件发送的信标的目标 IP 地址 222.222.222.222。以下步骤展示了处理过程：

1. 我们在 **Security Onion - Home** 仪表板上的搜索框中输入 IP 地址，以查看与该 IP 地址关联的日志类型：

![](img/c8ffa9959721097701584fcc95101362_513_0.png)

图 11.62 – Kibana – 222.222.222.222 – 概览

2. 此视图显示 Kibana 在多种日志（**数据集和模块**）中找到了对 222.222.222.222 IP 地址的引用。有 OT-IDS（SilentDefense）和 Suricata 日志、防火墙（pfSense）日志、连接日志、文件、HTTP 详情、DNS 记录等都与此 IP 地址相关联。让我们从 Suricata 日志开始，这些日志显示在 **Alert** 仪表板上（**Home – Alerts**）：

![](img/c8ffa9959721097701584fcc95101362_514_0.png)

图 11.63 – Kibana – 222.222.222.222 – Suricata

3. 请注意，我们必须将 IP 地址重新添加到搜索中（AND 222.222.222.222）。Suricata 警告潜在的恶意流量（嗯，显而易见！），但仅将警报与单个内部 IP 地址 172.25.100.220（HMI-2 计算机）关联：

![](img/c8ffa9959721097701584fcc95101362_514_1.png)

图 11.64 – Kibana – 222.222.222.222 – Snort 警报

顺便说一下，Suricata 在那里发现了一个有趣的警报。

6. 我们发现了一台潜在的额外受感染机器 – `172.25.100.203` – 但让我们继续寻找更多。作为快速说明，当我们过滤 `pass` 时，即仅显示防火墙允许连接的事件，我们会看到只有 HMI-2 的 IP 地址显示出来：

![](img/c8ffa9959721097701584fcc95101362_516_0.png)

这是 `172.25.100.203` 未在 Snort 日志中显示的原因：与 C&C 服务器的连接从未被允许，从而阻止了信标的成功（阻止了与 Web 服务器的连接建立）。如果我猜测原因的话，是因为防火墙规则允许 HMI-2（允许任何规则）到互联网的任何连接，而对我们发现的新 IP 则更为严格。

1. 切回 **Home** 仪表板，浏览 **Home** 仪表板上显示的其他日志仅确认了 `172.25.100.203` 的内部 IP 与 `222.222.222.222` 的关联。

2. 当我们切换到 DNS 仪表板（**Home - Network - DNS**）并搜索 URL 而不是 IP 地址时，我们发现信标的目标是 `very-malicious-website.com`；我们可以看到与 HTTP 以及 DNS 日志的关联。然而，只有 DNS 日志显示了额外的 IP 地址：

![](img/c8ffa9959721097701584fcc95101362_517_0.png)

图 11.68 – Kibana – DNS 发现

这支持了 172.25.100.203 从未成功建立与 C&C 服务器连接的理论。如果它建立了连接，Zeek 将能够获取连接详情并将其归档到 HTTP 日志下。

3. 由于我们已在本书前文检查过环境的 user-agent 字符串，最后要执行的是搜索信标过程检索的文件 serve.html。从 Home 仪表板对该 IOC 进行搜索仅显示与 HTTP 日志的关联（警报与 HTTP 流量相关）：

![](img/c8ffa9959721097701584fcc95101362_517_1.png)

图 11.69 – Kibana – serve.html 发现

4. 查看 **HTTP** 仪表板仅显示 HMI-2 IP 地址，这并不令人惊讶，考虑到之前的发现。因此，从网络特定 IOC 的角度来看，我们发现的唯一额外可疑主机是 172.25.100.203，当我们搜索时，我们可以将其识别为 HMI-1（SMB 仪表板）：

![](img/c8ffa9959721097701584fcc95101362_518_0.png)

图 11.70 – Kibana – 172.25.100.203 的 SMB 详情

让我们从不同角度搜索，查看主机本身存在的工件，看看是否能找到其他可疑机器。

### 搜索基于主机的入侵指标

当在环境中搜索基于主机的入侵指标时，拥有资产管理数据库会很方便。诸如 **Forescout** 或 **SolarWinds** 之类的工具允许你从门户搜索许多 IOC。这可以为你节省大量时间和精力。然而，我想展示另一种在环境中查找基于主机 IOC 的方法，使用 grep。

如果你还记得本书前文，我们使用 msinfo32.exe 工具为工业网络中的 Windows 机器生成系统报告。我们将这些报告以文本格式保存。我们可以使用 grep 实用程序在这些报告文件中进行搜索。grep 是一个 Unix 命令，用于在文件中搜索与指定模式匹配的字符字符串：

1. 为了练习，我们将 **MSinfo32** 报告复制到我们的 REMnux VM，以便从那里使用 grep 命令对其进行操作：

![](img/c8ffa9959721097701584fcc95101362_518_1.png)

图 11.71 – MSinfo32 报告

2. 现在，如果我们想搜索我们发现的进程名称 IOC，例如，我们可以通过以下命令 grep 它：

```
remnux@remnux:~/workdir/MSinfo32$ grep IPRIP *
HMI-1_2020_Sept-3.txt:Intranet Network Awareness (INA+)
IPRIP Running    Auto Share Process    c:\windows\
system32\svchost.exe -k netsvcs Normal    LocalSystem
0
HMI-2_2020_Sept-3.txt:Intranet Network Awareness (INA+)
IPRIP Running    Auto Share Process    c:\windows\
system32\svchost.exe -k netsvcs Normal    LocalSystem
0
```

报告文件的搜索显示共有两台系统存在 IPRIP 服务。输出甚至显示服务正在运行。我们可以根据需要打开报告查找具体信息。

3. 以同样的方式，我们可以搜索 dll 的文件位置（c:\windows\temp），看看是否能带来任何额外信息：

```
remnux@remnux:~/workdir/MSinfo32$ grep 'c:\\windows\\temp' *
HMI-2_2020_Sept-3.txt:ipripa    Not Available    23.50 KB
(24,065 bytes)    9/7/2020 4:54 PM Not Available    c:\windows\temp\ipripa.dll
```

4. 有趣的是，搜索仅返回一个命中，即 HMI-2 计算机。也许 dll 存储的位置在其他系统上不同。让我们改搜索 dll 名称：

```
remnux@remnux:~/workdir/MSinfo32$ grep -i "ipripa.dll" *
HMI-1_2020_Sept-3.txt:ipripa    Not Available    23.50 KB
(24,065 bytes)    9/11/2020 12:12 PM    Not Available c:\windows\system32\ipripa.dll
HMI-2_2020_Sept-3.txt:ipripa    Not Available    23.50 KB
(24,065 bytes)    9/7/2020 4:54 PM Not Available    c:\windows\temp\ipripa.dll
Ind-DC-2_2020_Sept-3.txt:ipntt    Not Available    23.50 KB
(24,065 bytes)    6/3/2020 1:51 PM Not Available    c:\windows\system\ipripa.dll
```

