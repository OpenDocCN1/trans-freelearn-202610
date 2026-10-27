

| 字段名称 | 描述 |
|---|---|
| 重新打开已关闭的匹配事件 | 如果您希望在匹配到新的警报时重新打开之前已关闭的事件，请启用此选项。如果您认为与之前已关闭事件匹配的额外警报值得对之前已关闭的事件进行进一步调查，则可能需要这样做。 |

表 7.4

6. 当您填写完所有字段后，点击 Next: Automated response > 继续。如果您需要在任何时候更改上一屏幕上的某些值，请点击 Previous 按钮。

Automated response 页面允许您选择在生成警报时自动运行哪些 playbook 和/或自动化，如下所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_227_0.png)

图 7.19 – 创建新规则，自动化响应

在前面的屏幕截图中，有多个可供选择的 playbook。如果没有列出任何 Playbooks，则表示您尚未创建任何以 When a response to an Microsoft Sentinel alert is triggered (Preview) 作为触发器类型的 playbook。有关创建 Microsoft Sentinel Playbooks 的更多信息，请参阅第 12 章，创建 Playbooks 和自动化。

7. 当您填写完所有字段后，点击 **Next: Review** > 继续。如果您需要更改上一屏幕上的某些值，请点击 **Previous** 按钮。

**Review and create** 屏幕将向您展示您所有选择的回顾并验证您的条目以确保其有效。如果存在问题，您将看到如下所示的屏幕：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_229_0.png)

图 7.21 – 创建新规则，Review and create 页面出现错误

下表提供了每个条目的进一步详细信息：

| 名称 | 描述 |
|---|---|
| Microsoft security service | 使用下拉列表选择要从中摄取警报的 Azure 资源。这包括 Microsoft Cloud App Security、Azure Security Center、Azure Advanced Threat Protection 等。 |
| 按严重性筛选 | 这可以是 Any 或，如果您选择 Custom，则可以从 High、Medium、Low 或 Informational 中选择一个或多个。这将筛选这些警报，仅允许所选严重性通过。 |
| 包含特定警报 | 如果选中，则只有您选择的那些警报会触发此规则。在弹出窗口中打开的文本框中输入特定的警报规则名称。可以选择多个特定规则。 |
| 排除特定警报 | 如果选中，则所有警报都会触发此规则，除了您选择的那些。在弹出窗口中打开的文本框中输入特定的警报规则名称。可以选择多个特定规则。 |

表 7.6

3. 当您填写完所有字段后，点击 Next: Automated response > 按钮继续。Automated response 屏幕将打开，如以下屏幕截图所示，您可以在其中查看和添加新的事件自动化规则。

您可以看到一条错误消息，提示存在验证失败，并且在出现错误的页面名称旁边有一个红点。在本例中，是 General 页面，如选项卡名称旁边的红点所示。如果没有错误，您将看到如下所示的页面：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_235_0.png)

图 7.27 – 创建 Microsoft 事件规则，Review and create 页面无错误

6. 最后，点击 Create 按钮创建您的规则。

我们已经介绍了创建新分析规则的三种不同方法：使用模板、从头创建新的计划规则，以及从头创建新的 Microsoft 事件规则。这三种方法各有用途，在为环境设置 Azure 时您很可能会全部使用。接下来，我们将了解如何管理现有的分析规则。

## 管理分析规则

规则创建后，您需要持续管理它们以确保它们保持有用。您可能需要调整规则以获得更好的结果，更改分配给计划规则的 Playbooks，禁用规则，甚至删除不再需要的规则。

您只能管理 Active rules 选项卡中列出的规则。因此，请按照以下两个简单步骤操作：

- 1. 首先，点击 Active rules 链接。
- 2. 在规则列表中，在 LAST MODIFIED 列的右侧是上下文菜单，以及每个规则的一行三个句点。点击它，您将看到如下所示的下拉列表：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_236_0.png)

图 7.28 – 分析规则上下文相关菜单

让我们看看如何使用这些选项：

- - Edit: 此条目将允许您编辑规则，以便根据需要修改任何字段。您也可以通过点击详细信息刀片上的 Edit 按钮来编辑规则。编辑规则将带您通过与创建规则相同的页面，所有保存的参数都已填入。进行必要的更改并保存它们。
- - Disable: 此条目将允许您禁用规则。如果规则被禁用，则此条目将标记为 Enable，并且将允许您启用规则。如果您认为规则不再需要而在删除之前，或如果您将执行已知会不必要地触发规则的操作，则可以禁用规则。只需记住完成后重新启用它！

## 问题

- 1. 五种不同的规则类型是什么？
- 2. 如果我希望规则按设定的间隔运行，应该使用哪种规则类型？
- 3. 您能否让来自其他 Azure 安全系统的警报在 Microsoft Sentinel 中创建事件？
- 4. 要使 Playbook 在计划警报触发时自动运行，您需要做什么？
- 5. 删除不再需要的规则的两种方法是什么？

## 进一步阅读

您可以参考以下链接获取更多详细信息：

- - *Creating custom analytic rules to detect suspicious threats:*
  https://docs.microsoft.com/en-us/azure/sentinel/tutorial-detect-threats-custom
- - *Microsoft Sentinel correlation rules: Active List out; make_list() in, the AAD/AWS correlation example:*
  https://techcommunity.microsoft.com/t5/azure-sentinel/azure-sentinel-correlation-rules-active-lists-out-make-list-in/ba-p/1029225
- - *Microsoft's Microsoft Sentinel Query Style Guide:*
  https://github.com/Azure/Azure-Sentinel/wiki/Query-Style-Guide#severity
- - *Microsoft Sentinel entity types reference:*
  https://docs.microsoft.com/en-us/azure/sentinel/entities-reference

# 8

# 创建和使用工作簿

Microsoft Sentinel 工作簿是一种创建和显示可自定义和交互式报告的方式，可以显示图表、图表和表格。信息可以使用您已经熟悉的相同的 **Kusto Query Language (KQL)** 查询从 Log Analytics 工作区呈现。这些工作簿基于已与 Azure Monitor 和 Log Analytics 工作区等其他 Azure 资源一起使用的 workbook 技术。

Microsoft Sentinel 提供了多个可直接使用的模板。您可以使用这些模板创建自己的工作簿，然后根据需要进行修改。大多数用于摄取数据的连接器都附带自己的工作簿，以便您更好地了解使用表格和可视化效果（包括条形图和饼图）摄取的数据。您也可以根据需要从头创建自己的工作簿。

在本章中，您将学习以下主题：

- - Workbooks 页面概述
- - 浏览现有工作簿
- - 创建工作簿
- - 编辑工作簿

- - 管理工作簿
- - 工作簿步骤类型

## Workbooks 页面概述

要转到 Workbooks 页面，请从 Microsoft Sentinel 导航刀片中选择 Workbooks。会出现一个新屏幕，其外观如下所示的屏幕截图：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_240_0.png)

图 8.1 – Microsoft Sentinel Workbooks 页面

前述屏幕截图中页面顶部的标题显示了 Refresh 和 Add workbook 按钮。添加新工作簿将在 添加新工作簿从头开始 部分中讨论。

