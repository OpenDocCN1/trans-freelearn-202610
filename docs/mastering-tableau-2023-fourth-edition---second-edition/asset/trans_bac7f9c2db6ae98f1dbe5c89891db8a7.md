

```sql
SELECT 
[t0].[Country] AS [Country], 
[t1].[ measure 0] AS
[TEMP(Calculation_418553293854285824)(2417030171)(0)],
[t0].[TEMP(Calculation_418553293854285824)(616435453)(0)] AS
[TEMP(Calculation_418553293854285824)(616435453)(0)] 
FROM 
( SELECT
['2012_World_Pop$'].[Country] AS [Country], SUM(['2012_World_Pop$'].[Population])
AS [TEMP(Calculation_418553293854285824)(616435453)(0)] 
FROM
[dbo].['2012_World_Pop$'] ['2012_World_Pop$'] 
WHERE 
(['2012_World_Pop$'].[Country]= 'Afghanistan') 
GROUP BY 
['2012_World_Pop$'].[Country] ) [t0] 
CROSS JOIN
( SELECT SUM(['2012_World_Pop$'].[Population]) AS [ measure 0] FROM
[dbo].['2012_World_Pop$'] ['2012_World_Pop$'] 
GROUP BY () ) [t1]
```

请注意术语 `CROSS JOIN`；LOD 计算生成的查询指示底层数据源引擎以一种方式返回数据，使 Tableau 能够将一个或多个国家的人口值除以世界人口总数，从而返回正确的总百分比。

## 使用 LOD INCLUDE 计算

接下来，我们将练习 `INCLUDE` LOD 计算。在本练习中，我们将创建一个显示以下内容的工作表：

*   每个区域的总销售额
*   所有区域总销售额的平均值
*   每个区域内所有州总销售额的平均值

使用 LOD 计算来显示这些值非常简单。让我们一探究竟。请跟随练习步骤操作：

1.  选择 `Practical Include` 工作表。
2.  选择 `Superstore` 数据集。
3.  创建一个名为 `Per State INCLUDE` 的计算字段，代码如下：

``` 
{INCLUDE [State]:SUM([Sales])}
```

4.  将 `Region` 拖至 `Columns` 货架，`SUM(Sales)` 拖至 `Rows` 货架，`AVG(Per State INCLUDE)` 拖至 `Details` 货架。请务必将 `Per State INCLUDE` 的聚合方式更改为平均值。
5.  通过右键单击 `Sales` 轴并选择 `Add Reference Line` 来添加两条 `Reference Lines`。使用以下设置：

![](Images/B18435_07_41.png)

图 7.41：参考线

1.  根据需要格式化工作表以完成它：

![](Images/B18435_07_42.png)

图 7.42：每个区域和州的平均值

正如你所见，得益于 LOD，我们可以显示每个 `Region` 的折线图，以及整个数据集的折线图。之所以能做到这一点，是因为我们在 `Per State INCLUDE` 计算中包含了 `State` 维度。使用此计算字段创建参考线，使 Tableau 不仅能显示每个（可见）`Region` 的平均值，还能显示每个 `Region` 内每个 `State`（不可见）的平均值。

## 使用 LOD EXCLUDE 计算

在本练习中，我们将使用 `Superstore` 数据集创建一个工作表，计算每个区域中每个城市产生的销售额百分比。

请跟随练习步骤操作：

1.  选择 `Practical Exclude` 工作表。
2.  选择 `Superstore` 数据集。
3.  创建以下计算字段：

![](Images/B18435_07_43.png)

图 7.43：区域值和每区域百分比 LOD

1.  将 `Region` 和 `City` 放在 `Rows` 货架上。
2.  将 `Measure Names` 放在 `Columns` 货架上，将 `Measure Values` 放在 `Text` 货架上。
3.  从 `Measure Values` 货架中移除所有度量实例，仅保留 `Sales`、`Region Values` 和 `% Per Region LOD`。
4.  为了使 `% Per Region LOD` 以百分比形式显示，必须调整数字格式。只需在 `Data` 窗格中右键单击该计算字段，然后选择 `Default Properties` | `Number Format` | `Percentage`：

![](Images/B18435_07_44.png)

图 7.44：更改数字格式

1.  将 `AGG(% Per Region LOD)` 的一个实例放在 `Filter` 货架上，并调整以显示至少 `0.1`（即 10%）：

![](Images/B18435_07_45.png)

图 7.45：百分比筛选器

1.  观察当前视图，`Region` 和 `City` 在 `Rows` 货架上：

![](Images/B18435_07_46.png)

图 7.46：Jacksonville 仍然可见

1.  现在，将 `State` 放在 `Rows` 货架上 `Region` 和 `City` 之间。请注意 `Jacksonville` 消失了：

![](Images/B18435_07_47.png)

图 7.47：将 Region 添加到 Rows 时 Jacksonville 消失

之所以发生这种情况，是因为 `Jacksonville` 存在于 `South` 区域的两个州中，即 `Florida` 和 `North Carolina`。你可以通过将 `% Per Region LOD` 筛选器替换为 `City`: `Jacksonville` 来查看这一点：

![](Images/B18435_07_48.png)

图 7.48：Jacksonville 存在于两个州中

我们的 `Region Values` 计算排除了 `City` 和 `State`，因此 `% Per Region LOD` 将考虑视图中可见内容的 `SUM(Sales)`，除以 `Region Values`。当视图中只有 `Region` 和 `City` 时，`Jacksonville` 的 `SUM(Sales)` 为 **44,713**（见 *图 7.48*）。该数字除以 **391,722** 的 `Region Values`（见 *图 7.46*）大于 10%。但随后我们向视图添加了 `State`，将 `SUM(Sales)` 改为 `Florida` 中 `Jacksonville` 的 **39,133** 和 `North Carolina` 中 `Jacksonville` 的 **5,580**（见 *图 7.50*）。当它们分别除以 `Region Values` 的 **391,722** 时，没有一个超过 10%，因此由于我们的筛选器，它们将从视图中消失。

LOD 是使用 Tableau 时较高级的主题之一，但它们非常强大，值得花时间去理解。想要更多实际示例，我建议你查看以下网站：[https://www.tableau.com/about/blog/LOD-expressions](https://www.tableau.com/about/blog/LOD-expressions)。我记得当我最初开始使用 Tableau 时，我不认为我会想要显示基于视图中不存在的维度的数据，或者想要排除维度。但我无数次需要这样做，例如，当向仪表板添加到总行平均值的距离计算时，或者当一个维度有多个最大日期而你只想显示每个单独最大日期的度量时。我回顾 LOD 文档的次数多得难以想象，我只能鼓励你也这样做；你阅读关于 LOD 的内容越多，就越容易掌握其概念并自然地使用它们。LOD 为仪表板带来的额外灵活性令人难以置信。

## 总结

我们本章首先探讨了 LOD 为何如此有影响力，以及为何它们被纳入 Tableau 备受赞誉。接下来，我们构建了两个演示场来探索三种 LOD——`FIXED`、`EXCLUDE` 和 `INCLUDE`——的工作原理。Tableau 的默认行为是基于视图中可见的维度进行计算。例如，如果视图中有州，销售额将按州呈现。如果你添加城市，销售额将按州、按城市调整。但是，如果你想操纵这种默认逻辑，可以使用 LOD。它们允许你基于任何维度计算度量，无论该维度是否在视图中呈现。我们还了解到，在 Tableau 的运算顺序中，`FIXED` LOD 高于 `EXCLUDE` 和 `INCLUDE` LOD。这一点很重要，以便你在仪表板中使用正确的 LOD 和/或筛选器。

在下一章中，我们将把注意力转向 Tableau 的可视化方面，探索不同的图表类型以及不太常见但非常有用的可视化图表。

## 在 Discord 上了解更多

要加入本书的 Discord 社区——你可以在其中分享反馈、向作者提问并了解新版本——请扫描下方的二维码：

[https://packt.link/tableau](https://packt.link/tableau)

![](Images/QR_Code2044012316175764640.png)

# 超越基础

