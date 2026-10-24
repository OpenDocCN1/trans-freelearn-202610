

# 创建与地役权网格的邻近关系

执行以下步骤：

+   1. 导航到 **Processing Toolbox**。
+   2. 在此工具箱中搜索 `proximity`。确保已选择 **Advanced Interface**。
+   3. 找到 **SAGA** 下的 **Proximity** 网格工具后，双击运行它。
+   4. 为 **Features** 字段选择 **easements**。
+   5. 为 **Distance** 指定输出文件为 `c2/data/output/easements_prox.tif`。
+   6. 对其他两个输出取消选中 **Open output file after running algorithm**，如下图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_254_0.png)

生成的网格为到最近地役权单元的距离。

4. 将输出保存在 c2/data/output 中。你可以保持其他输入为默认值。

![](img/2e88bf5938bf48a409c0a534a8775b21_256_0.png)

5. 输出将是每个单元的坡度，以垂直高程相对于水平距离的百分比表示（“上升/运行”）。

![](img/2e88bf5938bf48a409c0a534a8775b21_256_1.png)

# 使用 Map Calculator 组合标准

1. 确保所有标准网格（proximity、agriculture、developed、road 和 slope）都出现在图层面板中。如果没有，请添加它们。

+   2. 打开栅格计算器对话框。
+     1. 导航到 Raster | Raster calculator

+   3. 输入地图代数表达式。

+   - 通过在栅格波段选择区域中双击它们来添加栅格图层
+   - 通过键入或点击运算符区域中的按钮来添加运算符
+   - 输入的表达式应如下所示：

```
("slope@1" < 8) + ("applicants@1" = 1) +
  ("easement_prox@1"<2000) + ("roads_prox@1">100) +
  ("developed_prox@1" > 500) + ("agriculture@1" < 100)
```

> [ 💡 @1 指栅格的第一个也是唯一一个波段。 ]

![](img/2e88bf5938bf48a409c0a534a8775b21_257_0.png)

+   4. 为输出文件添加名称和路径并按 *Enter*。
+   5. 如果似乎什么都没发生，你可能需要设置样式。默认情况下，非零值设置为白色显示（与我们的背景颜色相同）。

![](img/2e88bf5938bf48a409c0a534a8775b21_258_0.png)

以下是前面地图图像的特写，以便你可以看到适宜性变化：

![](img/2e88bf5938bf48a409c0a534a8775b21_259_0.png)

在前面的屏幕截图中，单元的评分如下：

+   - Green = 5（高）
+   - Yellow = 4（中）
+   - Red = 3（低）

+   4. 创建一个 `rank` 字段，根据区域统计步骤创建的 `_mean` 字段手动编辑每个值。这是每个单元平均适宜性的度量。我们将使用此字段作为标签，向普通受众传达相对适宜性；因此，我们需要排名而不是粗略的平均值。

+   5. 现在，标记图层。

+   1. 在 **Layer Properties** 下，激活 **Labels** 选项卡。
+   2. 选择 `rank` 字段作为要标记的字段。
+   3. 添加其他格式，例如标签放置和缓冲区（光晕），使用标签选项卡对话框内的内部选项卡，如下图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_261_0.png)

# 识别最佳地点

6. 为图层添加样式。

+   1. 选择 **Graduated** 样式。
+   2. 选择合适的颜色渐变、分类数量和分类类型。
+   3. 点击 **Classify** 按钮，如下图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_262_0.png)

完成这些步骤后，你的地图将看起来类似于以下内容：

![](img/2e88bf5938bf48a409c0a534a8775b21_263_0.png)

# 将结果作为 Web 应用程序发布

现在我们已经完成了农田保护选址的建模，让我们采取步骤将其发布到 Web。

# qgis2leaf

qgis2leaf 允许我们使用 Leaflet 地图 API 将 QGIS 地图导出为 Web 地图格式（JavaScript、HTML 和 CSS）。Leaflet 是一个非常轻量、可扩展且响应迅速（且时尚）的 Web 映射界面。

qgis2leaf 将我们所有的矢量图层转换为 GeoJSON，这是表达地理 JavaScript 对象的最常见文本方式。由于我们的操作图层位于 GeoJSON 中，Leaflet 的点击交互得到支持，我们可以通过点击访问图层中的信息。它是一个完全可编辑的 HTML 和 JavaScript 文件。你可以自定义并上传到可访问的 Web 位置，如后续章节中所述。

只要到目前为止图层已正确准备（例如关于 CRS），qgis2leaf 就非常简单易用。它在为我们的 Leaflet Web 地图创建良好的起始应用程序，包括 GeoJSON、HTML 和 JavaScript 方面也非常强大。执行以下步骤：

+   1. 如果尚未安装，请确保安装 qgis2leaf 插件。
+   2. 导航到 Web | qgis2leaf | Exports a QGIS Project to a working Leaflet webmap。
+   3. 点击 Get Layers 按钮，将当前显示的图层添加到 qgis2leaf 将导出的集合中。

+   4. 选择底图并在需要时输入其他详细信息。
+   5. 选择 **Encode to JSON**。

![](img/2e88bf5938bf48a409c0a534a8775b21_265_0.png)

# 识别最佳地点

这些步骤将生成一个类似于以下内容的地图应用程序。我们将在下一章中查看如何恢复标签：

![](img/2e88bf5938bf48a409c0a534a8775b21_266_0.png)

# 总结

在本章中，使用选址示例，我们涵盖了基本矢量数据 ETL、栅格分析和 Web 地图创建。我们从矢量数据开始，在统一 CRS 后准备属性表。然后我们使用批量处理对其进行筛选并转换为栅格网格。我们还在应用邻近和地形分析时考虑了一些基本的栅格概念。通过地图代数，我们组合了这些结果用于加性建模选址。我们准备了结果，这需要转换为矢量、样式化和标记。最后，我们使用 qgis2leaf 将准备好的矢量输出发布为简单的 Leaflet Web 地图应用程序，为扩展奠定了坚实的基础。在下一章中，你将通过水文建模示例了解更多关于栅格分析和 Web 应用程序发布的内容。

# 3
## 发现物理
## 关系

在本章中，我们将为栅格物理建模示例创建一个应用程序。首先，我们将使用栅格分析对一些基本水文分析的物理条件进行建模。接下来，我们将使用模型自动化工具重做这些步骤。然后，我们将栅格值附加到矢量对象，以便在 Web 应用程序中高效查找。最后，我们将使用云平台实现从客户端应用程序代码进行动态查询。我们将查看一个环境规划案例，提供能力让利益相关者发现上游有毒地点。

在本章中，我们将涵盖以下主题：

+   - Hydrological modeling
+   - Workflow automation with graphical models
+   - Spatial relationships for a performant access to information
+   - The NNJoin plugin
+   - The CartoDB cloud platform
+   - Leaflet SQLQueries using an external API:CartoDB

此功能在某些版本的 Mac OS QGIS 中不可用。

**Clip by mask layer** 工具的输出，显示裁剪到县多边形的网格，将看起来类似于以下图像（黑白颜色渐变或映射到空值的映射可能被反转）：

![](img/2e88bf5938bf48a409c0a534a8775b21_271_0.png)

# 基于高程建模水文网络

现在我们的高程网格已经准备好，是时候真正建模水文网络位置和方向了。为此，我们将使用 **Channel network and drainage basins**，它只需要一个输入：已填充和裁剪的高程模型。该工具将使用 Strahler Order 阈值生成水文线，这与返回的河流的分层级别有关（例如，排除非常小的沟渠）。默认值为 5，对我们的目的来说是完美的，包括足够的水文线但不过多。结果看起来非常逼真。该工具还生成许多其他相关的网格，我们在本项目中不需要。执行以下步骤：

+   1. 导航到 **Processing Toolbox (Advanced Interface)**。
+   2. 搜索 **Channel network and drainage basins**（在 **SAGA | Terrain Analysis | Hydrology** 下）。
+   3. 运行 **Channel network and drainage basins** 工具。
+   4. 在 **Elevation** 字段中，输入已填充和裁剪的 DEM，即上一节中的输出。
+   5. 在 **Threshold** 字段中，保持默认值（5.0）。
+   6. 在 **Channels** 字段中，输入 `c3/data/output/channels.shp`。确保选中 **Open output file after running algorithm**
+   7. 对所有其他输出取消选择 **Open output file after running algorithm**。

