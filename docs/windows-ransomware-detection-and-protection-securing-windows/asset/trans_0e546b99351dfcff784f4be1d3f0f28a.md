

LockBit 除其他 **勒索软件即服务 (Ransomware as a Service, RaaS)** 运营商外，使用了一个众所周知的俄语网站论坛 XSS 来宣传其附属计划。然后，XSS 运营商在其网站上禁止了所有勒索软件主题，LockBit 开始使用自己的基础设施来宣传其附属计划。

LockBit 已知会招募内部人员通过其附属计划获得基础设施访问权限，用 *数百万美元* 换取访问有价值公司数据的权限：

## [勒索软件] LockBit 2.0 是一个附属计划。

附属计划 LockBit 2.0 暂时重新开放合作伙伴招募。

该计划自 2019 年 9 月起运行，原生使用 C 和 ASM 语言设计且无任何依赖。加密部分通过完成端口（I/O）实现，加密算法为 AES + ECC。在两年时间内无人能够解密它。

无与伦比的优势是加密速度和自传播功能。

你唯一需要做的是获得对核心服务器的访问权限，其余所有工作 LockBit 2.0 都会完成。启动会在域网络的所有设备上实现，前提是域控制器具有管理员权限。

简要功能集：
- Tor 系统中的管理员面板；
- 通过 Tor 与公司通信，带 PUSH 通知的聊天室；
- 自动测试解密；
- 自动解密器检测；
- 本地子网中的端口扫描器，可检测所有 DFS、SMB、WebDav 共享；
- 在运行时无需脚本即可在域网络中自动分发；
- 终止干扰服务和进程；
- 阻止可能破坏加密过程的进程启动；
- 设置文件权限并移除阻止属性；
- 删除卷影副本；
- 创建隐藏分区，拖放文件和文件夹；
- 清除日志和自我清除；
- 窗口化或隐藏运行模式；
- 通过 Wake-on-Lan 启动已关闭的计算机；
- 在网络打印机上打印勒索要求；
- 适用于所有版本的 Windows 操作系统；

图 1.5 – 显示 LockBit 招募程序的屏幕截图

LockBit 在其网站上宣传，其数据加密方法比其他勒索软件变种快得多，并且他们对加密方面的编程感到非常自豪。

此外，其勒索软件（像大多数其他勒索软件变种一样）在俄语国家和系统语言设置为俄语的基础设施中不运行。在某些情况下，内置检测机制会在系统运行俄语时通知运营商或停止信息收集过程。

他们使用的操作模式与我们谈到的其他组织类似；然而，在过去一年中他们也发展了很多。2021 年 10 月，也有传言称他们开发了首个 LockBit Linux-ESXi 变种。

ESXi 勒索软件并非新事物，但此新变种同时针对 vCenter 和 VMware ESXi，并利用漏洞来获得对 VMware 环境的访问权限。

## 最新增添

现在，在 2023 年，我们看到出现了包含来自旧组织附属或成员的新威胁组织。

我们有以下组织：
- Royal
- RansomHouse
- BlackCat
- ClopLeaks

还有数十个之多。在社交媒体上，我们每天可以看到新的受害者被发布。以下是可以用来跟踪这些不同威胁组织的来源：
- https://twitter.com/TMRansomMonitor
- https://twitter.com/RansomwareNews

由于我们看到新受害者受影响的频率很高，使用这些来源来了解当前趋势并了解哪些组织最活跃非常重要。

## 整体概览

现在我们已经查看了一些主要攻击向量以及更详细的一些勒索软件变种，我想描绘一个更大的图景并提供一些重要的考虑因素。

让我们从勒索软件攻击的第一阶段开始，即初始入侵：
- 在大多数情况下，钓鱼攻击被用来诱使用户点击恶意附件以运行特定有效载荷，从而在受感染端点上触发恶意软件，例如 BazarLoader。
- 其他攻击从利用易受攻击的端点开始，例如 Exchange、RDP 或其他可用的第三方服务。我们看到在附属方获得对组织的访问权限后，该访问权限会以 5,000 美元到 50,000 美元不等的价格出售给威胁行为者，具体取决于访问类型。

一旦攻击者成功获得访问权限，第二阶段就开始了，即收集信息：
- 获得端点访问权限后的初始阶段是评估环境，使用内置脚本和工具获取有关机器/网络/用户/数据的信息。如果他们以后想将访问权限出售，这些信息也用于收集他们已获得访问权限的组织类型的证明。

下表总结了一些勒索软件运营商用于评估环境并尝试进一步访问环境的常用工具和脚本。

需要指出的是，这不是一个完整列表；我只是列出了我在不同客户场景中遇到的一些工具。然而，它能更好地了解黑客用于收集信息的工具：

| ADFind | Atera | Invoke- SMB AutoBrute | Advanced IP Scanner |
| SharpView | BloodHound | Net-GPPPassword | MSSQLUDP Scanner |
| Net Use | DCSync | SharpChrome | Zero.exe |
| NetScan | Router Scan | BITSAdmin | Spashtop Remote |
| Esentutl | Mimikatz | Invoke-ShareFinder | SWLCMD |
| WMI C | Cobalt Strike | PowerView | UAC-TokenMagic |
| Nltest | WDigest | Process Hacker | Kerberoast |
| AnyDesk/TeamViewer | Getuin | FileZilla SFTP | Seatbelt |

图 1.6 – 常用工具和脚本概览表

除了前述表中提到的一些脚本/工具外，攻击者还使用许多内置功能在环境中导航。这些功能可以是 RDP 和文件资源管理器等。一些运营商也已知使用组策略管理在同一时间对多台机器执行操作。

截至撰写本文时，大多数勒索软件针对的是基于 Windows 的环境，因为大多数企业在数据中心的大部分区域都在运行 Windows。这包括 Active Directory、文件服务器和 SQL 服务器，以及 Windows 端点。然而，我们也看到勒索软件运营商转向新的目标类型。还有新的勒索软件变种出现，针对其他服务，例如 NAS 服务。其中一个新变种称为 Deadbolt，针对的是 QNAP NAS 设备。也有针对 Linux 和 Mac OS X 的变种，因此这是我们都应关注的问题。

## 身份攻击

现在我们已经查看了不同的攻击向量以及一些勒索软件变种及其攻击模式，我想更深入地探讨一些常见的攻击向量，从身份攻击开始。

随着向 Microsoft 365 等公共云服务的迁移，身份攻击变得越来越常见。

SaaS 服务有一个共同属性，即它们可从互联网访问，这意味着 *任何人* 都可以访问这些服务。

如前所述，常见的攻击向量之一是凭证填充，攻击者尝试使用从泄露中获取的用户名和/或电子邮件地址列表登录。

以下屏幕截图显示了我们其中一个租户的登录尝试，其中每天从多个位置看到大量登录尝试是典型的。

此屏幕截图取自我们的 Azure AD 登录日志，并使用 Log Analytics 解析（我将在 *第 7 章《使用 Azure 信息保护和数据保护保护信息》中介绍*）：

|  |  |  |  |
|---|---|---|---|
| > | 2/3/2022, 11:13:10.434 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:14:14.654 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:15:06.893 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:15:10.940 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:15:53.489 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:15:03.212 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:15:49.949 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:16:13.716 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:16:18.022 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |
| > | 2/3/2022, 11:16:06.503 AM | Sign-in was blocked because it came from an IP address with malicious activity | RU |

图 1.7 – Office 365 阻止的身份验证尝试概览

现在，由于这是 Azure AD，Microsoft 已内置不同 IP 过滤器来阻止来自已知恶意 IP 地址的登录尝试，这意味着它们在尝试身份验证前就被阻止了。然而，这只是显示了在短时间内有多少身份验证流量进入。

