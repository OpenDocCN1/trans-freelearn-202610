```
QObject.disconnect(canvas,SIGNAL("mapToolSet (QgsMapTool *)")
    ),self.map_tool_changed)
canvas.unsetMapTool(self.map_tool)
print "restore prev tool %s" %(self.prev_tool)
canvas.setMapTool(self.prev_tool)
```

Our new custom do_something() function is called when our map tool is used to successfully identify a feature. For this example, we simply print the feature's attributes on the Python Console. Of course, you can get creative here and add your desired custom functionality:

```
def do_something(self, layer, feature):
    print feature.attributes()
```

Finally, we also have to handle the case when the user switches to a different map tool. This is similar to the case of the user deactivating our tool in the map_tool_init() function:

```
def map_tool_changed(self):
    print "maptoolchanged"
    canvas = self.iface.mapCanvas()
    QObject.disconnect(canvas,SIGNAL("mapToolSet (QgsMapTool *)"),
        self.map_tool_changed)
    canvas.unsetMapTool(self.map_tool)
    self.map_tool_action.setChecked(False)
```

You also have to add the following import line at the top of the script to avoid errors concerning QObject, QgsMapTool, and others:

```
from qgis.core import *
from qgis.gui import *
from PyQt4.QtCore import *
```

When you are ready, you can reload the plugin and try it. You should have the Python Console open to be able to follow the plugin's outputs. The first thing you will see when you activate the plugin in the toolbar is that it prints maptoolinit on the console. Then, if you click on the map, it will print canvasReleaseEvent, and if you click on a feature, it will also display the feature's attributes. Finally, if you change to another map tool (for example, the Pan Map tool) it will print maptoolchanged on the console and the icon in the plugin toolbar will be unchecked.