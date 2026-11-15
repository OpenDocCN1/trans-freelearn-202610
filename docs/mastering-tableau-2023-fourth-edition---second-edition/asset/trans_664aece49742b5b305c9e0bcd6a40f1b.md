
# 创建四层地图

1. 我们需要使用一个新的 Excel 工作表，因为我们必须为目的地和起点创建空间字段。由于某些城市既是起点也是目的地，我们将创建两个内连接；两次连接都将初始的 `freight` Excel 工作表连接到新创建的 `latlong` Excel 工作表（如果您从 GitHub 下载数据，`latlong` 表是 `Freight.xlsx` 中的第二个选项卡）：

![](Images/B18435_09_30.png)

图 9.30：联接

1. 唯一的区别在于第一个联接基于 `Origin` 和 `City`，第二个基于 `Destination` 和 `City`。通过双击名称重命名 `latlong` 数据源，以避免混淆：

![](Images/B18435_09_31.png)

图 9.31：联接 II

1. 打开一个新工作簿并将其命名为 `4-layer map`。创建一个名为 `Line` 的计算字段，代码如下：

```
MAKELINE(MAKEPOINT([Lat (Origin)], [Long (Origin)]), MAKEPOINT([Lat (Destination)], [Long (Destination)]))
```

2. Tableau 的说明显示，要基于空间位置创建点，Tableau 需要经度和纬度，或坐标。

![](Images/B18435_09_32.png)

图 9.32：MAKEPOINT

1. 创建另一个名为 `Days to delivery` 的计算字段，代码如下：

```
[Delivered]-[Ordered]
```

2. 双击 `Origin` 文件中的 `City`，地图将出现，但您会遇到 `12 unknown`。点击错误，然后选择 `Edit Locations…`：

![](Images/B18435_09_33.png)

图 9.33：未知城市

1. 您将看到 Tableau 使用 `United Kingdom`（对您可能不同）作为默认国家来查找每个城市，但 Tableau 无法成功匹配，因为我们的数据集包含全欧洲的城市。不要使用 `Fixed` 位置，而是使用 `From field` 选项并选择 `Origin Country`。现在该字段将作为我们数据集中每个城市的国家：

![](Images/B18435_09_34.png)

图 9.34：起点国家

1. Tableau 自动将 `Origin Country` 添加到视图的 `Detail` 货架上，并正确显示城市。您也可以通过直接将 `Origin Country` 放置在 `Detail` 货架上来实现上一步：

![](Images/B18435_09_35.png)

图 9.35：起点城市

1. 由于我们的目标之一是在 `Origin` 和 `Destination` 城市之间绘制线条，因此添加 `Destination` 也是合理的，对吗？双击 `Destination` 将其添加到可视化中。

2. 我们现在在视图中拥有所有 `Origin` 和 `Destination` 城市，但无法区分它们，因为它们都在同一层上。因此，将刚添加的 `Destination` 字段从 `Detail` 货架拖动到视图左上角，直到出现 `Add a Marks Layer` 标志：

![](Images/B18435_09_36.png)

图 9.36：货运 II

1. 您现在将看到 `Marks` 卡显示两层：

![](Images/B18435_09_37.png)

图 9.37：Marks

1. 通过双击名称将 `Destination` 层重命名为 `Destination Country`。

2. 通过将 `Line` 拖到 `Add a Marks Layer` 上添加另一层，并将 `Days to Delivery` 添加到 `Color`。将此层命名为 `Line`，并从 `Marks` 卡下拉菜单中选择 `Automatic` 类型：

![](Images/B18435_09_38.png)

图 9.38：Marks 层

1. 通过将 `City` 拖到 `Add a Marks Layer` 上添加另一层，并将其命名为 `City (2)`。将 `Destination Country` 添加到 `Detail`。从 `Marks` 卡下拉菜单中选择 `Automatic` 类型：

![](Images/B18435_09_39.png)

图 9.39：Marks 层

1. 最后，返回 `Destination Country` 层，将 `Destination Country` 改为 `Detail`，并将 `Weight` 放在 `Color` 上。从 `Marks` 卡下拉菜单中选择 `Map`：

![](Images/B18435_09_40.png)

图 9.40：Marks 层

1. 通过拖放按以下顺序排序图层：`Line`、`City`、`City (2)` 和 `Destination Country`。您的工作表现在应如下所示：

![](Images/B18435_09_41.png)

图 9.41：地图

1. 为了使可视化更易于解读，将 `Origin` 放在 `Filters` 货架上，并仅选择您想要的几个起点。

2. 为了区分 `Origin` 和 `Destination`，打开 `City` 和 `City (2)` 地图层，分别将 `City` 和 `City (Destination)` 从 `Detail` 改为 `Shape`。为 `Origin` 选择一种形状，为 `Destination` 选择另一种形状，例如，`Origin` 使用实心圆，`Destination` 使用十字。您也可以通过点击 `Color` 货架为形状着色：

![](Images/B18435_09_42.png)

图 9.42：最终分层地图

现在，让我们看看我们创建了什么。我们可以看到货物从每个圆圈运输到十字城市。两个城市由黄到红渐变的线条连接。偏黄表示交付快，偏红表示交付时间长。目的地国家以青绿到深蓝的色彩范围着色。颜色越深，运输过去的重量越大。我们可以轻松发现意大利接收了最重的运输，来自伊斯坦布尔，并在 6 天内送达。最慢的交付是从布达佩斯到华沙，耗时 26 天。

希望这个练习向您展示了分层地图的优势。每一层都允许您以独特的方式使用 `Color`、`Shape`、`Size`、`Label` 和其他 `Marks`，为您的分析开辟新路径。在下一节中，我们将更进一步，在 Tableau 中使用外部技术制作地图。

# 利用其他技术扩展 Tableau 制图功能

接下来，我们将探讨如何使用其他工具扩展 Tableau 的制图功能。Tableau 的开发人员精心创建了一个易于扩展的制图界面。这种可扩展性的某些领域，例如连接到 `Web Map Service` (`WMS`) 服务器，可直接从界面获得；我们将在下一节探讨。

## 使用 WMS 自定义地图

将自定义地图引入 Tableau 的最简单方法是直接从 Desktop 进行。我们需要一个指向 `WMS` 服务器的格式正确的 URL。Tableau Desktop 可以连接到任何支持 `WMS 1.0.0`、`1.1.0` 或 `1.1.1` 标准的 `WMS` 服务器。

寻找此类 URL 列表的好地方是 [http://directory.spatineo.com](http://directory.spatineo.com)，它提供了大量不同制图服务的信息。

以下练习受 Jeffrey A. Shaffer 的文章 *[Building weather radar in Tableau in under 1 minute](https://www.dataplusscience.com/TableauWeatherRadar.html)* 启发。不过，我们将使用不同的地图，我们的来源是 *NASA Earth Observation* 数据 ([https://neo.gsfc.nasa.gov/](https://neo.gsfc.nasa.gov/))。在本练习中，我们将看到，通过在数据集下方绘制额外数据——以背景图像的形式——您将能够为您的业务做出更好的决策。例如，如果您的销售数字取决于好（或坏）天气，或者您的生产面临附近野火的风险，您将能够在一个 Tableau 仪表板中看到所有这些信息：

1. 打开与本章关联的工作簿，通过 Desktop 导航到 `WMS Server` 工作表。

2. 选择 `Superstore` 数据源。

3. 将 `State/Province` 放在 `Detail` 货架上。

4. 复制 `http://neo.gsfc.nasa.gov/wms/wms` URL，然后在 Tableau 中导航到 `Map` | `Background Maps` | `Add WMS Map`。粘贴 URL 并点击 `OK`：

![](Images/B18435_09_43.png)

图 9.43：添加 WMS 地图

1. 在 `Marks` 卡中，将 `View` 类型设置为 `Map`。

2. 点击 `Color` 货架以开启 `Border`，将颜色设为白色，并将 `Opacity` 设为 `0%`：

![](Images/B18435_09_44.png)

图 9.44：活跃火灾地图
