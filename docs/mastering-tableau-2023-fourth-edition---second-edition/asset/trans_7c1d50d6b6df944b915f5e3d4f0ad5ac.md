
![](Images/B18435_09_05.png)

**图 9.5：距离**

您的屏幕应如上一个屏幕截图所示。请注意，交叉表非常杂乱。理想情况下，`Latitude` 和 `Longitude` 应显示在单独的列中。遗憾的是，我们无法使用生成的纬度和经度来实现这一点，因为尽管它们列在 `Data`（数据）窗格的 `Measures`（度量）部分下，但 Tableau 并不将它们视为度量。为了完成此练习，我们需要能够从单独的数据源访问纬度和经度坐标。

1. 为此，首先单击 `Map`（地图）工作表选项卡。
2. 右键单击可视化图表并选择 `View Data`（查看数据）。在生成的对话框中，通过选择每个单元格并单击 `Copy`（复制），复制 `Summary`（摘要）选项卡中的所有数据。
3. 关闭对话框，然后按 `Ctrl` + `V`（Mac 为 `Command` + `V`）在 Tableau 中创建一个新数据集。将生成的数据集（现称为 `Clipboard_[timestamp]`）重命名为 `Lat Long`。此外，将工作表命名为 `Lat Long`。
4. 在 `Lat Long` 工作表中，将 `Latitude (generated)` 和 `Longitude (generated)` 重命名为 `Lat` 和 `Long`。
5. 返回 `Miles`（英里）工作表，并在 `Transit Data`（交通数据）数据源中创建两个计算字段：一个名为 `LAT`，包含代码 `AVG([Lat Long].[Lat])`；另一个名为 `LONG`，包含代码 `AVG([Lat Long].[Long])`。
6. 从 `Marks`（标记）卡上的 `Text`（文本）中移除 `Latitude (generated)` 和 `Longitude (generated)`。
7. 将 `Measure Names`（度量名称）放置在 `Columns`（列）货架和 `Filter`（筛选器）货架上，并选择 `Lat` 和 `Long`。此外，将 `Measure Values`（度量值）放置在 `Text`（文本）货架上。现在，我们可以将纬度和经度视为真正的度量。
8. 创建两个更多的计算字段：一个名为 `Lookup Lat`，包含代码 `Lookup(Lat,-1)`；另一个名为 `Lookup Long`，包含代码 `Lookup(Long, -1)`。将新创建的两个计算字段放置在 `Measure Values`（度量值）货架上。
9. 创建一个名为 `Great Circle Distance Formula`（大圆距离公式）的计算字段，包含以下代码：

```
3959 * ACOS (
SIN(RADIANS([Lat])) * SIN(RADIANS([Lookup Lat]))+ COS(RADIANS([Lat])) * COS(RADIANS([Lookup Lat])) * COS(RADIANS([Lookup Long]) - RADIANS([Long]))
)
```

对于公里，将 `3959` 改为 `6378`。该数字代表地球半径，需要从英里更新为公里。

1. 将新创建的计算字段 `Great Circle Distance Formula` 放置在 `Measure Values` 货架上。
2. 通过单击字段本身并选择 `Compute Using: City`（按以下项计算：City），更改 `Great Circle Distance Formula` 的计算，使其使用 `City` 进行计算。
3. 相应地调整以下计算：

| **名称** | **代码** |
| --- | --- |
| `Lookup Lat` | `IFNULL(LOOKUP(Lat,-1), LOOKUP(Lat,1))` |
| `Lookup Long` | `IFNULL(LOOKUP(Long,-1), LOOKUP(Long,1))` |

1. 选择 `Map` 工作表，将 `Marks` 类型设置为 `Line`（线）。
2. 将 `Trip ID` 放置在 `Detail`（详细信息）货架上。
3. 将 `City` 拖到 `Marks` 卡视图的底部。
4. 将 `Great Circle Distance Formula` 字段放置在 `Tooltip`（工具提示）货架上。再次检查它是否仍设置为 `Compute Using: City`：

![](Images/B18435_09_06.png)

**图 9.6：澳大利亚距离**

1. 创建两个更多的计算字段，一个名为 `Source City`，包含以下代码：

```
{ FIXED [Trip ID]:MIN(IF [Dest/Orig]='Source' THEN City END)}
```

第二个计算字段应名为 `Destination City`，并包含以下代码：

```
{ FIXED [Trip ID]:MIN(IF [Dest/Orig]='Destination' THEN City END)}
```

1. 使用新创建的计算字段按需格式化。特别请注意，在以下屏幕截图中，`Source City` 位于 `Color`（颜色）货架上，而 `Destination City` 用于 `Tooltip` 货架上：

![](Images/B18435_09_07.png)

**图 9.7：航班**

总之，我们最初向 Tableau 添加了一个数据集。然后我们需要将每个城市的纬度和经度从工作簿中复制出来，以创建第二个数据源，随后我们将其作为混合数据源用于查找纬度和经度值。借助 LOD 计算，我们可以识别出发城市和目的城市，最后，我们能够使用所谓的大圆公式来测量城市之间的距离。可视化图表中的每种颜色代表一个起始城市；例如，所有绿线测量从珀斯到其他城市的距离。悬停在一条线上时，将显示出发城市、目的城市以及以英里（或公里）为单位测量的距离。

如果您想分析货运路线或任意两点之间的距离，现在您已经确切知道如何在 Tableau 中可视化此类数据。

![](Images/B18435_09_08.png)

**图 9.8：美国航班出发延误**

在*图 9.8*中，您可以看到另一个类似的可视化图表，绝对值得一看（尝试复制它！）：[https://www.tableau.com/solutions/workbook/big-data-more-common-now-ever](https://www.tableau.com/solutions/workbook/big-data-more-common-now-ever)。

在下一节中，我们将讨论如何利用地图和多边形的组合来进一步扩展我们的 Tableau 工具箱。

# 创建自定义多边形

Tableau 原生提供多边形的地理区域包括全球机场代码、城市、国家、地区、领土、州、省，以及一些邮政编码和二级行政区（相当于县）：美国区号、`Core-Based Statistical Areas`（`CBSAs`，核心统计区）、`Metropolitan Statistical Areas`（`MSAs`，大都市统计区）、国会选区和邮政编码。这意味着，例如，可以轻松创建世界国家的填充地图。只需复制世界国家列表（`Ctrl` + `C`），然后在 Tableau Desktop 的空白工作表上按 `Ctrl` + `V` 将该列表粘贴到 Tableau 中。一个新数据源将添加到右上角的 `Data`（数据）下。接下来，将 Tableau 中的 `View`（视图）类型设置为 `Filled Map`（填充地图），并将国家列表放在 `Detail`（详细信息）货架上。Tableau 将自动为每个数据点绘制多边形：

![](Images/B18435_09_09.png)

**图 9.9：原生多边形**

有些地理类型 Tableau 不会自动提供多边形。这些包括电话区号。对于这些地理类型，Tableau 将绘制符号地图，而不是填充地图，如下所示：

![](Images/B18435_09_10.png)

**图 9.10：填充地图失败**

即使选择了 `Filled Map` `Marks` 类型，Tableau 也无法填充地图，因为这些区域的轮廓对 Tableau 来说是未知的。此外，可能会出现特殊的映射需求，要求为地图上通常不包含的区域绘制多边形。例如，组织可能定义不遵循常规地图边界的销售区域。或者，Tableau 作者可能将篮球场或足球场的图像导入 Tableau 并绘制多边形以表示比赛区域的特定部分。另一种替代方案（我们在*第 8 章 超越基础*中详细讨论过）可能是绘制商店中的书架地图。

但除了自己绘制多边形外，还有一些文件类型支持绘制多边形。其中一种文件类型是 `.shp`。在下一个练习中，我们将使用此类文件为德克萨斯州创建多边形。

## 德克萨斯州的多边形

通过处理上述 `.shp` 文件类型的示例，Tableau 可以创建多边形。让我们试一下，在德克萨斯州地图上以多边形形式显示德克萨斯州的人口：
