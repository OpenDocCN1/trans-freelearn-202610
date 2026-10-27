

现在 Microsoft Graph Security API 扩展已在 MineMeld 服务器上设置完成，我们可以配置 Microsoft Sentinel 连接器。

## 配置 Microsoft Sentinel 连接器

以下是配置 API 扩展的步骤：

- 1. 登录 MineMeld Web 门户。转到 CONFIG，然后选择页面底部的原型图标，即三条水平线（即汉堡菜单），如下图所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_124_0.png)

图 4.14 – MineMeld 配置页面

2. 如你所见，有很多选项可供选择。查看列表以了解可能想要实现的额外 TI 源，然后搜索 Microsoft 并查看结果。选择名为 microsoft_graph_secapi.output 的原型：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_125_0.png)

图 4.15 – Microsoft 原型

4. 在下一页，提供节点的名称，然后为 INPUTS 字段选择 inboundaggregator：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_127_0.png)

图 4.17 – 配置新节点

5. 通过查看节点列表确认新节点处于活动状态，如以下屏幕截图所示。选择页面顶部的 COMMIT 按钮。这将保存更改并重启流程以启用新功能：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_128_0.png)

图 4.18 – 确认新节点已添加

6. 现在转到 NODES 选项卡并选择新的 API 扩展，然后输入来自 Azure AD 中之前注册的应用程序的相关详细信息（见图 4.6）：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_129_0.png)

图 4.19 – 为 Microsoft Sentinel 配置 API 扩展

7. 对于 **CLIENT SECRET** 字段，你需要在 Azure AD 应用中生成一个新的密钥。转到之前创建的 Azure AD 应用并选择 **Certificates & secrets**，然后选择 **+ New client secret** 并按照出现的步骤创建新的密钥（确保复制详细信息，因为你只有一次机会；否则，你需要再次生成新的客户端密钥）：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_130_0.png)

图 4.20 – 创建新的客户端密钥

8. 将此新密钥添加到设置后，你应该会看到以下完成的屏幕：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_131_0.png)

图 4.21 – 完成配置

至此，MineMeld 服务的配置要求已完成。TI 源现在应发送到你的 Microsoft Sentinel 实例。

## 确认数据正在被 Microsoft Sentinel 接收使用

要确认数据正在发送到 Microsoft Sentinel，请按照以下步骤操作：

- 1. 转到 Azure 门户，然后 Microsoft Sentinel。
- 2. 选择 Logs。
- 3. 在命令窗口中输入以下命令并选择 Run：

```
ThreatIntelligenceIndicator
| take 100
```

4. 你应该会看到以下显示结果的屏幕：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_132_0.png)

图 4.22 – 检查 TI 源活动

5. 以下屏幕截图显示了其中一个事件的详细信息示例：

Completed. Showing partial results from the last 24 hours.

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_133_0.png)

图 4.23 – TI 数据源详细信息

你现在已将 MineMeld 服务器配置并连接到你的 Microsoft Sentinel 工作区，并且可以看到 TI 数据源出现在日志中。你现在可以使用此信息来帮助在 Microsoft Sentinel 中创建新的分析和狩猎查询、笔记本和工作簿。

我们建议定期审查以确保此信息既相关又经常更新。新的 TI 源会定期提供，你不想错过这些有用的信息，如果它可以帮助你找到新的和高级威胁。

## 总结

在本章中，我们探讨了 TI 的概念、新术语和解决方案选项，以及作为社区协作创建和共享 TI 源的概念。有多个选项可用于向 Microsoft Sentinel 添加 TI 源，我们知道 Microsoft 正在努力进一步开发此功能。TI 源将协助分析和检测不受欢迎的行为和潜在的恶意活动。选择范围很广，为组织选择合适的源是配置 Microsoft Sentinel 的重要部分。

下一章将介绍 Kusto 查询语言（KQL），这是一种强大的手段，可搜索为 Microsoft Sentinel 收集的所有数据，包括我们刚刚添加的 TI 数据。

## 问题

- 1. 命名三种威胁指标类型示例。
- 2. ATT&CK 框架的全称是什么？
- 3. 哪些 Microsoft Sentinel 组件可以利用 TI 源？
- 4. 谁开发了 STIX 语言和 TAXII 协议？

## 进一步阅读

以下资源可用于进一步探索本章涵盖的一些主题：

- - Malware Information Sharing Project: https://www.misp-project.org/
- - Mitre ATT&CK framework: https://attack.mitre.org/
- - Microsoft Security Graph API: https://github.com/microsoftgraph/security-api-solutions/tree/master/QuickStarts
- - STIX standard: https://stixproject.github.io/
- - TAXII protocol: https://www.mitre.org/sites/default/files/publications/taxii.pdf
- - Build a MineMeld server: https://live.paloaltonetworks.com/t5/general-articles/manually-install-minemeld-on-ubuntu-16-04/ta-p/253336
- - The Microsoft Graph Security API extension in MineMeld: https://github.com/PaloAltoNetworks/minemeld-msgraph-secapi

## 5 使用 Kusto 查询语言（KQL）

Kusto 查询语言（KQL）是一种纯文本、只读语言，用于查询存储在 Azure Log Analytics 工作区中的数据。与 SQL 类似，它使用从数据库开始，然后是表，最后是列的实体层次结构。在本章中，我们只关注表和列级别。

在本章中，你将学习可以使用来查询日志的众多 KQL 命令中的一些。

在本章中，你将学习以下内容：

- - 如何测试你的 KQL 查询
- - 如何查询表
- - 如何限制返回的行数
- - 如何限制返回的列数
- - 如何在多个表上执行查询
- - 如何以图形方式查看结果

本章涵盖以下主要主题：

- - 运行 KQL 查询
- - KQL 命令简介
- - 查询语句
- - 标量函数

## 运行 KQL 查询

在本章中，我们将使用 **Azure Data Explorer (ADX)** 中可用的示例数据。这是一个非常有用的工具，可以尝试简单的 KQL 命令。随意使用它来尝试本章中的各种命令。查询中使用的所有信息均来自 https://dataexplorer.azure.com/clusters/help/databases/Samples 提供的示例数据。

如果提示，请使用登录 Azure 门户时使用的登录凭据。首次登录时，你将看到以下屏幕。请注意，你的登录名可能会显示在页眉的右侧：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_136_0.png)

要运行本章的示例，你需要展开屏幕左侧的 Samples 日志，然后选择 StormEvents。你可以展开 StormEvents 以查看字段列表，如果这样做，你的屏幕应如下所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_137_0.png)

图 5.2 – StormEvents

要运行查询，请在页面顶部的查询窗口中输入或粘贴命令，即前一屏幕截图中编号为 1 的行右侧。输入查询后，单击 Run 按钮运行查询。输出将在页面底部的结果窗口中显示。

在以下屏幕截图中，输入并运行了一个简单查询。查询已输入查询窗口，你可以看到结果显示在其下方：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_138_0.png)

图 5.3 – 已执行查询

你可以用 ADX 做更多事情，所以随意尝试。现在你了解了如何运行查询和查看结果，让我们看看 KQL 提供的各种命令。

## KQL 命令简介

与 SQL 不同，查询从数据源开始，数据源可以是表或生成表的运算符，然后是转换数据的命令以满足需要。每个命令的输出可以通过使用管道（ | ）分隔符传递到下一个命令。

这是什么意思？如果你熟悉 SQL，你会编写类似 Select * from table 的语句来获取表中的值。相同的查询在 KQL 中只是 table，其中 table 指日志的名称。隐含的是你想要所有列和行。稍后，我们将讨论如何最小化返回的信息。

我们在这里只触及 KQL 功能的表面，但足以让你开始编写自己的查询，以便为 Microsoft Sentinel 开发查询。

