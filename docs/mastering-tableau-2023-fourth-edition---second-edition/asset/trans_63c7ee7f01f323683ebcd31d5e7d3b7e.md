

## 排名

让我们来了解一下这是如何工作的。我们不需要编写 `Rank(Sum(Sales))` 函数，只需在任意货架上的 `Sales` 实例上右键单击，然后选择 `Quick Table Calculation`（快速表计算）| `Rank`（排名），相同的代码就会自动生成。请注意，`RANK()` 正在对每个区域内的产品名称进行计数。这表明 `RANK()` 表计算在缓存/分区中的数据集上按方向运行。

实际场景：当我想要筛选一个持续变化的视图，且该视图被划分为多个子部分时，我会使用 `RANK()`——例如，我每篇博客文章的最后三条评论。或者当我想要展示随时间变化的行为时——例如，两年内排名前 10 位的产品。这将生成一张凸峰图。

## 大小

以下工作表的目标是在 `Superstore` 数据集中显示城市数量在五个或五个以下的所有州。此示例将演示 `SIZE()` 如何利用缓存中的整个分区/数据集，因此它是非方向性的。我们还将使用 `FIRST()` 表计算函数（它是方向性的）来清理视图。

请遵循以下步骤：

1.  在与本章关联的工作簿中，导航到 `Size` 工作表。
2.  将 `Analysis`（分析）| `Stack Marks`（堆叠标记）设置为 `Off`（关闭）。
3.  创建以下计算字段：

| 名称 | 计算 |
| --- | --- |
| `Size` | `SIZE()` |
| `City Count` | `IF FIRST() = 0 THEN [Size] ELSE NULL END` |

4.  将 `State` 拖至 `Rows`（行）货架，`City` 拖至 `Detail`（详细信息）货架，`City Count` 拖至 `Text`/`Label`（文本/标签）货架，`Size` 拖至 `Detail` 货架。
5.  右键单击 `Size` 筛选器，选择 `Compute Using`（使用计算方式）| `City`。将 `Size` 从 `Marks`（标记）卡移动到 `Filters`（筛选器）货架。
6.  在生成的对话框中，选择 `At most`（至多）值为 `5`：

![](Images/B18435_05_12.png)

图 5.12：至多

7.  在 `Marks` 卡上，右键单击 `City Count` 并选择 `Edit Table Calculation`（编辑表计算）。在 `Nested Calculations`（嵌套计算）下，选择 `City Count`。
8.  选择 `Compute Using` | `Specific Dimensions`（特定维度）并勾选 `City`：

![](Images/B18435_05_13.png)

图 5.13：特定维度

9.  再次使用 `Nested Calculations` 下的下拉菜单选择 `Size`。然后，选择 `Compute Using` | `Specific Dimensions` 并勾选 `City`。
10. 观察最终视图：

![](Images/B18435_05_14.png)

图 5.14：最终工作表

让我们思考一下前面的工作表是如何运作的：

*   **Size**：`SIZE()` 生成一个单一数字，例如特拉华州的 3，基于缓存中的分区/数据集。该数字可能会根据分区和寻址设置而变化，但不会因在分区中的移动而改变。因此，它表现为非方向性。
*   **City Count**：`IF FIRST() = 0 THEN LOOKUP([Size],0) ELSE NULL END` 字段并非严格必需。你可以直接将 `Size` 放在 `Text`/`Label` 货架上。但是，如果你这样做，你会注意到视图中的数字看起来会变粗。这是因为数字实际上被重复了，然后层叠在一起。利用 `FIRST() = 0` 只会显示一组数字。

请注意，前面的练习有一个 `Nested Calculations` 选项，这是因为 `Size` 计算字段在 `City Count` 计算字段中被引用了。

实际场景：`SIZE()` 可能是使用最少的。老实说，除了解释和演示它之外，我从未真正使用过它。但我见过其他人用 `SIZE()` 做以下事情；通过开启总计列视图，`SIZE()` 可以生成非默认聚合，如标准差。只需使用此公式：`if size()=1 then sum({STDEV([Sales])}) else sum([Sales]) end`。

如果你对表计算还不够满足，这里还有更多场景：[https://www.tableau.com/blog/top-10-tableau-table-calculations](https://www.tableau.com/blog/top-10-tableau-table-calculations)。

现在我们已经介绍了不同的表计算，我们将看到如何操作计算的分区（范围）和寻址（方向）。

# 函数的应用

到目前为止，我们已经涵盖了两个主要问题中的第一个：*函数是什么？* 现在我们将继续下一个问题：*函数如何应用？*

让我们尝试通过以下三个选项来理解这个问题，它们都是 `INDEX()` 函数的应用：

![](Images/B18435_05_15.png)

图 5.15：应用

这三个屏幕截图中都使用了 `INDEX()` 函数；然而，在每个截图中应用的方式都不同。第一和第二个截图都显示 **1**、**2** 和 **3**，但在方向上有所不同。第三个截图的范围是从 **1** 到 **9**。那么，`INDEX()` 在每种情况下是如何应用的？

回答这个问题可能会令人困惑，因为 Tableau 使用了不同的术语。在 Tableau 本身中，表计算的应用方式可能被称为 running along（沿...运行）、moving along（沿...移动）、compute using（使用计算方式），或 **partitioning (scoping)（分区/范围）** 和 **addressing (direction)（寻址/方向）**。为了我们的目的，我们将使用分区和寻址这两个术语，并根据 Tableau 文档（[https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm)）在此定义它们：

> 定义如何对计算进行分组的维度，即定义其执行所在数据范围的维度，称为分区字段。表计算在每个分区内单独执行。其余维度，即表计算所依据的维度，称为寻址字段，并决定计算的方向。

这基本上意味着分区定义了正在使用哪些字段，例如 `Segment`（细分市场）。如果你有三个细分市场分布在三个品类上，你可以单独查看每个组合、一个细分市场内的所有品类，或一个品类内的所有细分市场。你查看它的方式就是 Tableau 所说的寻址。

如果在视图中使用了表计算，你可以右键单击它并选择 `Edit Table Calculation`。这样做会显示一个对话框，允许你选择特定维度。如果勾选了某个维度，则对其进行寻址。如果未勾选，则对其进行分区。

请看下图示例：

![](Images/B18435_05_16.png)

图 5.16：寻址和分区

Tableau 提供了许多开箱即用的分区和寻址设置，包括 `Table (down)`（表格(向下)）、`Table (across)`（表格(向右)）、`Pane (down)`（窗格(向下)）和 `Cell`（单元格）。我们偶尔会提到这些选项，但不会进行详细回顾。这引出了我们的第一条分区和寻址准则：

**不要使用 Tableau 提供的开箱即用的分区和寻址设置，包括 `Table (across)` 和 `Pane (down)`。强迫自己点击 `Specific Dimensions` 并手动定义分区和寻址，以便你清楚地了解每个表计算是如何应用的。**

前面的准则有几个注意事项：

*   有一个例外，即 `Cell`。无法使用分区和寻址在视图中寻址单个单元格。相反，必须将 `Compute Using` 设置为 `Cell`，或者在 `Table Calculation` 对话框中选择 `Cell`。令人惊讶的是，沿每个单元格寻址表计算可能很有用。一个示例在与本章关联的工作簿的 `Percent of Total` 工作表上提供。在那里，它被用来显示每个单元格分为两个类别时的 100%。

