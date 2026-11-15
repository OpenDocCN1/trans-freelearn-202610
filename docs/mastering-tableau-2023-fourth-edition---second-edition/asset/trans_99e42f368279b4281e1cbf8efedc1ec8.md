
请注意，在 Tableau 文档中，缓存中的数据集通常被称为分区。本章的结构旨在提供一个简单的模式，以便理解表计算。

*函数介绍* 部分探讨每个独特的表计算函数。*函数应用* 部分探讨表计算如何通过分区和寻址维度应用到视图中。

# 定向和非定向寻址

我们将首先列出不同的表计算函数来开始本节。

Tableau 提供了广泛的表计算，但如果我们将考虑范围缩小到表计算函数的独特组别，我们会发现只有 11 个：

| 表计算 |
| --- |
| `LOOKUP` |
| `PREVIOUS VALUE` |
| `RUNNING` |
| `WINDOW` |
| `FIRST` |
| `INDEX` |
| `LAST` |
| `RANK` |
| `SIZE` |
| `TOTAL` |
| `SCRIPT & MODEL` |

正如*分区和寻址方向*中所述，表计算函数作用于缓存，因此根据表计算的类型（定向或非定向），如果缓存数据发生变化，结果可能会改变。在以下练习中，我们将看到定向和非定向表计算函数的实际运作。

让我们更仔细地看看定向和非定向表计算：

1.  导航至 `https://public.tableau.com/profile/marleen.meier` 以查找并下载与本章关联的工作簿。
2.  导航至 `Directional/Non-Directional` 工作表。
3.  创建计算字段，如下表所示：

| 名称 | 计算 | 备注 |
| --- | --- | --- |
| `Lookup` | `LOOKUP(SUM([Sales]),-1)` | 请注意此计算中包含的 `-1`。这指示 Tableau 从上一行检索值。 |
| `Size` | `SIZE()` | `SIZE` 函数返回分区中的行数。因此，正如下面的屏幕截图所示，大小等于总行数。 |
| `Window Sum` | `WINDOW_SUM(SUM([Sales]))` | `WINDOW_SUM` 函数（`WINDOW_SUM(expression, [start, end])`）可以定向或非定向地运行。由于此示例不包含 `[start, end]` 选项，因此它非定向运行。 |
| `Window Sum w/ Start&End` | `WINDOW_SUM(SUM([Sales]),0,1)` | 此窗口函数示例定向运行，这可以通过包含 `[start, end]` 选项来看出。 |
| `Running Sum` | `RUNNING_SUM(SUM([Sales]))` | 就其本质而言，`RUNNING` 函数定向运行，因为它们考虑前几行以进行计算。 |

1.  将 `Category` 和 `Ship Mode` 放置在 `Rows` 货架上。
2.  双击 `Sales`、`Lookup`、`Size`、`Window Sum`、`Window Sum w/ Start&End` 和 `Running Sum`，以填充视图，如下面的屏幕截图所示：

![](Images/B18435_05_01.png)

图 5.1：表计算

1.  *图 5.1* 中的表显示，`Size` 函数和 `Window Sum` 函数是仅有的两个非定向函数。所有其他函数根据方向显示数字。例如，`Lookup` 向下移动并打印实际字段之前的值。您可以通过单击任何表计算字段的三角形并选择 `Edit Table Calculation` 来更改方向。

此第一个练习是两个表计算类别的示例：定向和非定向。现在，我们将逐个考虑表计算函数。遗憾的是，篇幅不允许我们探索所有表计算；然而，为了获得实用的理解，考虑除一个之外的所有独特选项组别应该足够了。以 `Script_` 开头的四个表计算，以及六个 `Model_` 函数，将在*第 15 章*、*集成编程语言*中介绍。

虽然我在每节末尾添加了真实场景，但跟随练习的重点是演示表计算功能，并考虑每个功能如何**定向**或**非定向**地与缓存中的数据集（也称为**分区**）交互。

## 探索每个独特的表计算函数

以下练习将向我们单独展示每个表计算；在每节末尾，您将看到我对真实场景的思考。

### Lookup 和 Total

以下工作表的目标是显示在 2013 年最后两天进行购买的客户，以及 12 月相关的绝对销售额和相对销售额。

在下表中，`LOOKUP` 表现为定向，而 `TOTAL` 是非定向。这种行为在结果中很容易注意到。请按照以下步骤操作：

1.  在与本章关联的工作簿中，导航至 `Lookup/Total` 工作表。
2.  将 `Customer Name` 和 `Order Date` 拖至 `Rows` 货架。通过右键单击并选择 `Day` 以及 `Discrete`，将 `Order Date` 设置为 `Month/Day/Year` 离散型。
3.  将 `Order Date` 放置在 `Filters` 货架上，选择仅查看 2013 年 12 月，并选择 `Month/Year` 作为日期格式。
4.  创建以下计算字段：

| 名称 | 计算 |
| --- | --- |
| `Lookup Intervening Time` | `DATEDIFF('day', LOOKUP(MAX([Order Date]), 0), MAKEDATE(2013,12,31))` |
| `% Total Sales` | `SUM([Sales])/TOTAL(SUM([Sales]))` |

1.  右键单击 `% Total Sales`，选择 `Default Properties` | `Number format`，将数字格式设置为百分比，保留两位小数。
2.  将 `Lookup Intervening Time` 放置在 `Filters` 货架上，并选择 0 到 1 的范围。
3.  双击 `Sales` 和 `% Total Sales`，将这两个字段放置在视图上。
4.  根据需要进行格式化：

![](Images/B18435_05_02.png)

图 5.2：总销售额

让我们考虑一下前面的工作表是如何运作的：

*   `Order Date` 上的筛选器确保返回给 Tableau 的数据集仅包含 2013 年 12 月的数据。
*   `% Total Sales` `SUM([Sales])/TOTAL(SUM([Sales]))` 包含 `TOTAL(SUM([Sales]))` 计算字段，它返回整个数据集的总销售额。用 `SUM([Sales])` 除以此总计可返回总计的百分比。
*   `Lookup Intervening Time`：`DATEDIFF('day', LOOKUP(MAX([Order Date]), 0), MAKEDATE(2013,12,31))` 将返回一个整数，反映 `LOOKUP` 函数返回的日期与 `12/31/2013` 之间的差异。请注意，`LOOKUP` 函数的偏移量为零。这导致每行返回与该行关联的日期。这与包含偏移量为 -1 的 `LOOKUP` 函数的定向和非定向表计算不同，后者导致视图中的每行返回与前一行关联的数据。

乍一看，您可能会认为可以通过从 `Filters` 货架中移除 `Lookup Intervening Time` 并调整 `[Order Date]` 上的筛选器以仅显示 12 月的最后两天来简化此工作簿。然而，如果您这样做，`% Total Sales` 将在视图的所有行中加起来等于 100%，这将无法满足工作簿的目标。将 `Lookup Intervening Time` 视为不进行筛选，而是隐藏 12 月除最后两天之外的所有日期。这种隐藏确保了计算 `% Total Sales` 所需的数据位于缓存/分区的数据集中。

真实场景：我使用 `Lookup` 的频率比 `Total` 高。两者在使用 Tableau 时都非常宝贵。我们已经看到，您可以使用 lookup 进行变化对比；您可以考虑月度利润对比、今天与去年同期的员工人数对比、每周每天的网站点击量差异等。
