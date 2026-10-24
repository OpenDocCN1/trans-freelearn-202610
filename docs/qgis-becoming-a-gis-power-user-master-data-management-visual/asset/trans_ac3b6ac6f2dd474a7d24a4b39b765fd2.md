

# 创建内存图层

创建临时矢量图层的一种好方法是使用所谓的**内存图层**。内存图层是临时分析输出或可视化的一个不错选择。它们是临时草图图层的脚本等价物，我们在 *第 3 章，数据创建与编辑* 中使用过它。与临时草图图层一样，内存图层存在于 QGIS 会话中，并在关闭 QGIS 时被销毁。在下面的示例中，我们创建一个内存图层并向其中添加一个面要素。

基本上，内存图层是一个 `QgsVectorLayer`，与其他图层一样。然而，提供者（第三个参数）不是上一个加载文件示例中的 'ogr'，而是 'memory'。第一个参数不是文件路径，而是一个定义字符串，指定几何类型、CRS 以及属性表字段（本例中为一个名为 `MYNUM` 的整数字段和一个名为 `MYTXT` 的字符串字段）：

```
mem_layer =
QgsVectorLayer("polygon?crs=epsg:4326&field=MYNUM:integer&field=MYTXT:string", "temp_layer", "memory")
if not mem_layer.isValid():
    raise Exception("Failed to create memory layer")
```

一旦我们创建了 `QgsVectorLayer` 对象，我们就可以开始向其数据提供者添加要素：

```
mem_layer_provider = mem_layer.dataProvider()
my_polygon = QgsFeature()
my_polygon.setGeometry(
    QgsGeometry.fromRect(QgsRectangle(16,48,17,49)))
my_polygon.setAttributes([10,"hello world"])
mem_layer_provider.addFeatures([my_polygon])
QgsMapLayerRegistry.instance().addMapLayer(mem_layer)
```

> 注意我们首先创建一个空白的 `QgsFeature`，然后分别使用 `setGeometry()` 和 `setAttributes()` 为其添加几何和属性。当我们将图层添加到 `QgsMapLayerRegistry` 时，图层会在地图上渲染。

# 导出地图图像

保存当前地图的最简单选项是使用与项目下“另存为图像”相同的脚本等价物。这将以 QGIS 应用程序窗口中地图区域相同的分辨率将当前地图导出为图像文件：

```
iface.mapCanvas().saveAsImage('C:/temp/simple_export.png')
```

如果我们想要对导出的图像的尺寸和分辨率有更多控制，则需要几行额外的代码。下面的示例展示了如何创建自己的 QgsMapRendererCustomPainterJob 对象，并使用自定义的 QgsMapSettings 配置尺寸（宽度和高度）、分辨率（dpi）、地图范围和地图图层：

```
from PyQt4.QtGui import QImage, QPainter
from PyQt4.QtCore import QSize
# configure the output image
width = 800
height = 600
dpi = 92
img = QImage(QSize(width, height), QImage.Format_RGB32)
img.setDotsPerMeterX(dpi / 25.4 * 1000)
img.setDotsPerMeterY(dpi / 25.4 * 1000)
# get the map layers and extent
layers = [ layer.id() for layer in iface.legendInterface().layers() ]
extent = iface.mapCanvas().extent()
# configure map settings for export
mapSettings = QgsMapSettings()
mapSettings.setMapUnits(0)
mapSettings.setExtent(extent)
mapSettings.setOutputDpi(dpi)
mapSettings.setOutputSize(QSize(width, height))
mapSettings.setLayers(layers)
mapSettings.setFlags(QgsMapSettings.Antialiasing | QgsMapSettings.UseAdvancedEffects | QgsMapSettings.ForceVectorOutput | QgsMapSettings.DrawLabeling)
# configure and run painter
p = QPainter()
p.begin(img)
mapRenderer = QgsMapRendererCustomPainterJob(mapSettings, p)
mapRenderer.start()
mapRenderer.waitForFinished()
p.end()
# save the result
img.save("C:/temp/custom_export.png","png")
```

## 使用 Python 创建自定义地理处理脚本

在 *第 4 章，空间分析* 中，我们使用 **Processing Toolbox** 的工具来分析数据，但我们并不局限于这些工具。我们可以用自己的脚本扩展 *processing*。*processing* 脚本相对于上一节中看到的普通 Python 脚本的优势如下：

- Processing 会自动为脚本生成图形用户界面以配置脚本参数
- Processing 脚本可用于 **Graphical modeler** 以创建地理处理模型

如下截图所示，**Scripts** 部分最初是空的，除了一些用于添加和创建新脚本的 **Tools**：

![](img/2e88bf5938bf48a409c0a534a8775b21_189_0.png)

### 编写你的第一个 Processing 脚本

我们将创建第一个简单的脚本；它获取一些图层信息。开始时，双击 Scripts | Tools 中的 Create new script 条目。这将打开一个空的 Script editor 对话框。下面的截图显示了 Script editor，其中包含一个简短的脚本，该脚本会在 Python Console 上打印输入图层的名称：

![](img/2e88bf5938bf48a409c0a534a8775b21_190_0.png)

第一行表示我们的脚本将被放入 Learning QGIS 脚本组中，如下截图所示。双哈希（##）是 Processing 语法，表示该行包含 Processing 特定信息而不是 Python 代码。脚本名称由保存脚本时选择的文件名创建。对于本示例，我将脚本保存为 my_first_script.py。第二行定义了脚本输入，本例中为矢量图层。在下一行，我们使用 Processing 的 getObject() 函数来访问输入图层对象，最后图层名称会打印在 Python Console 上。

你可以直接从编辑器中通过点击 Run algorithm 按钮运行脚本，或通过在 Processing Toolbox 中双击条目运行。如果想更改代码，请使用条目上下文菜单中的 Edit script，如此截图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_190_1.png)

> 学习如何为 Processing 编写自定义脚本的一个好方法是查看现有脚本，例如 https://github.com/qgis/QGIS-Processing/tree/master/scripts。这是官方脚本仓库，你也可以使用 Processing Toolbox 中的内置 **Get scripts from on-line scripts collection** 工具下载脚本。

### 编写具有矢量图层输出的脚本

当然，在大多数情况下，我们不想只在 **Python Console** 上输出内容。因此，下面的示例展示了如何创建矢量图层。更具体地说，脚本会在输入图层中的点周围创建正方形面要素。数值 `size` 输入参数控制输出矢量图层中正方形的大小。自动生成的对话框中显示的默认大小设置为 `1000000`：

```
##Learning QGIS=group
##input_layer=vector
##size=number 1000000
##squares=output vector
from qgis.core import *
from processing.tools.vector import VectorWriter
# get the input layer and its fields
my_layer = processing.getObject(input_layer)
fields = my_layer.dataProvider().fields()
# create the output vector writer with the same fields
writer = VectorWriter(squares, None, fields, QGis.WKBPolygon,
my_layer.crs())
# create output features
feat = QgsFeature()
for input_feature in my_layer.getFeatures():
    # copy attributes from the input point feature
    attributes = input_feature.attributes()
    feat.setAttributes(attributes)
    # create square polygons
    point = input_feature.geometry().asPoint()
    xmin = point.x() - size/2
    ymin = point.y() - size/2
    square = QgsRectangle(xmin,ymin,xmin+size,ymin+size)
    feat.setGeometry(QgsGeometry.fromRect(square))
    writer.addFeature(feat)
del writer
```

在此脚本中，我们使用 VectorWriter 来写入输出矢量图层。创建 VectorWriter 对象的参数为 fileName、encoding、fields、geometryType 和 crs。

> 可用的几何类型为 QGIS.WKBPoint、QGIS.WKBLineString、QGIS.WKBPolygon、QGIS.WKBMultiPoint、QGIS.WKBMultiLineString 和 QGIS.WKBMultiPolygon。你也可以在 Python Console 中输入 VectorWriter.TYPE_MAP 来获取此几何类型列表。

注意我们使用输入图层的字段（my_layer.dataProvider().fields()）来创建 VectorWriter。这确保输出图层具有与输入图层相同的字段（属性表列）。同样，对于输入图层中的每个要素，我们将其属性值（input_feature.attributes()）复制到对应的输出要素中。

运行脚本后，结果图层将被加载到 QGIS 中，并使用输出参数名称列出；本例中图层名为 squares。下面的截图显示了自动生成的输入对话框以及将脚本应用于样本数据集中机场时的输出：

![](img/2e88bf5938bf48a409c0a534a8775b21_192_0.png)

