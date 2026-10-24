> You can create planes.qml by saving the airport style you created in Chapter 2, Viewing Spatial Data (by going to Layer Properties | Style | Save Style | QGIS Layer Style File), or use any other style you like.

Of course, we can also create a style in code. Let's take a look at a basic single symbol renderer. We create a simple symbol with one layer, for example, a yellow diamond:

```
from PyQt4.QtGui import QColor
symbol = QgsMarkerSymbolV2()
symbol.symbolLayer(0).setName('diamond')
symbol.symbolLayer(0).setSize(10)
symbol.symbolLayer(0).setColor(QColor('#ffff00'))
v_layer.rendererV2().setSymbol(symbol)
v_layer.triggerRepaint()
```

A much more advanced approach is to create a rule-based renderer. We discussed the basics of rule-based renderers in Chapter 5, Creating Great Maps. The following example creates two rules: one for civil-use airports and one for all other airports. Due to the length of this script, I recommend that you use the Python Console editor, which you can open by clicking on the Show editor button, as shown in the following screenshot:

![](bbox=[0.2, 0.49, 0.8, 0.75])