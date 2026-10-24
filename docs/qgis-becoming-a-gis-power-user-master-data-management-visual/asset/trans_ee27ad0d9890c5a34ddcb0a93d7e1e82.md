

## 常见数据预处理步骤

### 如何操作...

此插件按文件夹分配位置，因此文件夹中的所有照片将获得相同的坐标。这非常适合批量分配通用坐标：

1. 首先加载你的 GPS 日志，或创建一个新的矢量图层并数字化你想要分配给照片的点。在本例中，加载 `centerofcalifornia.shp`。

![](img/2e88bf5938bf48a409c0a534a8775b21_532_0.png)

> 没有很多位置或只是手写了坐标？ 你可以在没有现有图层的情况下手动将信息输入到插件界面。

2. （可选）加载一个背景参考图层。

3. 现在你有了要与照片关联的图层，在 **Vector | Geotag and import photos | Geotag photos** 中打开插件。

4. 选择图层，然后选择要用作标签的字段（位置）。

5. 现在，点击 **Populate Table**。 界面中应该已添加一行。

6. 现在，通过点击要分配的行中 **Path to Folder** 下方的空白框来选择照片：

   1. 你可以输入路径或点击 **...** 浏览。
   2. 选择 `geotag` 文件夹。
   3. 确保在返回主屏幕后按 **Enter** 或点击框外：

![](img/2e88bf5938bf48a409c0a534a8775b21_533_0.png)

7. （可选）如果希望地理标记的版本是文件的全新副本而不是修改原始文件，请勾选重命名地理标记文件的复选框（务必保留原始备份）。

8. 点击确定。

### 其工作原理...

ExifTool 将写入图像文件的内置元数据中的一个部分，称为 EXIF。 在摄影中，将额外数据存储在照片中是标准做法，许多软件管理工具可以轻松读取。 WGS 84 坐标中的纬度和经度是 EXIF 部分中编码 GPS 数据的标准方法。

### 还有更多...

现在你有了地理标记的照片，你可以将其上传到 Flickr 等网站，该网站会将其显示在地图上，或者跳到第 4 章《数据探索》中的食谱《Viewing Geotagged Photos》，了解如何在 QGIS 中制作地图的方法。

此插件非常手动，并且按文件夹分配位置，因为它是为了专门与相机陷阱一起使用而创建的。 相反，如果你在每张照片位置之间移动并且有 GPS 日志，还有其他非 QGIS 工具可以帮助你将 GPS 点与照片匹配。 Digikam（摄影管理工具）具有根据时间戳匹配进行地理标记的功能。

### 另请参阅

- OpenStreetMap wiki 在 http://wiki.openstreetmap.org/wiki/Geotagging_Source_Photos 列出了其他免费和付费选项

## 4 数据探索

在本章中，我们将介绍以下食谱：

- 列出列中的唯一值
- 探索列中数值分布
- 使用 Time Manager 探索时空数据
- 使用 Time Manager 创建动画
- 设计时间依赖样式
- 使用 QuickMapServices 插件加载底图
- 使用 OpenLayers 插件加载底图
- 查看地理标记照片

### 引言

本章重点介绍食谱，这些食谱将帮助你直观检查并更好地理解你的数据。 本章中的食谱包括基于空间和时间属性对数据进行汇总、检查、过滤和样式化的方法，以便在执行分析之前让你更好地了解数据。 主要目标是创建一些数据可视化或摘要，让你作为人类能够利用大脑识别感兴趣模式的能力。 你对数据的理解越好，后续选择合适的分析方法就越容易。

### 列出列中的唯一值

在调查新数据集时，能够快速检查列包含哪些值非常有帮助。 在本食谱中，我们将使用 GUI 和 Python 控制台的两种不同方法来列出我们示例 POI 数据集中 POI 类别的唯一值。

#### 准备工作

要遵循本食谱，请加载 poi_names_wake.shp。

#### 如何操作...

如果你只是寻找基于 GUI 的解决方案，List unique values 工具同时可在 Vector | Analysis Tools 以及 Processing Toolbox 菜单中使用。 你可以使用其中任何一个来获取列中唯一值的列表。 该工具在 Processing Toolbox 菜单中可用，这使得它可以包含在处理模型中，从而实现流程自动化。 以下列出唯一值的步骤使用 Processing Toolbox 菜单：

1. 从 Processing Toolbox 菜单启动 List unique values。
2. 选择 poi_names_wake 图层作为输入图层，并将 class 属性作为目标字段。
3. 点击运行并等待工具完成。 结果将显示在结果视图中，结果视图将自动打开。

如果你想进一步自定义此任务，例如统计该数据集中值的出现次数，是时候启动 Python 控制台了：

1. 启动 Python Console，你可以在插件菜单中找到它，并点击显示编辑器按钮（在 Python 控制台左侧的工具栏中）打开编辑器窗口。
2. 将以下简短脚本粘贴到编辑器中，保存它，然后点击运行脚本按钮。 （确保在图层列表中选择了 POI 图层。）它遍历活动图层中的所有要素并创建一个字典对象，其中包含所有唯一值和相应的计数：

```python
import processing
layer = iface.activeLayer()
classes = {}
features = processing.features(layer)
for f in features:
    attrs = f.attributes()
    class_value = f['class']
    if class_value in classes:
        classes[class_value] += 1
    else:
        classes[class_value] = 1
print classes
```

以下屏幕截图显示了屏幕的样子：

![](img/2e88bf5938bf48a409c0a534a8775b21_537_0.png)

#### 其工作原理...

在第一行，我们使用 `import processing`，因为它提供了一个非常方便且实用的函数 `processing.features()` 来访问图层要素，我们在第 7 行使用它。 值得注意的是，如果存在选择，`processing.features()` 只会返回所选要素的迭代器。

第 3 行获取当前活动图层对象。 第 5 行创建空的字典对象，我们将用唯一值和相应计数填充它。

从第 5 行开始的 `for` 循环遍历图层的所有要素。 对于每个要素，我们获取分类字段中的值（第 7 行）。 你可以更改列名以分析其他列。 然后，我们只需要检查该值是否已存在于 `classes` 字典中（第 8 行），还是需要添加它（第 11 行）。

#### 还有更多...

如果你使用的是 SQL 数据库，例如 Spatialite 或 PostGIS，你可以使用 COUNT 和 GROUP BY 函数的查询来实现类似结果，以按类别统计要素或记录的数量：

```sql
SELECT class, COUNT(*)
FROM poi_names_wake
GROUP BY class
```

### 探索列中数值的分布

在本食谱中，我们将了解如何探索数值列的属性。 我们将查看 QGIS 提供的工具并将其应用于分析我们示例 POI 数据集中的高程值。

#### 准备工作

要遵循本食谱，请加载 `poi_names_wake.shp`。 如果你遵循了之前的食谱 *Listing unique values in a column*，你可以直接继续。

#### 如何操作...

了解数值列属性的第一印象的好方法是使用 **Vector** 中的 **Basic Statistics** 工具。 这允许你计算统计值，例如最小值和最大值、平均值和中位数、标准差和总和。

如果你想检查高程值的分布，有方便的 Statist 插件。 Statist 生成值分布的交互式直方图表示：

1. 使用 **Plugin Manager** 安装 **Statist**。
2. 从 **Vector** 菜单启动 **Statist**。
3. 指定 **Input vector layer** 和你想要分析的属性（**Target field**），然后点击 **OK** 计算统计信息。
4. 使用图表下方的按钮，你可以缩放和平移图表，以及保存图表图像。
5. 你甚至可以通过更改标题和轴标签及范围来自定义图表。 只需使用最右边的绿色勾号按钮打开自定义对话框即可：

