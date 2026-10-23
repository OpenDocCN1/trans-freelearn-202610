

- **DKIM** – 使用 DKIM，会添加一个签名，可以确认电子邮件是以发送域所有者授权的方式发送的，并且电子邮件的相关部分未被更改。采用非对称加密在电子邮件头中作为单独的头部字段添加数字签名。用于签名的公钥随后会与发送组织的 DNS 记录一起发布。
- **DMARC** – 这基于 SPF 和/或 DKIM，并告诉我们接收方应如何处理未通过这些检查的电子邮件。此外，该标准还提供了一种报告协议，可为收件方和发件方邮件组织提供更高程度的失败邮件报告。

验证所发布的各种 DNS 记录的一个很好的工具是 MxToolbox.com，它允许您在 SPF、DKIM 和 DMARC 记录发布后，只需使用搜索菜单即可轻松扫描域。例如，您可以输入 `spf:Microsoft.com` 来获取 Microsoft 的 SPF 记录。

虽然我在这里不会过多详细介绍这些机制，但 Cisco 的这份演示文稿更详细地介绍了优缺点以及功能的工作方式：`https://www.ciscolive.com/c/dam/r/ciscolive/apjc/docs/2019/pdf/BRKSEC-1243.pdf`。

关于 Office 365，大多数这些功能都很容易实现，您可以在 `https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/use-dmarc-to-validate-email?view=o365-worldwide` 阅读更多关于这些功能实现的信息。

您可以设置 DMARC，以便从接收您域电子邮件的邮件服务器接收定期报告。我建议您定期监控收到的 DMARC 报告。然而，这需要您拥有 DMARC 报告服务。Microsoft 没有将自己的 DMARC 报告服务作为 Office 365 的一部分，但它已与 Valimail 签署了合作伙伴关系，Office 365 客户如果注册 `https://use.valimail.com/ms-dmarc-monitor.html` 即可获得免费的 DMARC 报告服务。

## 保护内容和 URL

除了在 Office 365 中实施 SPF、DMARC 和 DKIM 外，Microsoft 在 Exchange Online Protection 中还有额外的安全机制来保护用户免受钓鱼攻击。

这些功能例如如下：

- 安全附件（适用于 Exchange 和 SharePoint 与 OneDrive）
- 安全链接
- 反钓鱼保护
- 反欺骗保护

此外，Microsoft 最近推出了一项名为 **零小时自动清除 (ZAP)** 的新功能，可让您追溯检测并中和已交付给 Exchange Online 用户的有害钓鱼、垃圾邮件或恶意软件消息。您可以在 https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/zero-hour-auto-purge?view=o365-worldwide 阅读更多关于 ZAP 功能的信息。

让我们先来看看 Safe Attachments 和 Safe Links，它们适用于拥有 Office 365 E5 许可证的组织。Safe Attachments 服务使用虚拟环境在电子邮件消息交付给最终用户之前检查附件（使用爆破检测恶意内容）。该功能默认对所有最终用户启用，如果发现恶意内容，消息会自动放入隔离区。

您还拥有适用于 SharePoint、OneDrive 和 Teams 的相同功能，可以检查上传文件中是否存在恶意内容，但该功能默认未启用。该功能可使用 Microsoft PowerShell 或 Microsoft Defender 门户中的 Safe Attachments 选项启用。

如果您使用 Microsoft PowerShell，请执行以下步骤：

- 1. 使用 Exchange Online PowerShell 模块连接到 Office 365，如此处所述：https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps。
- 2. 然后，运行此 PowerShell 命令以启用 Safe Attachments：
- 3. 用户无法打开、复制或共享由 Safe Attachments 检测到的恶意文件；然而，他们仍可下载包含恶意内容的文件。您可以通过输入以下命令来禁用此功能：

```
Set-AtpPolicyForO365 -EnableATPForSPTeamsODB $true
```

```
Set-SPOTenant -DisallowInfectedFileDownload $true
```

这可确保用户无法下载可能包含恶意软件或其他恶意内容的文件。

另一个应该使用的功能是，在我们拥有正确的许可证的情况下，使用 Microsoft Defender for Office。您可以在本书的 GitHub 仓库中找到有关如何实施它的更多详细信息，您可以在此处找到：https://github.com/PacktPublishing/Windows-Ransomware-Detection-and-Protection/tree/main/chapter4。

## 其他对策

除了前面列出的资源外，在减少钓鱼攻击风险或用户访问恶意内容方面，还有一些其他额外提示：

- 默认情况下，Microsoft Teams 允许开放式联合，这允许任何组织向另一组织中的另一用户发送直接即时消息。最近，我遇到了一起案例，其中大量用户收到来自另一组织中某人的消息，冒充 IT 部门的用户。

您可以在 `https://msandbu.org/phishing-attacks-in-microsoft-teams-and-external-federation/` 阅读更多关于该攻击及其对策的信息。

- 作为 SharePoint 管理员，您还可以定义同步策略来定义用户允许（或不允许）同步的附件文件类型。阻止文件扩展名为 `.exe`、`.hta`、`.js` 或 `.iso` 的文件是一个好的开始。您可以在 `https://docs.microsoft.com/en-us/onedrive/block-file-types` 阅读更多关于如何设置此功能的信息。

- 需要注意的是，诸如安全链接和安全附件等功能为电子邮件、Teams 和 SharePoint 提供了安全机制。它们仍可被绕过，使用合法的 URL，例如来自 Microsoft 的其他 OneDrive URL 或其他基于 Azure 的服务，如此处所述：`https://thalpius.com/2022/03/28/microsoft-defender-for-office-365-safe-links-bypass/`。

因此，重要的是要注意，虽然提到的这些安全功能会提高您的安全性，但它们并非万无一失。

## 总结

在本章中，我们更详细地研究了使用 Endpoint Manager 的端点对策，其中包括 ASR 规则以及 Defender 和反恶意软件规则的管理。此外，我们还学习了如何确保我们的端点保持最新，并且我们的浏览器定期更新。然后，我们深入了解了如何使用 MFA 服务（如 Conditional Access 和使用 Azure MFA 服务的 NPS）来保护我们的服务。最后，我们研究了如何使用 SPF、DKIM、DMARC 以及 Office 365 中的其他机制（如 Safe Attachments）来保护我们的电子邮件。

作为保护免受勒索软件攻击的整体安全策略的一部分，实施保护最终用户和设备的安全机制将最大程度地降低勒索软件的风险。然而，我们仍然需要对环境有一个整体的看法。因此，实施虚拟基础设施的安全机制也很重要。在下一章中，我们将更深入地研究如何在 Microsoft Azure 中为运行的工作负载实施对策。

## 勒索软件对策
– Microsoft Azure 工作负载

许多组织正在将虚拟机工作负载迁移到云端。随之而来的是，他们需要很好地了解不同的安全机制以及在放置基础设施之前架构应是什么样的。

不幸的是，我见过许多客户在将工作负载迁移到公共云后因缺乏知识或安全机制有限而遭受勒索软件攻击或信息泄露的案例。

本章将介绍如何在 Microsoft Azure 中为虚拟机工作负载构建安全架构的理论，以及以下主题：

- Microsoft Azure 中的网络分段和设计最佳实践
- 使用 DDoS 和 WAF 机制保护外部服务的机制
- Azure 中身份和访问管理的身份安全最佳实践
- 使用 Azure Arc 保护混合工作负载
- Microsoft Azure 中的身份和访问管理
- 使用 Azure Backup 和 Azure Policy 进行数据保护

## 技术要求

在本章中，我们将重点介绍 Microsoft Azure 中使用的架构和服务。为了能够使用本章中列出的功能和服务，您需要一个 Microsoft Azure 的活跃订阅。如果您没有活跃订阅，您可以在 https://azure.microsoft.com/en-us/free/ 注册一个免费的 200 美元试用版。还需要注意的是，许多服务都有运行成本，因此如果您设置测试环境，请在完成后删除资源，以免产生任何经常性费用。

