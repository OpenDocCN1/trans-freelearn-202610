

收集运行 ASR 审计模式的信息并确定要应用哪些规则可能需要一些时间；不过，安全公司 Palantir 已根据两年 ASR 审计数据提出了建议。在此，他们展示了可以配置为默认的规则。您可以在 https://blog.palantir.com/microsoft-defender-attack-surface-reduction-recommendations-a5c7d41c3cf8 上阅读更多相关信息。

我们将首先使用 Endpoint Manager 为一台机器部署 ASR 规则并置于审计模式。当机器上启用了 ASR 规则的审计模式并且某条规则被触发时，机器会创建一条事件，我们可以对其进行跟踪。

与 ASR 相关的任何事件都会记录在 Windows 事件查看器中，可在 Applications and Services | Microsoft | Windows | Windows Defender | Operational 部分找到。

要在大规模查看这些事件，我们要么需要使用 Log Analytics 或 Windows 事件转发等服务将所有这些日志集中收集，要么可以使用 Microsoft Defender for Endpoint 进行查看。

首先，让我们使用 Endpoint Manager 创建一条 ASR 规则。要使用此方法，我们应已有一个已在 Endpoint Manager 中注册的现有设备，其次，机器应属于某个设备组。您可以在 https://docs.microsoft.com/en-us/mem/intune/enrollment/device-group-mapping 上阅读有关如何设置设备组的更多信息。

ASR 规则在 Endpoint Manager 门户中进行配置，可在 https://endpoint.microsoft.com/ 找到。然后，在 Endpoint security 下选择 Attack surface reduction，如以下屏幕截图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_120_0.png)

图 4.1 – Endpoint Manager 中的 ASR 规则

然后，单击 **Create Policy** 并选择 **Windows 10 and later** 作为平台。在 **Profile** 下，选择 **Attack surface reduction** 并单击 **Create Policy.**

首先，为 ASR 配置配置文件命名，我倾向于使用以下命名约定，以便轻松了解策略的目标和目标平台。如果您拥有一个拥有明确工作组（如 IT、HR 或工厂工人）的庞大环境，我也倾向于将此作为命名约定的一部分。

例如，您可以使用 win10_targetgroup_locationx_asr_audit。

然后，在 **Configuration Settings** 下，将所有规则设置为 **Audit mode**。然后，单击 **Next** 直到到达 **Assignments**。在那里，选择 **Groups** 并找到包含虚拟机的 Azure AD 设备组，然后单击 **Assign。**

一旦应用了 ASR 配置文件，在测试客户端同步新规则之前可能需要一些时间。您还可以使用此处列出的方法之一强制设备连接并下载最新的配置更新：https://docs.microsoft.com/en-us/mem/intune/user-help/sync-your-device-manually-windows。

一旦设备完全同步，我们需要测试 ASR 规则是否按预期工作。一种有效的方法是使用来自 Microsoft 的 ASR 测试工具，可从 https://demo.wd.microsoft.com/Page/ASR2 下载。

它提供内置测试场景，可直接从客户端运行，如以下屏幕截图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_121_0.png)

图 4.2 – Exploit Guard Demo Tool 配置

在此，如果您运行以下场景之一，例如 **Block creation of child process by Office application**，它应触发一个事件，该事件将在 **Microsoft-Windows-Windows Defender/Operational** 下生成，**EventID** 值为 **1122**，其中显示 **Microsoft Defender Exploit Guard audited an operation that is not allowed by your IT administrator**，如以下屏幕截图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_122_0.png)

图 4.3 – Exploit Guard 事件 ID

> **Note**
> 
> 例如，阻止 Office 应用程序创建子进程的规则是 2021 年出现的 MSHTML CVE 漏洞的缓解措施之一，您可以在 `https://www.microsoft.com/security/blog/2021/09/15/analyzing-attacks-that-exploit-the-mshtml-cve-2021-40444-vulnerability/` 上阅读更多相关信息。

最佳做法是尽可能配置尽可能多的这些规则，从 IT 部门和其他高级用户等小群体开始，通过收集审计日志并确定是否有任何 ASR 规则可能会阻止潜在应用程序。一旦验证完成，您应将 ASR 规则默认定义为阻止模式。

## Microsoft Defender 和反恶意软件

大多数终端也应安装并激活反恶意软件/防病毒解决方案。在本章中，我们将重点介绍如何使用 Microsoft Defender Antivirus，但也有其他不错的选择。我建议查看 AV-Comparatives 最新测试中审查的供应商当前列表，详细内容见 https://www.av-comparatives.org/tests/business-security-test-2021-august-november/，这可以表明现在最好的选择是什么。

就 Microsoft Defender Antivirus 的配置和管理而言，这也是通过组策略或使用 Microsoft Endpoint Manager 完成的，与我们配置 ASR 规则时所做的方式类似。

虽然我们可以配置很多设置，但我建议使用澳大利亚政府数字转型局详细说明的大部分蓝图，如 https://desktop.gov.au/blueprint/abac/intune-configuration.html#antivirus 中所述。

这里最重要的一点是，默认情况下，防病毒引擎应每 8 小时检查一次新的签名更新文件。仅供参考，Microsoft 每天可发布 10 多个签名更新。作为蓝图的一部分，它每小时检查一次新的签名更新文件。

此外，此蓝图将启用诸如 **Potentially Unwanted Applications (PUA)** 等功能，这是指由 Microsoft SmartScreen 评估为声誉不佳的应用程序，并阻止这些应用程序在设备上运行。

一旦为设备配置了策略，您可以使用此测试来验证在尝试从以下网站下载文件时 SmartScreen 是否正常工作：https://www.amtso.org/feature-settings-check-potentially-unwanted-applications/

如果启用了 PUA，在尝试从前述网站下载文件时，您应收到如下屏幕截图所示的消息：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_123_0.png)

*图 4.4 – Microsoft Edge 中 SmartScreen 阻止下载*

虽然许多这些安全功能可以阻止许多已知攻击和漏洞，但攻击者仍可能使用变通方法来绕过这些安全机制。

通常，勒索软件攻击者首先会尝试禁用终端上的防病毒保护，并使用 Mimikatz 等工具收集存储的凭据。他们还可能搜索并窃取其他类型的密码，例如保存在网络浏览器中的密码。Redline 恶意软件就是这种行为的示例，它专门针对保存在 Google Chrome 中的密码。

请注意，我们将在第 10 章《保护 Windows 免受勒索软件攻击的最佳实践》中更详细地介绍 Mimikatz。

此外，Microsoft 还有其他可帮助我们缓解这些攻击向量的功能，例如凭据转储以及限制应用程序和进程直接在底层操作系统上运行的能力。

这包括以下功能：

- **Microsoft Defender Application Guard** – 此功能允许我们在隔离的 Hyper-V 容器中打开 Microsoft Office 中的不受信任文件或 Microsoft Edge 中的不受信任站点。该容器与主机操作系统分离，这意味着容器内的进程无法直接影响操作系统。
- **Microsoft Defender Credential Guard** – 此功能确保 **Local Security Authority (LSA)** 也称为本地用户数据库，使用 **virtualization-based security (VBS)** 受到保护。这意味着 LSA 数据库无法被操作系统的其余部分直接访问。

应注意的是，当您启用凭据保护时，您不能使用旧协议，如 NTLMv1、MS-CHAPv2、Digest 或 CredSSP。

我们还在 Windows 中拥有其他有用的机制，用于限制允许在终端用户机器上运行的非批准可执行文件，例如 Microsoft Defender Application Control 或 Applocker。在此，我们可以根据应用程序的发布者定义策略，并定义应用程序和可执行文件的允许/阻止列表。两个功能都需要我们定义已批准的可执行文件和/或供应商的列表。此外，Microsoft 最近引入了名为 Smart App Control 的新功能，您可以将责任留给 Microsoft。

两个功能在底层虚拟机上都有一些硬件要求，如以下列表所述。还应指出的是，这些功能在任何虚拟化环境中均不受支持，例如基于 VDI 的工作负载：

- 支持 VBS
- 安全启动
- TPM（v1.2 或 2.0）
- 64 位 CPU
- Windows Hypervisor 和 CPU 虚拟化支持

两个功能也可以在 Endpoint Manager 中进行配置，转到 **Devices | Configuration Profiles | Create Profile**，选择 **Windows 10 and later | Templates**，然后选择 **Endpoint Protection。**

虽然 Credential Guard 只是一个简单的开关，只要满足要求即可启用，但 application guard 有稍多的配置选项。作为起点，我倾向于仅为 Microsoft Edge 启用该功能并启用日志记录，如以下屏幕截图所示：
