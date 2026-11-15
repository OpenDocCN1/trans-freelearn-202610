
# 创建带有趋势线的散点图

为了更快地创建 `Y-Axis` 计算字段，请将 `X-Axis` 计算字段复制并粘贴到 Excel 中，然后查找并替换 `` `[Parameters].[X-Axis]` `` 为 `` `[Parameters].[Y-Axis]` ``。

1. 将 `X-Axis` 计算字段拖至 `Columns`（列），将 `Y-Axis` 拖至 `Rows`（行）。同时，将 `ID` 字段放入 `Detail`（详细信息）卡片。

2. 为了查看 Tableau 的相关性分析，将 `Trend Line`（趋势线）拖到工作表上：

![](Images/B18435_12_06.png)

图 12.6：趋势线

1. 带有趋势线的工作表如下所示。当鼠标悬停在趋势线上时，您将看到线的方程、`R-Squared`（R 平方）值和 `P-value`（P 值）的值：

![](Images/B18435_12_07.png)

图 12.7：趋势线数值

`Y-Axis` 方程意味着，对于 `x-axis`（x 轴）上的每一点，`y-axis`（y 轴）的值将增加 17.9717，起始于 `x-axis` 上的 0 点和 `y-axis` 上的 -4.33685 点。因此，总体而言，`y-term`（y 项）的增长速度快于（同样在增长的）`x-term`（x 项）。`R-Squared` 值解释了趋势线解释了多少方差（80.81%），最后，`P-value` 解释了模型的显著性。`P-value` 值小于 0.05 被认为是显著的，意味着数据有 5% 的概率是随机遵循趋势线的。上图中的趋势线 `P-value` 小于 0.01%。因此，我们可以放心地假设这两个变量之间存在真实关系。

1. 要查看更多系数，请右键单击该线并选择 `Describe Trend Line…`（描述趋势线……）：

![](Images/B18435_12_08.png)

图 12.8：描述趋势线……

将出现以下窗口：

![](Images/B18435_12_09.png)

图 12.9：趋势线信息

1. 或者，您可以选择 `Describe Trend Model…`（描述趋势模型……）选项，您将看到以下内容：

![](Images/B18435_12_10.png)

图 12.10：趋势模型信息

如果您想了解更多关于解读趋势线的信息，可以阅读以下文章：[https://onlinehelp.tableau.com/current/pro/desktop/en-us/trendlines_add.htm](https://onlinehelp.tableau.com/current/pro/desktop/en-us/trendlines_add.htm)。

1. 网格线有点分散我的注意力，所以我决定通过右键单击屏幕并选择 `Format…`（格式……）来移除它们：

![](Images/B18435_12_11.png)

图 12.11：格式设置

1. 我在 `Format`（格式）窗口中选择第五个选项“Lines”（线），并将 `Grid Lines`（网格线）值设置为 `None`（无）：

![](Images/B18435_12_12.png)

图 12.12：移除网格线

1. 我还将 `Marks`（标记）从 `Automatic`（自动）更改为 `Circle`（圆形），并将颜色更改为黑色。

2. 现在，我认为在散点图上显示每个点代表哪个国家会很有帮助，所以我将 `Marks`（标记）卡片中的 `ID` 字段从 `Detail`（详细信息）更改为 `Label`（标签）；然而，这看起来有点太乱了：

![](Images/B18435_12_13.png)

图 12.13：ID 转为文本

1. 我点击撤销箭头，并通过右键单击 `ID` 并选择 `Show Highlighter`（显示高亮器）来添加高亮器功能。现在，最终用户将能够搜索某个国家，该点将在散点图中高亮显示：

![](Images/B18435_12_14.png)

图 12.14：显示高亮器

1. 我还希望在悬停任意点时，国家名称能以红色清晰地显示在工具提示中，我通过在 `Edit Tooltip`（编辑工具提示）中将 `ID` 颜色更改为红色并增大字号来实现这一点：

![](Images/B18435_12_15.png)

图 12.15：工具提示

结果如下所示：

![](Images/B18435_12_16.png)

图 12.16：悬停某一点

1. 我已经添加了趋势线，现在我想在视图中显示 `Pearson R`（皮尔逊相关系数），也称为 `R-Squared` 值，以便用户知道两个指标是否相关；但是，无法将 `Describe Trend Line` 或 `Describe Trend Model` 选项设置为始终显示。因此，我将在一个新字段中自行计算该值；我使用 `CORR()` 函数来实现这一点：

![](Images/B18435_12_17.png)

图 12.17：Pearson R

1. 但我想显示一些文本，指示两个变量是否相关，而不是显示数值本身，因为我想让我的仪表板更清晰。因此，我创建了另一个名为 `Correlation yes or no`（相关性是或否）的计算字段，代码如下：

```
IF [Pearson R] > 0.7 then 'The two variables <' + [Parameters].[X-Axis]+ '> and <'+ [Parameters].[Y-Axis]+ '> have a very strong positive correlation of: ' + STR([Pearson R])
ELSEIF [Pearson R] < 0.7 and [Pearson R] > 0.4 then 'The two variables <'  + [Parameters].[X-Axis] + '> and <'+ [Parameters].[Y-Axis]+ '> have a strong positive correlation of: '  + STR([Pearson R])
ELSEIF [Pearson R] < 0.4 and [Pearson R] > 0.2 then 'The two variables <'  + [Parameters].[X-Axis] + '> and <'+ [Parameters].[Y-Axis]+ '> have a moderate positive correlation of: ' + STR([Pearson R])
ELSEIF [Pearson R] <0.2 and [Pearson R] > -0.2 then 'The two variables <'  + [Parameters].[X-Axis] + '> and <'+ [Parameters].[Y-Axis]+ '> have no or a weak correlation of:' + STR([Pearson R])
ELSEIF [Pearson R] < -0.2 and [Pearson R] >-0.4 then 'The two variables <'  + [Parameters].[X-Axis] + '> and <'+ [Parameters].[Y-Axis]+ '> have a moderate negative correlation of: '+ STR([Pearson R])
ELSEIF [Pearson R] < -0.4 and [Pearson R] > -0.7 then 'The two variables <'  + [Parameters].[X-Axis] + '> and <'+ [Parameters].[Y-Axis]+ '> have a strong negative correlation of: ' + STR([Pearson R])
ELSEIF [Pearson R] < -0.7 THEN 'The two variables <'  + [Parameters].[X-Axis] + '> and <'+ [Parameters].[Y-Axis]+ '> have a very strong negative correlation of: ' + STR([Pearson R])
END
```

2. 完成此操作后，我创建一个新工作表，并将 `Correlation yes or no` 放在 `Text`（文本）卡片上。正如您所见，该工作表非常简单：

![](Images/B18435_12_18.png)

图 12.18：相关性文本

1. 我右键单击标题并选择 `Hide Title`（隐藏标题）。

最后，几乎所有准备工作都已就绪，可以根据本节开头的草图构建仪表板了。只剩下两个轴的分布图还缺失。

# 添加轴分布图

我们已经有了一个好的开端，但现在我想在散点图旁边添加 `x-axis`（x 轴）和 `y-axis`（y 轴）的分布图。我采取以下步骤：

1. 我从 `x-axis` 开始，打开另一个工作表，将 `X-Axis` 拖至 `Rows`（行）。接下来，我单击刚添加的 `X-Axis` 并选择 `Measure | Count`（度量 | 计数）。现在右键单击数据窗格中的 `X-Axis` 并选择 `Create | Bins…`（创建 | 箱……）。在弹出窗口中，点击 `OK`。最后，我将新维度 `X-Axis (bin)` 拖至 `Columns`（列）。

2. 关闭 `Edit Axis [X-Axis]`（编辑轴 [X-Axis]）窗口后，我右键单击 `X-Axis` 并取消选择 `Show Header`（显示页眉），以不再显示页眉。

3. 我在另一个新工作表上继续处理 `Y-Axis`，将 `Y-Axis` 拖至 `Columns`（列）。接下来，我通过单击 `Columns` 上的 `Y-Axis` 并选择 `Measure | Count`（度量 | 计数）来更改它。现在，右键单击数据窗格中的 `Y-Axis` 并选择 `Create | Bins…`（创建 | 箱……）。在弹出窗口中，我点击 `OK`。

![](Images/B18435_12_19.png)

图 12.19：箱

1. 最后，我将新维度 `Y-Axis (bin)` 拖至 `Rows`（行）。

2. 我在 `Marks`（标记）卡片中将颜色更改为灰色，并在页面顶部选择 `Entire View`（整个视图）。

3. `y-axis` 分布应与散点图对齐；因此，我单击 `Y-Axis (bin)` | `Sort`（排序）| `Descending`（降序）。接下来，我右键单击 `Y-Axis` 并取消选择 `Show Header`（显示页眉），以不再显示页眉。

4. 为了将所有内容组合在一起，我打开一个仪表板页面并将其命名为 `Distribution`（分布），然后将 `Scatterplot`（散点图）工作表放在页面底部，将 `Correlation`（相关性）工作表放在仪表板画布顶部。

5. 我将分布工作表添加到散点图的相关侧（`Y-Axis` 添加到 `y-axis`，`X-Axis` 添加到 `x-axis`）。
