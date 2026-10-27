

一旦进入 **Edit** 模式或 **Dashboard Builder**，您可以添加、删除或修改现有组件，与创建新仪表板时一样。您可以通过单击 **Save** 按钮旁箭头下的 **Save As** 链接，将修改后的仪表板保存为新仪表板，或者您也可以使用 **Save As** 下的 **Delete** 链接删除仪表板，如以下屏幕截图所示：

![](img/72d01bea5a9ccf02f9452aab7505d9de_277_0.png)

好的，让我们来看一个动手练习，编辑之前创建的仪表板，通过添加一个新组件然后将仪表板保存为新仪表板：

1. 打开 **Account Dashboard**。
2. 单击 **Edit** 按钮。
3. 单击第一个组件（最左侧）并将大小更改为四列三行。
4. 相应地将第二个组件移动到第一个组件的右侧。
5. 通过单击 **+ Component** 按钮添加新组件。
6. 选择相同的 **Account by Owner** 报表。
7. 选择 **Vertical Bar Chart**。
8. 将 **Sort Rows By** 更改为 **Value Ascending**。
9. 添加 **Subtitle** 为 `All accounts` 和 **Footer** 为 `Date = all time`。
10. 单击 **Add** 按钮。
11. 修改组件大小为两列三行并将其移动到第二个组件的右侧。
12. 单击 **Save** 按钮旁箭头下的 **Save As** 链接；如果单击 **Done** 按钮，系统会询问是否要保存或丢弃更改。
13. 输入名称为 `Account Dashboard copy`；将文件夹保留为现有文件夹。
14. 单击 **Create** 按钮。参考以下屏幕截图：

![](img/72d01bea5a9ccf02f9452aab7505d9de_278_0.png)

15. 现在我们不再处于编辑模式或仪表板构建器中；我们处于查看模式。请注意，右上角有几个按钮和链接：

- **Refresh:** 单击此按钮刷新仪表板数据。
- **Edit:** 单击此按钮编辑仪表板。
- **Clone:** 单击此链接克隆为新仪表板而无需编辑，并保存为新仪表板。
- **New Dashboard:** 单击此项可在无需返回 **Dashboards** 选项卡的情况下创建新仪表板。
- **Delete:** 单击此项可删除现有仪表板；被删除的仪表板将被移至回收站（截至 Summer '17 版本，您需要切换到 Classic 才能访问回收站）。

接下来，让我们将 **Account Dashboard copy** 仪表板从 **Company Dashboards** 文件夹移动到名为 **Asia Sales** 的文件夹：

1. 编辑仪表板。
2. 单击齿轮图标 **Edit Dashboard Properties** 按钮。
3. 单击现有文件夹，选择新文件夹，然后单击 **Save** 按钮关闭属性窗口。这在以下屏幕截图中显示：

![](img/72d01bea5a9ccf02f9452aab7505d9de_279_0.png)

4. 再次单击 **Save** 按钮以保存仪表板的更改。
5. 现在仪表板应位于新文件夹中；您可以通过单击 Dashboards 选项卡进行双重检查，它应列在最近项目中。

## The dashboard components

如前所述，一个仪表板最多可包含 20 个组件，每个组件可以以图表、矩阵或表格的形式呈现。

构建仪表板主要有两个步骤：

1. 选择报表作为数据源。
2. 选择组件类型。

### Defining the dashboard components

在 Salesforce Lightning 中提供 11 种仪表板组件类型（截至 Summer '17 版本）：

- Horizontal Bar Chart
- Vertical Bar Chart
- Stacked Horizontal Bar Chart
- Stacked Vertical Bar Chart
- Line Chart
- Donut Chart
- Metric Chart
- Gauge Chart
- Funnel Chart
- Scatter Chart
- Table

以下屏幕截图中突出显示的区域显示了这些：

![](img/72d01bea5a9ccf02f9452aab7505d9de_281_0.png)

我们不会讨论所有组件类型，但让我们讨论其中几种。

### Tables

表格在以易于扫描的格式呈现信息方面很有用。图表可以让仪表板阅读者一目了然地获取信息，而表格则提供更多详细信息。将表格添加到您的仪表板以按摘要显示数据，如以下屏幕截图所示：

![](img/72d01bea5a9ccf02f9452aab7505d9de_282_0.png)

| STAGE | SUM OF AMOUNT |
|---|---:|
| Value Proposition | $330,000.00 |
| Qualification | $282,000.00 |
| Negotiation/Review | $236,000.00 |
| Id. Decision Makers | $170,000.00 |
| Proposal/Price Quote | $100,000.00 |

### Funnel charts

当您在有序集合中有多个分组并希望显示它们之间的比例时，使用漏斗图。漏斗图非常适合显示您的机会阶段。请考虑以下屏幕截图：

![](img/72d01bea5a9ccf02f9452aab7505d9de_283_0.png)

### Gauge charts

添加到 Lightning Experience 仪表板的仪表图默认显示百分比和总数。您可以手动设置每个区段范围的值，并且还可以更改图表颜色。请考虑以下屏幕截图：

![](img/72d01bea5a9ccf02f9452aab7505d9de_284_0.png)

### Stacked Bar charts

堆叠条形图非常适合比较条形之间的绝对值。要使用堆叠条形图，报表必须至少有两个分组，并且其中一个分组必须是数字。

您可以将堆叠条形图配置为 100%；这使您可以比较每个条形的相对值。例如，堆叠条形图可让您比较销售代表之间的机会比例。

以下屏幕截图显示了两个堆叠条形图；右侧的堆叠条形图配置为 Stack to 100%：

![](img/72d01bea5a9ccf02f9452aab7505d9de_285_0.png)

### Scatter charts

散点图可用于显示一个或两个报表数据组以及摘要。它们也适用于识别大型数据集中的趋势。要使用散点图，报表必须至少有两个汇总字段。散点图可以轻松显示机会的年龄与机会阶段。请考虑以下屏幕截图：

![](img/72d01bea5a9ccf02f9452aab7505d9de_286_0.png)

### Cumulative Line charts

与其使用普通折线图，您可以将折线图配置为累积折线图。您可以在累积报表上制作折线图，以突出显示每月已赢得的机会。请考虑以下屏幕截图：

![](img/72d01bea5a9ccf02f9452aab7505d9de_287_0.png)

### Combination charts

组合图在单个图表上绘制两个或多个数据集，使图表 *multimetric*。例如，条形图显示每个月的管道 **Amount**，而 **Line Chart** 显示每个月的管道数量，并在右侧添加第二个 y 轴以进行额外测量。这在以下屏幕截图中显示：

![](img/72d01bea5a9ccf02f9452aab7505d9de_288_0.png)

## Reports as a data source

Salesforce 为您提供灵活性，可使用任何已保存的报表作为组件的数据源。但是，使用保存在 **My Personal Custom Reports / Private Report** 文件夹中的报表的组件将不适用于其他用户，除非报表存储在查看用户的私有报表文件夹中，因为仪表板是作为私有报表文件夹所有者查看的。

您可以使用任何报表类型的报表作为数据源，但对于 **Tabular** 格式的报表，只有添加了 **Row Limit** 过滤器的 **Tabular** 报表才能用作数据源。请考虑以下屏幕截图：

![](img/72d01bea5a9ccf02f9452aab7505d9de_289_0.png)

本节的要点是不要假设报表和仪表板中图表的类型和图表功能是相同的。它们看起来相似，但并不完全相同。

## Summary

在本章中，我们使用报表，包括打开报表、搜索报表以及将报表添加到收藏夹。我们还导航报表并讨论了运行报表时可用的每个组件。

当您单击 **Reports** 选项卡时，您将看到最近打开的报表。您可以单击标题以排序查找报表。 **REPORTS** 下有一个菜单。在 **REPORTS** 下将是 **FOLDERS** 类别，您可以在其中检查您创建的文件夹、与您共享的文件夹以及您有权访问的所有文件夹。

操作按钮在 Reports 选项卡中为每个报表提供，并且在打开报表后也可用，具体取决于用户一般权限和与特定报表相关的用户权限。

您还了解了订阅报表；要将报表交付到电子邮件，您需要具有订阅报表的权限，并且最多可以订阅五个报表。

在下一章中，我们将开始使用仪表板，其中报表充当仪表板组件的数据源。
