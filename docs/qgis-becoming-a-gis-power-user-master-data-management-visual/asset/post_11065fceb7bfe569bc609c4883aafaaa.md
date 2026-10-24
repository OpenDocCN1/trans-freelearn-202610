
# Creating a memory layer

A great way to create a temporary vector layer is by using so-called **memory layers**. Memory layers are a good option for temporary analysis output or visualizations. They are the scripting equivalent of temporary scratch layers, which we used in *Chapter 3, Data Creation and Editing*. Like temporary scratch layers, memory layers exist within a QGIS session and are destroyed when QGIS is closed. In the following example, we create a memory layer and add a polygon feature to it.

Basically, a memory layer is a `QgsVectorLayer` like any other. However, the provider (the third parameter) is not 'ogr' as in the previous example of loading a file, but 'memory'. Instead of a file path, the first parameter is a definition string that specifies the geometry type, the CRS, and the attribute table fields (in this case, one integer field called `MYNUM` and one string field called `MYTXT`):

```
mem_layer =
QgsVectorLayer("polygon?crs=epsg:4326&field=MYNUM:integer&field=MYTXT:string", "temp_layer", "memory")
if not mem_layer.isValid():
    raise Exception("Failed to create memory layer")
```

Once we have created the `QgsVectorLayer` object, we can start adding features to its data provider:

```
mem_layer_provider = mem_layer.dataProvider()
my_polygon = QgsFeature()
my_polygon.setGeometry(
    QgsGeometry.fromRect(QgsRectangle(16,48,17,49)))
my_polygon.setAttributes([10,"hello world"])
mem_layer_provider.addFeatures([my_polygon])
QgsMapLayerRegistry.instance().addMapLayer(mem_layer)
```

> Note how we first create a blank `QgsFeature`, to which we then add geometry and attributes using `setGeometry()` and `setAttributes()`, respectively. When we add the layer to `QgsMapLayerRegistry`, the layer is rendered on the map.

# Exporting map images

The simplest option for saving the current map is by using the scripting equivalent of Save as Image (under Project). This will export the current map to an image file in the same resolution as the map area in the QGIS application window:

```
iface.mapCanvas().saveAsImage('C:/temp/simple_export.png')
```

If we want more control over the size and resolution of the exported image, we need a few more lines of code. The following example shows how we can create our own QgsMapRendererCustomPainterJob object and configure to our own liking using custom QgsMapSettings for size (width and height), resolution (dpi), map extent, and map layers:

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

## Creating custom geoprocessing scripts using Python

In *Chapter 4, Spatial Analysis*, we used the tools of **Processing Toolbox** to analyze our data, but we are not limited to these tools. We can expand *processing* with our own scripts. The advantages of *processing* scripts over normal Python scripts, such as the ones we saw in the previous section, are as follows:

- Processing automatically generates a graphical user interface for the script to configure the script parameters
- Processing scripts can be used in **Graphical modeler** to create geoprocessing models

As the following screenshot shows, the **Scripts** section is initially empty, except for some **Tools** to add and create new scripts:

![](img/2e88bf5938bf48a409c0a534a8775b21_189_0.png)

### Writing your first Processing script

We will create our first simple script; which fetches some layer information. To get started, double-click on the Create new script entry in Scripts | Tools. This opens an empty Script editor dialog. The following screenshot shows the Script editor with a short script that prints the input layer's name on the Python Console:

![](img/2e88bf5938bf48a409c0a534a8775b21_190_0.png)

The first line means our script will be put into the Learning QGIS group of scripts, as shown in the following screenshot. The double hashes (##) are Processing syntax and they indicate that the line contains Processing-specific information rather than Python code. The script name is created from the filename you chose when you saved the script. For this example, I have saved the script as my_first_script.py. The second line defines the script input, a vector layer in this case. On the following line, we use Processing's getObject() function to get access to the input layer object, and finally the layer name is printed on the Python Console.

You can run the script either directly from within the editor by clicking on the Run algorithm button, or by double-clicking on the entry in the Processing Toolbox. If you want to change the code, use Edit script from the entry context menu, as shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_190_1.png)

> A good way of learning how to write custom scripts for Processing is to take a look at existing scripts, for example, at https://github.com/qgis/QGIS-Processing/tree/master/scripts. This is the official script repository, where you can also download scripts using the built-in **Get scripts from on-line scripts collection** tool in the **Processing Toolbox**.

### Writing a script with vector layer output

Of course, in most cases, we don't want to just output something on the **Python Console**. That is why the following example shows how to create a vector layer. More specifically, the script creates square polygons around the points in the input layer. The numeric `size` input parameter controls the size of the squares in the output vector layer. The default size that will be displayed in the automatically generated dialog is set to `1000000`:

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

In this script, we use a VectorWriter to write the output vector layer. The parameters for creating a VectorWriter object are fileName, encoding, fields, geometryType, and crs.

> The available geometry types are QGIS.WKBPoint, QGIS.WKBLineString, QGIS.WKBPolygon, QGIS.WKBMultiPoint, QGIS.WKBMultiLineString, and QGIS.WKBMultiPolygon. You can also get this list of geometry types by typing VectorWriter.TYPE_MAP in the Python Console.

Note how we use the fields of the input layer (my_layer.dataProvider().fields()) to create the VectorWriter. This ensures that the output layer has the same fields (attribute table columns) as the input layer. Similarly, for each feature in the input layer, we copy its attribute values (input_feature.attributes()) to the corresponding output feature.

After running the script, the resulting layer will be loaded into QGIS and listed using the output parameter name; in this case, the layer is called squares. The following screenshot shows the automatically generated input dialog as well as the output of the script when applied to the airports from our sample dataset:

![](img/2e88bf5938bf48a409c0a534a8775b21_192_0.png)
