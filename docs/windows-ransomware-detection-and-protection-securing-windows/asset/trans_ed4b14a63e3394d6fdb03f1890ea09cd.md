

> **注意**
> 
> 本书官方 GitHub 仓库中提供了更多使用 Kusto 检测入侵的示例，可在此处找到：`https://github.com/PacktPublishing/Windows-Ransomware-Detection-and-Protection`。

第一个攻击场景可能涉及钓鱼攻击，而第二个场景可能涉及针对配置错误服务的暴力破解攻击。

在第三种场景中，可公开访问的服务可能用于合法目的，例如 VPN、远程工作和 Web 应用程序。然而，仍有可能利用零日漏洞或现有漏洞来获取对您基础设施的访问权限。定期审查和评估这些服务以确保其安全非常重要。

了解可公开访问的外部服务也很重要，例如用于网站的服务。例如，正在使用的服务和框架的映射可能包括以下内容：

- VPN（Fortinet）
- VDI（VMware Horizon）
- Web 应用程序（IIS 和 Apache），主要使用 .NET 应用程序
- 运行旧版 Java 的 Web 应用程序，用于 ERP 系统
- 电子邮件（Microsoft Exchange）

虽然可能很难确定是否使用了漏洞利用，但首先要确定每个服务的软件版本类型，并确保它们已更新到最新版本。如果没有，请确保它们运行的是最新版本，如果您怀疑其中一个外部服务存在初始入侵，请尝试查找有关该漏洞的更多信息以及缓解方法。

例如，CIS 维护着一个正在被利用的已知漏洞的最新列表，网址为 `https://www.cisa.gov/known-exploited-vulnerabilities-catalog`，在您绘制服务地图后也可以将其用作来源。

## 摘要

在本章中，我们了解了更多关于如何处理潜在攻击、如何将攻击划分为不同阶段、我们应如何在组织内部和向当局通报，以及一些寻找入侵技术证据的方法。

需要注意的是，本章提到的技术只是常用的方法中的几个例子。有效 IT 安全的关键在于拥有在需要时提供洞察的正确工具，以及及时了解最新的漏洞和攻击方法。威胁形势不断变化，因此跟踪当前威胁至关重要。这将在下一章中进一步讨论。

## 监控威胁形势

威胁形势不断演变，因此 IT 领域的所有人员都必须拥有工具和新闻来源，以便轻松及时了解当前威胁。

在本章中，我想提供一些建议，说明您如何使用不同的在线来源和流程来监控未来威胁的威胁形势，以帮助您保持最新状态。

本章还提供了我们过去一年所看到的趋势的见解，以及保护工作负载免受未来威胁将重要的因素。

在本章中，我们将涵盖以下主题：

- 如何监控威胁形势
- 建立流程以确保威胁得到处理
- 未来会怎样？

## 如何监控威胁形势

作为一名 IT 专业人员，我花费大量时间收集和处理信息，因此拥有工具来轻松聚合合适的信息和新闻水平非常重要。当然，收集信息的方式有很多，这里我只分享我个人收集信息的一些不同方式。

我使用的主要工具之一是 Feedly，这是一项用于使用 RSS 聚合来自不同来源的 feed 的服务。Feedly 使用简单，因此您可以轻松添加新的网站作为数据源，并且个人账户免费使用。

您可以使用社交账户在 feedly.com 免费注册 Feedly。注册后，您必须添加要关注的来源，可以通过添加单个页面或添加一堆不同来源来添加。您可以通过单击左侧的 **Following website** 按钮并将网站 URL 添加到搜索面板，然后按 *Enter* 来添加单个来源。然后您将看到您搜索的网站，如下图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_240_0.png)

图 9.1 – 使用 Feedly 通过 RSS 收集网站更新

然后，您单击 **FOLLOW** 按钮将其添加为新闻来源。除了添加单个站点外，您还可以通过访问以下地址添加捆绑包：https://feedly.com/i/discover/sources/search/topic/cybersecurity。

在这里，您有一个可以关注的不同来源列表，以便轻松获取与安全新闻和更新相关信息。

此外，Feedly 还具有名为 Leo 的 AI 引擎，它还可以帮助减少重复信息的杂乱，对于威胁情报信息收集也非常有用。有了 Leo，您可以使用不同的预定义集合来从大量开放源收集信息，如下图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_241_0.png)

图 9.2 – Feedly 和 Leo 网络警报配置

因此，例如，我可以监控具有高漏洞评分的来源，并在此处直接获取聚合源。替代方案是直接访问每个网站并将每个来源添加到 Feedly 的主 RSS 引擎，通过其他来源通过电子邮件获取信息，甚至手动访问每个站点获取信息。

您还可以定义不同的过滤器，例如 AND/OR，这使我们能够收集与我们基础设施中产品相关的漏洞数据，无论是 VMware、Veeam、Citrix 还是 Microsoft。

我们还可以在不访问网站的情况下将相关信息和文章直接转发到 Teams 或 Slack。需要注意的是，Leo 的功能以及将事件转发到 Teams/Slack 的能力仅在 Feedly 的企业版中可用，每位用户每月费用约为 12 美元。

此外，我个人也大量使用 Twitter 来收集威胁信息。与 Feedly 相比，Twitter 的缺点是作为社交媒体平台会有更多噪音。然而，我使用它不仅仅是为了获取威胁信息；它也有助于获取研究、其他相关新闻或故障排除问题的信息。

在 Twitter 上，您还可以创建包含预定义人员或频道的列表以供关注。但是，这需要您过滤大量噪音。更好的选择是使用不同的搜索运算符来收集可能有用的信息：

| 搜索运算符 | 查找的推文 |
|---|---|
| CVE Microsoft | 同时包含 CVE 和 Microsoft。这是默认运算符。 |
| "Office Zeroday" | 包含确切短语 – Office Zeroday。 |
| ransomware OR cve | 包含 ransomware 或 cve（或两者）。 |
| ransomware -cve | 包含 ransomware 但不包含 cve。 |
| #Zerologon | 包含 Zerologon 标签。 |

表 9.1 – 显示 Twitter 的不同搜索运算符

需要注意的是，许多信息安全社区的人也转向了 Mastodon，这是一个 Twitter 的替代品，例如 Infosec Mastodon 网络：https://infosec.exchange/。

此外，您还有其他更官方的来源可用于收集与新漏洞相关信息，例如以下内容：

- Microsoft Security Response Center：https://msrc-blog.microsoft.com/
- Cisco Talos Intelligence：https://talosintelligence.com/vulnerability_info
- VMware Security Response Center：https://www.vmware.com/security/vsrc.html

您还有更多厂商中立的网站也积极监控新漏洞，例如网络安全和基础设施安全局（CISA），它还监控已知被利用的漏洞：https://www.cisa.gov/known-exploited-vulnerabilities-catalog。

微软最近基于其在 2021 年收购的公司 RiskIQ 引入了几项新服务。此次收购后发布的产品之一是 Microsoft Defender Threat Intelligence，如果您拥有 Microsoft 365 许可证，可以在此处访问：https://ti.defender.microsoft.com/。

该服务提供不同的 SKU。您有免费选项和付费选项，后者按用户每年收费。虽然付费版本提供有关漏洞和指标的更丰富信息，但免费版本也可用于查找漏洞以及与 IP 地址相关信息。

例如，如果我输入 PrintNightmare 的 CVE ID，我可以获得有关该漏洞的详细信息，如下图所示：

