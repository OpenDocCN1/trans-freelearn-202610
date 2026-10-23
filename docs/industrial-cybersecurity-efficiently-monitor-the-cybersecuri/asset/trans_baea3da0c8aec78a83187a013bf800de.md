

## 已配置的用户

尽管 msinfo32.exe 应用程序抓取了大量信息，但缺少一个关键细节：系统用户账户。为了将此信息添加到我们的捆绑包中，我们可以使用随当今每个版本的 Windows 一起提供的原生 net.exe 工具。

## Net user

我们想要获取的第一种用户信息是本地账户。这可以通过在 Windows 命令提示符中发出 net user 命令来完成：

```
C:\Documents and Settings\Administrator>net user

User accounts for \\HMI-1

----------------------------------------
---------
Administrator          Guest          HelpAssistant
SUPPORT_388945a0
The command completed successfully.
```

该工具的输出显示了所有本地配置的用户账户。输出随时间的变化应被视为高度可疑。

## Net localgroup administrators

net user 命令仅显示在计算机上创建的本地账户。我们想查看是否创建了任何域账户或特殊账户并将其添加到系统的特权组中。我们使用 net localgroup <groupname> 命令来做到这一点。以下示例显示了属于 Administrators 组的用户账户（Windows 客户端系统上最特权的组）：

```
C:\Documents and Settings\Administrator>net localgroup administrators
Alias name          administrators
Comment             Administrators have complete and unrestricted access to the computer/domain

Members

----------------------------------------
---------
```

Administrator

QT-DOMAIN\Domain Admins

The command completed successfully.

此外，我们对 **power users** 组感兴趣：

C:\Documents and Settings\Administrator>net localgroup "power users"

Alias name  power users

Comment  Power Users possess most administrative powers with some restrictions. Thus, Power Users can run legacy applications in addition to certified applications

Members

----------------------------------------------------------------------------- -------

The command completed successfully.

最后，如果我们想查看是否创建了不应该存在的任何组，我们可以不指定组使用 net localgroup 命令：

C:\Documents and Settings\Administrator>net localgroup

Aliases for \\HMI-1

----------------------------------------------------------------------------

*Administrators

*Attackers

*Backup Operators

*Guests

*HelpServicesGroup

*Network Configuration Operators

*Power Users

*Remote Desktop Users

*Replicator

*Users

The command completed successfully.

这些命令的输出可以轻松复制并粘贴到已导出的系统信息报告中，以完成信息包。

至此，我们已经结束了对工业环境的积极探索。通过使用与端点交互或在网络上传送数据包的主动工具，我们发现了大量信息，这些信息可以帮助我们建立包含服务、应用程序和操作系统版本等详细信息的资产清单，包括软件和固件修订版。我们还拥有用户账户和其他端点特定细节的清单。我们应该以一种方便的方式存储所有这些信息，使我们能够查询工业环境安全态势的特定问题。

## 总结

本章我们涵盖了大量内容。我们讨论了各种主动安全监控方法，并亲手实践了一些常见工具和扫描技术。我们研究了发现漏洞的方法，以及如何在 ICS 网络上扫描已知错误配置和其他问题。

在上一章和本章讨论的技术和工具之间，我们捕获了大量与安全相关信息。在下一章中，我们将讨论存储、检索、搜索、关联和呈现所有这些信息的方法。

## 8
## 工业威胁情报

现在我们的安全监控工具已经安装，我们将开始寻找扩展其功能和增强其能力的方法。虽然在下一章 *第 9 章，可视化、关联和告警* 中，我们将研究增加可见性、更详细的信息以及总体上更好的威胁检测功能的方法，但在本章中，我们将讨论允许我们向安全监控程序添加 **威胁情报** 的工具、技术和活动。威胁情报通过将发现的安全监控工件（例如 URL、IP地址和文件哈希）与已知威胁信息（入侵指标）相关联，为我们的安全监控解决方案增加了存在置信度、被入侵可能性和利用可行性，从而使 resulting 信息对我们的环境相关且可操作。

本章将涵盖以下主题：

+   * 威胁情报解析
+   * 在工业环境中使用威胁信息
+   * 获取威胁信息
+   * 使用威胁信息创建威胁情报
+   * 向我们的 Security Onion 部署添加威胁源

## 技术要求

本章中使用以下工具和资源：

+   * **AlienVault OTX**：`https://otx.alienvault.com`

## 威胁情报解析

威胁情报是知识，是与您的组织和特定环境相关的威胁的知识。威胁情报是系统化流程的结果，其中来自各种数据源的基于证据的分析导致可操作的数据，有助于发现或根除最可能在您的网络和环境中遇到的威胁。威胁情报为针对您的组织、工业部门或运营空间的对手以及他们执行其恶意 **战术、技术和程序 (TTPs)** 的方式提供深入洞察。这种知识使我们能够就如何保护我们的环境做出更有效和精确的决策。

许多所谓的威胁情报网站会试图向您出售他们所称的威胁情报。按定义，这是不可行的。除非一家公司非常熟悉您的环境并且能够为您执行将威胁信息转化为可操作情报数据的流程，否则他们能提供的最好只是威胁信息。在这种意义上，威胁信息仅仅是一份 **入侵指标 (IOCs)** 列表。如前所述，IOCs 是表征威胁的技术信息片段。它们可以是 **互联网协议 (IP)** 地址、域名、文件名和位置、注册表键、文件哈希等。IOCs 使威胁检测、威胁狩猎以及将环境中发现的工件与恶意行为者、行为、内容或活动的已建立和已验证工件进行关联成为可能。

例如，假设我们在环境中识别出一个系统正在与 `http[:]//148.251.82.21/Microsoft/Update/KS4567890[].php` 通信。这个 URL 似乎有些可疑，不是吗？好吧，如果我们在 AlienVault 的威胁交换网站上对域名或 IP 地址进行快速查找（`https://otx.alienvault.com/indicator/ip/148.251.82.21`），我们可以看到我们的怀疑是正确的：

![](img/c8ffa9959721097701584fcc95101362_319_0.png)

通过一次简单的查找，我们刚刚发现这个可疑的 IP 地址与 **BlackEnergy** 恶意软件活动有关。

我们刚刚执行的操作被认为是事件响应或网络取证。该系统已经与可疑服务器通信，并且可能已经被入侵。我们使用威胁信息来支持我们的结论并形成一个明智的决定，即该网络流量是恶意的，但仍然，我们只是将网络流量工件与 IOC 列表进行了比较。

