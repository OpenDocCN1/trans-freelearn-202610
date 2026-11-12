

使用 LOD 计算来显示这些值非常简单。让我们探索一下。按照练习步骤操作：

- 1. 选择 Practical Include 工作表
- 2. 选择 Superstore 数据集

- 3. 创建一个名为 Per State INCLUDE 的计算字段，使用以下代码：

```
{INCLUDE [State]:SUM([Sales])}
```

- 4. 将 Region 拖到 Columns 栏，SUM(Sales) 拖到 Rows 栏，AVG(Per State INCLUDE) 拖到 Details 栏。请务必将 Per State INCLUDE 更改为平均聚合。

- 5. 右键单击 Sales 轴并选择 Add Reference Line，添加两条参考线。使用以下设置：

![](img/66cf6b89349fda3581a4f196cb3c98a4_317_0.png)

### 图 7.41：参考线

- 6. 按需格式化工作表以完成它：

![](img/66cf6b89349fda3581a4f196cb3c98a4_318_0.png)

### 图 7.42：按区域和州计算的平均值

由此可见，得益于 LOD，我们可以为每个 **区域** 显示一条折线图，同时还可以为整个数据集显示一条。之所以可行，是因为我们在 **Per State INCLUDE** 计算中包含了 **州** 维度。使用此计算字段创建参考线，使 Tableau 不仅能显示每个（可见） **区域** 的平均值，还能显示每个 **区域** 下每个（不可见） **州** 的平均值。

## 使用 LOD EXCLUDE 计算

在本练习中，我们将使用 Superstore 数据集创建一个工作表，用于计算区域内每个城市产生的销售额百分比。

按照练习步骤操作：

- 1. 选择 Practical Exclude 工作表
- 2. 选择 Superstore 数据集
- 3. 创建以下计算字段：

```
Region Values
{ EXCLUDE [City],[State] :ZN( SUM([Sales]) ) }
```

```
% Per Region LOD
SUM([Sales])/SUM( [Region Values] )
```

### 图 7.43：区域值和按区域计算的 LOD 百分比

- 4. 将 Region 和 City 放在 Rows 栏上

- 5. 将 Measure Names 放在 Columns 栏上，Measure Values 放在 Text 栏上

- 6. 从 Measure Values 栏中删除所有度量实例，只保留 Sales、Region Values 和 % Per Region LOD

- 7. 为了让 % Per Region LOD 显示为百分比，必须调整数字格式。只需在“数据”窗格中右键单击该计算，然后选择“默认属性 | 数字格式 | 百分比”：

![](img/66cf6b89349fda3581a4f196cb3c98a4_319_0.png)

### 图 7.44：更改数字格式

- 8. 将 AGG(% Per Region LOD) 的一个实例放在 Filter 栏上，并调整以显示至少 0.1（即 10%）：

![](img/66cf6b89349fda3581a4f196cb3c98a4_320_0.png)

### 图 7.45：% 筛选器

- 9. 观察当前视图，Region 和 City 在 Rows 栏上：

![](img/66cf6b89349fda3581a4f196cb3c98a4_320_1.png)

### 图 7.46：Jacksonville 仍然可见

- 10. 现在，将 State 放在 Rows 栏的 Region 和 City 之间。请注意 Jacksonville 消失了：

![](img/66cf6b89349fda3581a4f196cb3c98a4_321_0.png)

### 图 7.47：将 Region 添加到 Rows 时 Jacksonville 消失

之所以会这样，是因为 Jacksonville 位于南区域的两个州，即佛罗里达州和北卡罗来纳州。您可以通过将 % Per Region LOD 筛选器替换为 City: Jacksonville 来查看这一点：

![](img/66cf6b89349fda3581a4f196cb3c98a4_321_1.png)

### 图 7.48：Jacksonville 存在于两个州中

我们的 Region Values 计算排除了 City 和 State，因此 % Per Region LOD 将考虑视图中可见内容的 SUM(Sales)，除以 Region Values。当视图中仅包含 Region 和 City 时，Jacksonville 的 SUM(Sales) 为 44,713（见图 7.48）。该数字除以 Region Values 的 391,722（见图 7.46）大于 10%。但随后我们将 State 添加到视图中，使得佛罗里达州 Jacksonville 的 SUM(Sales) 变为 39,133，北卡罗来纳州 Jacksonville 变为 5,580（见图 7.50）。这两个数字除以 391,722 的 Region Values 都不超过 10%，因此会因我们的筛选器而从视图中删除。

在使用 Tableau 时，LOD 是较为高级的主题之一，但它们非常强大，值得花时间去理解。如需更多实际示例，我建议您查看以下网站：https://www.tableau.com/about/blog/LOD-expressions。我记得刚开始使用 Tableau 时，我从未想过自己会想要显示基于视图中不存在的维度的数据，或者想要排除维度。但我曾多次需要这样做，例如，在将“到总行平均值的距离”的计算添加到仪表板时，或者当一个维度有多个最大日期且您只想显示每个单独最大日期的度量时。我回顾 LOD 文档的次数比您想象的还要多，我只能鼓励您也这样做；您对 LOD 越了解，就越容易掌握该概念并自然地使用它们。LOD 为您的仪表板带来的额外灵活性令人难以置信。

## 跟随练习的仪表板

在本章的跟随练习仪表板中，我们有一个非常酷的数据集可供使用。它是来自美国运输部/美国交通统计局（BTS）的美国航空公司准点率数据。让我们开始吧！

- 1. 从以下网址下载承运人准点率数据集：https://www.kaggle.com/datasets/mexwell/carrier-on-time-performance-dataset
- 2. 将 Tableau 连接到数据集
- 3. 接下来，我们将构建三个工作表并将它们组合到一个仪表板中
- 让我们从工作表 1 开始：随时间变化的延误 vs 路线基准（FIXED）。

- 1. 打开一个新工作表
- 2. 将 Flight Date 拖到 Columns。右键单击并设置为 Month。
- 3. 将 Arr Delay 拖到 Rows。如有需要，将聚合更改为 AVG。
- 4. 将 Flight Date 拖到 Filters
- 5. 创建字段 Avg ArrDelay(FIXED) = { FIXED : AVG([Arr Delay])}
- 6. 将 Route Avg ArrDelay (FIXED) 拖到视图中，将其作为 Rows 上的第二个度量并使用双轴

这显示了什么：

- 折线 = 平均延误随时间的变化情况（所选航班）
- 第二条线 = 来自 FIXED LOD 的稳定路线基准

### 图 7.49：延误 vs 基准

接下来，我们将创建工作表 2：KPI 磁贴（LOD 指标摘要）：

- 1. 打开一个新工作表。
- 2. 将 **Origin** 拖到 **Rows**。
- 3. 将 **YEAR(Flight Date)** 拖到 **Columns**。同时也将 **AVG(Arr Delay)** 和 **Arr Delay without Origin = { EXCLUDE [Origin]: AVG([Arr Delay]) }** 拖到 **Columns**。
- 4. 将两个度量设为双轴，并同步它们。
- 5. 在标记卡中为 **到达延误** 选择 **条形图**，为另一个度量选择 **折线图**。
- 6. 将 **YEAR(Flight Date)** 放到 **Filter** 栏，保留 2018、2019 和 2020。现在您有了一个由 LOD 驱动的摘要面板。

### 图 7.50：按起源划分的延误

接下来，我们将创建工作表 3：按航空公司划分的延误：

- 1. 打开一个新工作表
- 2. 将 Reporting Airline 拖到 Columns
- 3. 将 Dest State 拖到 Rows
- 4. 将 AVG(Arr Delay) 放在 Color 上

![](img/66cf6b89349fda3581a4f196cb3c98a4_324_1.png)

### 图 7.51：按航空公司划分的到达延误

- 4. 最后，构建仪表板

- 5. 拖入：
  - 左上和中间：工作表 1：随时间变化的延误 vs 基准
  - 左下和中间：工作表 3：按航空公司划分的到达延误
  - 右侧从上到下：工作表 2：按起源划分的延误
  - 如果您喜欢整洁的布局，可以使用垂直和水平容器对它们进行排列

- 6. 为最终润色，添加以下标题：
  - 仪表板：航班延误（LOD 仪表板）
  - 根据喜好编辑颜色和字体；

![](img/66cf6b89349fda3581a4f196cb3c98a4_325_0.png)

### 图 7.52：最终仪表板

现在，您已经拥有了自己的 LOD 驱动的 Tableau 仪表板

## 摘要

本章首先探讨了 LOD 为何如此有影响力，以及其加入 Tableau 为何备受赞誉。接着，我们构建了两个游乐场来探索三种 LOD——FIXED、EXCLUDE 和 INCLUDE——的工作原理。Tableau 的默认行为是基于视图中可见的维度进行计算。例如，如果视图中有州，销售额将按州显示。如果添加城市，销售额将按州、按城市进行调整。但是，如果您想操纵此默认逻辑，可以使用 LOD。它们允许您基于任何维度计算度量，无论该维度是否在视图中表示。我们还看到，FIXED LOD 在 Tableau 的操作顺序中高于 EXCLUDE 和 INCLUDE LOD。这一点很重要，以便您在仪表板中使用正确的 LOD 和/或筛选器。

在下一章中，我们将把注意力转向 Tableau 的可视化方面，探索不同的图表类型以及不常见但非常有用的可视化效果。

