

+   6. 添加一条名为 75% Percentile: 的 **Percentile line**，并将 **Percentile** 设置为 75%。
+   7. 对所有线条，切换 **Data label** 为 **On** 并将 **Style** 设置为 **Both**。
+   8. 添加一个 **Forecast** 为 **1 Month(s)**，**Confidence interval** 为 75% 且 **Seasonality** 值为 **60** 个点的预测。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_385_0.png)

*Figure 9.4: Line chart with analytics lines and forecast*

在此示例中，40% 被视为关键盈利阈值，因此一条常数线有助于突出低于此水平的值。此外，设置为 75% 的百分位线有助于识别最高四分位数值。来自 **Analytics** 画布的线条及其格式可在不为模型添加额外的 DAX 度量或使视觉对象杂乱的情况下为用户提供更多分析价值。

**Confidence interval** 和 **Seasonality** 设置取决于源数据，可能需要一些试验才能获得最佳结果。

## See also

- *Use the Analytics pane in Power BI Desktop:* http://bit.ly/2s2fA0P
- *How to trace a Power BI Desktop file:* http://bit.ly/2tYRLZg

## Enriching Content with Custom Visuals and Quick Insights

Power BI 的自定义视觉对象可以从 AppSource 中查看和下载，以提供超出 Power BI Desktop 标准视觉对象支持的额外功能和选项。目前 AppSource 中已有超过 300 个自定义视觉对象，其中许多由 Microsoft 开发以满足常见需求，例如子弹图、直方图和甘特图。AppSource 中其他由第三方开发但经 Microsoft 安全验证的自定义视觉对象具有独特且强大的功能，例如流图网络可视化和 ZoomCharts 开发的交互式视觉对象。除了自定义视觉对象外，Quick Insights 还可用于 Power BI 服务和 Power BI Desktop，对语义模型应用高级分析算法以提取见解，例如趋势或关系，并快速生成用于报表和仪表板的新可视化。

2. 在右上角使用 Search 查找已认证的 Bullet Chart 视觉对象（蓝色星标/勾选图标），单击 Bullet Chart 视觉对象，然后单击 Add 按钮。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_387_0.png)

Figure 9.6: Adding a custom visual from AppSource

3. 单击 OK 按钮。

4. 创建以下度量值：

```
Sales 2011 = SUMX(FILTER('Internet Sales',YEAR([OrderDate])=2011),[SalesAmount])
Sales 2012 = SUMX(FILTER('Internet Sales',YEAR([OrderDate])=2012),[SalesAmount])
Sales 2012 Improve = [Sales 2011] * .7
Sales 2012 Sat = [Sales 2011] * .8
Sales 2012 Good = [Sales 2011] * .9
Sales 2012 VG = [Sales 2011]
```

5. 在 Visualizations 画布中选择 Bullet Chart 图标，将其添加到报表画布。

6. 按如下方式配置视觉对象：

- Category: SalesTerritoryCountry 列
- Value: Sales 2012 度量值
- Needs Improvement: Sales 2012 Improve 度量值
- Satisfactory: Sales 2012 Sat 度量值
- Good: Sales 2012 Good 度量值
- Very Good: Sales 2012 VG 度量值

右键单击特定数据点时会出现 **Analyze** 选项，可为所选项目生成额外的可视化，例如折线图上的日期或条形图上的维度值。生成的视觉对象随后可以添加到 Power BI Desktop 文件中并像其他所有视觉对象一样进行编辑。**Analyze** 选项是 Power BI Desktop 中 **Quick Insights** 功能和分析引擎的体现。

**Quick Insights** 也可以针对 Power BI 服务中的整个语义模型执行。要对语义模型运行 Quick Insights，请单击给定语义模型的省略号并选择 **Get quick insights**。生成的见解可通过同一上下文菜单中的 **View insights** 选项访问。每个见解都包含一个 Power BI 视觉对象、所应用的见解标题（算法），例如异常值和相关性，以及简短描述。来自 **View insights** 的视觉对象也可以固定到新的和现有的仪表板。

**Quick Insights** 对语义模型应用复杂的算法，包括类别异常值、相关性、时间序列中的变化点、低方差、大多数、时间序列中的季节性以及时间序列中的总体趋势。见解引擎在渲染结果时受限于设定的时间。

**Quick Insights** 视觉对象可以像其他 Power BI 报表和仪表板图块一样固定到新的和现有的仪表板。此外，**Quick Insights** 可以针对之前由 **Quick Insights** 生成的视觉对象执行。

通过隐藏或取消隐藏列可以改进 **Quick Insights** 的结果。**Quick Insights** 不会搜索隐藏的列，因此隐藏（或删除）不必要的列可以使见解算法仅关注重要列。同样，任何重复的列都可以删除或隐藏，以便为 **Quick Insights** 运行分配的时间得到高效利用。

## See also

- *Create custom data visualizations for Power BI:* https://bit.ly/3xY9GAk
- *Get your Power BI visual certified:* https://bit.ly/2RETFPa
- *Generate data insights on your semantic model automatically with Power BI:* https://bit.ly/33qJErT
- *Optimize your data for Power BI Quick Insights:* https://bit.ly/3trWOPz

## Creating Geospatial Mapping with ArcGIS Maps

来自 **Environmental Systems Research Institute (Esri)** 的 ArcGIS 映射和空间分析软件，Esri 是地理信息系统 (**GISes**) 的市场领导者，已内置于 Power BI Desktop 中，以从数据的空间组件生成见解。熟悉的报表可视化字段井和 Power BI 的交叉筛选功能可以与 ArcGIS 地理空间功能和语义模型相结合，例如分类类型、图钉和参考图层，以将自定义、智能的地理可视化构建到 Power BI 解决方案中。

在本食谱中，创建一个自定义地理列以包含多个地理属性（即街道地址、城市和州）以支持 ArcGIS 服务的准确地理编码。然后使用 Power BI Desktop 中的 ArcGIS 视觉对象将客户地址绘制到 **Cluster** 主题地图可视化中，并辅以 **Pins** 和 **Infographics**。

### Getting ready

为准备本食谱，请执行以下步骤：

- 1. 在 Power BI 服务中，单击右上角的 Settings（齿轮图标），选择 General，单击 Item settings，并启用 ArcGIS maps。这需要对 Power BI 服务具有管理员访问权限。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_391_0.png)

- 2. 从以下 GitHub 仓库下载 Creating Geospatial Mapping with ArcGIS Maps Start.pbix：https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition。
- 3. 在 Power BI Desktop 中打开 Creating Geospatial Mapping with ArcGIS Maps Start.pbix。
- 4. 编辑 AdventureWorks 查询，并将 Source 行更改为指向安装了 AdventureWorksDW2022 数据库的 SQL 服务器。
- 5. 从 Home 选项卡的 ribbon 中单击 Close & Apply。

### How to create geospatial mapping with ArcGIS maps

要实施本食谱，请执行以下步骤：

- 1. 在 Report view 中，选择 Customers 表中的 City 列并分配 City 的 Data category。
- 2. 选择 Customers 表中的 Full Address 列并分配 Address 的 Data category。
- 3. 使用 Customers 表中的 SalesTerritoryRegion 列应用页面级筛选器，并仅选择 Southwest。
- 4. 在 Visualizations 画布中选择 ArcGIS Maps for Power BI 视觉对象，然后将视觉对象的大小扩展到整个页面。
- 5. 在视觉对象中单击 Continue as guest 按钮。
- 6. 从 Customers 表中，为 Location 添加 Full Address 列，为 Size 添加 CustomerKey 列，为 Color 添加 YearlyIncome 列。

