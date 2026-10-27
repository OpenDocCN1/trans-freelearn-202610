

## Summary

在本章中，你学习了如何创建一个可以在分析查询规则中使用的工作簿，以执行 SOAR 操作。Playbooks 基于 Azure Logic Apps构建，唯一的区别是，只有使用 Microsoft Sentinel 连接器，Logic App 才能成为 playbook。

通过你所学到的知识，你现在可以创建 playbooks 来自动化许多以前必须手动执行的操作。你阅读了其中一个示例，但你可以做的事情没有限制！

在下一章中，我们将使用本章学到的知识构建一个 playbook，该 playbook 将创建一个新的 ServiceNow 工单并使用工单号更新事件。

## Questions

回答以下问题以测试你对本章的了解：

- 1. 在 logic app 能够用作 Microsoft Sentinel playbook 之前需要做什么？
- 2. 如何判断某个特定的 playbook 上次运行是否成功？
- 3. 在 playbook 的工作流中，我如何获取有关事件的信息？
- 4. 什么是动态内容？
- 5. 我可以在一个字段中组合动态内容和静态内容吗？

## Further reading

请参考以下链接以获取有关本章所涵盖主题的更多信息：

- Logic Apps overview: https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview
- Connectors for Azure Logic Apps: https://docs.microsoft.com/en-us/azure/connectors/apis-list
- Sample Azure logic apps: https://docs.microsoft.com/en-us/samples/browse/?products=azure-logic-apps
- Sample Microsoft Sentinel playbooks: https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks
- Testing logic apps with mock data but setting up static results: https://docs.microsoft.com/en-us/azure/logic-apps/test-logic-apps-mock-data-static-results

让我们来看下一个方法。

## Integrating Azure security alert sources (not just Sentinel) with ServiceNow Security Incident Response via the Microsoft Graph Security API

自本书第一版发布以来，Microsoft 和 ServiceNow 已合作通过 Microsoft Graph Security API 实现 Microsoft Sentinel 和 ServiceNow Security Incident Response 集成。

Microsoft Graph Security API 是“一个中间服务（或代理），它提供单一的编程接口来连接多个 Microsoft Graph Security 提供商（也称为安全提供商或提供商）。对 Microsoft Graph Security API 的请求会被分发到所有适用的安全提供商。结果会被聚合并以通用模式返回给请求的应用。” – https://docs.microsoft.com/en-us/graph/security-concept-overview

使用 Microsoft Graph Security API 将 Microsoft Sentinel 与 ServiceNow 集成的一个关键优势是：它不需要将 Azure 中的所有安全警报都流经 Sentinel。它甚至不需要你使用 Sentinel。这些是较少见的用例。大多数组织都会使用 Sentinel，并使用 Sentinel 聚合和关联其所有 Azure 安全警报。

使用 Microsoft Graph Security API 将 Microsoft Sentinel 与 ServiceNow 集成存在一些限制，包括以下内容：

- Microsoft Graph Security API 不支持将数据从 ServiceNow 发送回 Sentinel。例如，当 ServiceNow 中的安全事件关闭时，组织通常希望 Sentinel 中对应的警报自动关闭。
- ServiceNow 为 Microsoft Graph Security API 提供的集成插件不支持在将多个 Sentinel 警报聚合到同一个 ServiceNow 安全事件时使用复杂条件。例如，基于两个字段的聚合将要求两个字段都匹配；没有一个字段或另一个字段匹配的选项。

> Note
> 有办法绕过这个问题，但需要将创建的集成配置文件数量加倍，增加很多复杂性。但可以做到。

## Figure 13.5 – Add a client secret pane

输入密钥的描述，并使用 Expires 下拉菜单确定你希望此密钥存在多长时间。你需要在密钥存在的时间长度与你需要进入、创建新密钥并使用此新信息更新插件的频率之间取得平衡。

5. 点击 **Add** 按钮添加新密钥。完成后，窗格将关闭，你将被带回 **Certificates & secrets** 页面，如下所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_420_0.png)

Figure 13.6 – New secret created

从此页面，你需要复制密钥的 **Value** 字段。这就是我们需要提供给插件的 **client secret key**。再次提醒，请将此文本保密并考虑将其存储在安全保管库中并以编程方式检索和轮换密钥。将它视为用户的密码一样对待。

我们现在已经获得了提供给插件所需的六个信息中的三个：Client ID、Tenant ID 和 client secret。要获取 Subscription ID、Resource Group 名称和 Workspace 名称，我们需要进入 Microsoft Sentinel。

## Installing the Microsoft Sentinel integration plugin in ServiceNow

此插件可在 ServiceNow 商店中使用。确保阅读发行说明以确认你满足所有要求，例如 ServiceNow 实例版本。

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_422_0.png)

Figure 13.8 – ServiceNow Sentinel plugin

建议在开发时使用插件的最新版本，并在生产环境中始终使用开发时所使用的相同版本。

## Configuring the ServiceNow Sentinel plugin to authenticate to Microsoft Sentinel

这是你将输入在 Configuring the Microsoft Azure portal 部分收集的变量的步骤。请记住，不要向第三方服务提供商提供你的 client secret。相反，使用屏幕共享和桌面控制解决方案，在他们进行到此步骤时自行输入。

此步骤的一个陷阱是，点击提交后会有轻微延迟，并且不会告诉你是否成功。它会告诉你是否有错误。如果窗口在没有错误的情况下关闭，则表示你已成功将 ServiceNow 实例认证到 Microsoft Sentinel 实例。如果出现错误，通常是由 Configuring the Microsoft Azure portal 部分遗漏步骤或表单数据中的拼写错误引起的（如果复制/粘贴，请注意前导或尾随空格）。请耐心等待——大约需要 20 秒来判断是否成功：

- **Retrieve recent Microsoft Sentinel incidents**：这将从 Sentinel 导入最近的示例事件。由于你之前更改了此设置，你应该获得 20 个。这取决于 Sentinel 实例是否有可用的事件：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_427_0.png)

Figure 13.12 – Mapping form

将下拉菜单设置为 **Retrieve recent Microsoft Sentinel incidents** 后，点击 **Import Sample Data** 按钮。这需要几分钟时间，正如表单上消息所指示的那样。我们看到它可能不到一分钟，但也可能需要几分钟，达到你可能想去喝杯咖啡或散步然后再回来查看的程度。一旦示例记录出现，你可以点击查看它们是由哪些产品生成的。我们在这里使用术语 *catching samples*，因为你正在尝试捕获产品类型的示例记录，以便你可以为该产品类型创建特定的配置文件。从样本中首先出现的产品开始。然后，继续处理示例记录，直到无法捕获任何新产品为止。可能需要多次导入尝试和特定的时间来捕获不常见产品的示例事件。

