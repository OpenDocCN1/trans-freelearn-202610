> A good way of learning how to write custom scripts for Processing is to take a look at existing scripts, for example, at https://github.com/qgis/QGIS-Processing/tree/master/scripts. This is the official script repository, where you can also download scripts using the built-in **Get scripts from on-line scripts collection** tool in the **Processing Toolbox**.

# Writing a script with vector layer output

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