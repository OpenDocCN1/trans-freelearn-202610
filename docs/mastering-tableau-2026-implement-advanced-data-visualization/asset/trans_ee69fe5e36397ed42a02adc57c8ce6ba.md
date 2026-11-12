

- 10. 关闭对话框并按 Ctrl + V（Mac 上为 Command + V）在 Tableau 中创建新数据集。将生成的数据集（现名为 Clipboard_[timestamp]）重命名为 Lat Long。同时将工作表命名为 Lat Long。

- 11. 在 Lat Long 工作表中，将 Latitude (generated) 和 Longitude (generated) 重命名为 Lat 和 Long。

- 12. 返回 Miles 工作表，在 Transit Data 数据源中创建两个计算字段：一个名为 LAT，包含代码 AVG([Lat Long].[Lat])，另一个名为 LONG，包含代码 AVG([Lat Long].[Long])。

- 13. 从标记卡的 Text 中移除 Latitude (generated) 和 Longitude (generated)。

- 14. 将 Measure Names 放置到 列 和 筛选器 卡，并选择 Lat 和 Long。同时，将 Measure Values 放置到 文本 卡。现在，我们能够将纬度和经度视为真正的度量值。

- 15. 再创建两个计算字段：一个名为 Lookup Lat，包含代码 Lookup(Lat, -1)，另一个名为 Lookup Long，包含代码 Lookup(Long, -1)。将这两个新创建的计算字段放置到 Measure Values 卡。

- 16. 创建名为 Great Circle Distance Formula 的计算字段，代码如下：

```
3959 * ACOS (
SIN(RADIANS([Lat])) * SIN(RADIANS([Lookup Lat]))+ COS(RADIANS([Lat])) *
COS(RADIANS([Lookup Lat])) * COS(RADIANS([Lookup Long]) - RADIANS([Long]))
)
```

对于千米，将 3959 改为 6378。该数字代表地球半径，需要从英里更新为千米。

- 17. 将新创建的计算字段 Great Circle Distance Formula 放置到 Measure Values 卡。

- 18. 更改 Great Circle Distance Formula 的计算方式，使其按 City 计算：点击该字段本身并选择 使用方式：City

- 19. 相应调整以下计算

- Lookup Lat: IFNULL(LOOKUP(Lat,-1), LOOKUP(Lat,1))

- Lookup Long: IFNULL(LOOKUP(Long,-1), LOOKUP(Long,1))

- 20. 选择 Map 工作表，将标记类型设置为 Line。

- 21. 将 Trip ID 放置到 详细信息 卡。

- 22. 将 City 拖动到标记卡的底部。

- 23. 将 Great Circle Distance Formula 字段放置到 提示 卡。再次确认其仍设置为 使用方式：City。

![](img/66cf6b89349fda3581a4f196cb3c98a4_388_0.png)

图 9.6：澳大利亚距离

- 24. 再创建两个计算字段，一个名为 Source City，包含以下代码：

```
{ FIXED [Trip ID]:MIN(IF [Dest/Orig]='Source' THEN City END)}
```

第二个计算字段应命名为 Destination City，并包含以下代码：

```
{ FIXED [Trip ID]:MIN(IF [Dest/Orig]='Destination' THEN City END)}
```

- 25. 使用新创建的计算字段按需进行格式设置。特别是，请注意在以下截图中，**Source City** 位于 **颜色** 卡，而 **Destination City** 用于 **提示** 卡：

![](img/66cf6b89349fda3581a4f196cb3c98a4_389_0.png)

图 9.7：航班

总之，我们在此所做的工作如下：最初，我们向 Tableau 添加了一个数据集。随后，我们需要从工作簿中复制每个城市的纬度和经度，以创建第二个数据源，然后将其作为混合数据源用于查找纬度和经度值。借助 LOD 计算，我们可以识别出发城市和目的地城市，最后，我们能够使用所谓的大圆公式来测量城市之间的距离。可视化中的每种颜色代表一个出发城市；例如，所有绿色线条都测量从珀斯到其他城市的距离。将鼠标悬停在线条上方时，会显示出发城市和目的地城市，以及以英里（或千米）为单位的测量距离。

如果您想分析货运路线或两点之间的任何其他距离，那么您现在知道如何在 Tableau 中可视化此类数据。

![](img/66cf6b89349fda3581a4f196cb3c98a4_390_0.png)

图 9.8：美国航班出发延误

在图 9.8 中，您可以看到另一个类似的可视化，这绝对值得查看（尝试复制它）：[Tableau 解决方案工作簿：大数据比以往任何时候都更常见](https://www.tableau.com/solutions/workbook/big-data-more-common-now-ever)。

在下一节中，我们将讨论如何利用地图和多边形的组合来进一步扩展我们的 Tableau 工具箱。

## 创建自定义多边形

Tableau 原生提供多边形的地理区域包括全球机场代码、城市、国家/地区、区域、领土、州、省份，以及某些邮政编码和二级行政区（县级区域）：美国区号、核心统计区 (CBSAs)、大都市统计区 (MSAs)、国会选区和 ZIP 代码。这意味着，例如，可以轻松为世界各国创建填充地图。只需复制世界各国列表（Ctrl + C），然后在 Tableau Desktop 的空白工作表上将鼠标定位并按 Ctrl + V 将该列表粘贴到 Tableau 中。新数据源将添加到数据区域的右上角。接下来，将 Tableau 的视图类型设置为 填充地图，并将国家列表放置到 详细信息 卡。Tableau 将自动为每个数据点绘制多边形。有一些地理类型，Tableau 不会自动提供多边形。这些包括电话区号。对于这些地理类型，Tableau 会绘制符号地图，但不会绘制填充地图，如下所示：

![](img/66cf6b89349fda3581a4f196cb3c98a4_391_0.png)

图 9.9：填充地图失败

尽管选择了填充地图 **标记** 类型，Tableau 仍无法填充地图，因为这些区域的轮廓对 Tableau 来说是未知的。此外，可能会出现特殊的映射需求，需要为地图上通常不包含的区域绘制多边形。例如，组织可以定义不遵循常规地图边界的销售区域。或者，Tableau 作者可以将篮球场或足球场的图片导入 Tableau，并绘制多边形来表示比赛区域的特定部分。另一种替代方案（我们在 _第 8 章，超越基础_ 中详细讨论过）是映射商店中的书架。

除了自行绘制多边形外，还有一些文件类型支持绘制多边形。其中一种文件类型是 .shp。在下一个练习中，我们将使用此类文件并为德克萨斯州创建多边形。

通过处理上述 .shp 文件类型的示例，Tableau 可以创建多边形。让我们尝试一下，并将德克萨斯州的人口显示为德克萨斯州地图上的多边形：

- 1. 打开 **德克萨斯州交通部** 页面：https://gis-txdot.opendata.arcgis.com/datasets/txdot-city-boundaries/explore?location=30.990445%2C-100.168292%2C6.68

- 2. 使用 **下载** 下拉菜单并下载 **Shapefile**

![](img/66cf6b89349fda3581a4f196cb3c98a4_392_0.png)

图 9.10：德克萨斯州数据

在您的下载目录中，应该能找到多个文件：

![](img/66cf6b89349fda3581a4f196cb3c98a4_392_1.png)

图 9.11：德克萨斯州城市边界

如果在文本编辑器中打开 .shp 文件，将看到以下内容：

![](img/66cf6b89349fda3581a4f196cb3c98a4_393_0.png)

图 9.12：文本编辑器中的 .shp

- 3. 幸运的是，Tableau 具有读取 .shp 文件的原生功能。在与本章相关的工作簿中，导航到 City Limits 工作表并选择 数据 | 新建数据源 | 文本文件。

- 4. 导航到您的下载目录或保存德克萨斯州 shapefile 的路径，并选择 .shp 文件。

![](img/66cf6b89349fda3581a4f196cb3c98a4_393_1.png)

图 9.13：加载空间文件

- 5. 在下一个截图中，您可以看到 Tableau 如何读取 .shp 文件，以及它如何创建了一个名为 **Geometry** 的附加字段，指示我们正在查看该文件中的多边形和多重多边形。

![](img/66cf6b89349fda3581a4f196cb3c98a4_394_0.png)

图 9.14：Geometry

- 6. 打开一个新工作表，并将 **Geometry** 拖放到空白画布上。

![](img/66cf6b89349fda3581a4f196cb3c98a4_394_1.png)

图 9.15：添加几何

