
+   Another great resource to learn programming with QGIS is *QGIS Python Programming Cookbook, Joel Lawhead*, published by Packt Publishing

## Writing Processing algorithms

You can extend the capabilities of QGIS by adding scripts that can be used within the Processing framework. This will allow you to create your own analysis algorithms and then run them efficiently from the toolbox or from any of the productivity tools, such as the batch processing interface or the graphical modeler.

This recipe covers basic ideas about how to create a Processing algorithm.

## Getting ready

A basic knowledge of Python is needed to understand this recipe. Also, as it uses the Processing framework, you should be familiar with it before studying this recipe.

## How to do it...

We are going to add a new process to filter the polygons of a layer, generating a new layer that just contains the ones with an area larger than a given value. Here's how to do this:

-   1. In the **Processing Toolbox** menu, go to the **Scripts/Tools** group and double-click on the **Create new script** item. You will see the following dialog:

![](img/2e88bf5938bf48a409c0a534a8775b21_754_0.png)

2. In the text editor of the dialog, paste the following code:

```
##Cookbook=group
##Filter polygons by size=name
##Vector_layer=vector
##Area=number 1
##Output=output vector

layer = processing.getObject(Vector_layer)
provider = layer.dataProvider()
writer = processing.VectorWriter(Output, None,
    provider.fields(), provider.geometryType(), layer.crs())
for feature in processing.features(layer):
    print feature.geometry().area()
    if feature.geometry().area() > Area:
        writer.addFeature(feature)
del writer
```

3. Select the Save button to save the script. In the file selector that will appear, enter a filename with the .py extension. Do not move this to a different folder. Make sure that you use the default folder that is selected when the file selector is opened.

4. Close the editor.

5. Go to the Scripts groups in the toolbox, and you will see a new group called Cookbook with an algorithm called Filter polygons by size.

6. Double-click on it to open it, and you will see the following parameters dialog, similar to what you can find for any of the other Processing algorithms:

![](img/2e88bf5938bf48a409c0a534a8775b21_755_0.png)

## How it works...

The script contains mainly two parts:

-   A part in which the characteristics of the algorithm are defined. This is used to define the semantics of the algorithm, along with some additional information, such as the name and group of the algorithm.
-   A part that takes the inputs entered by the user and processes them to generate the outputs. This is where the algorithm itself is located.

In our example, the first part looks like the following:

```
##Cookbook=group
##Filter polygons by size=name
##Vector_layer=vector
##Area=number 1
##Output=output vector
```

We are defining two inputs (the layer and the area value) and declaring one output (the filtered layer). These elements are defined using the Python comments with a double Python comment sign (#).

The second part includes the code itself and looks like the following:

```
layer = processing.getObject(Vector_layer)
provider = layer.dataProvider()
writer = processing.VectorWriter(Output, None,
    provider.fields(), provider.geometryType(), layer.crs())
for feature in processing.features(layer):
    print feature.geometry().area()
    if feature.geometry().area() > Area:
        writer.addFeature(feature)
del writer
```

The inputs that we defined in the first part will be available here, and we can use them. In the case of the area, we will have a variable named Area, containing a number. In the case of the vector layer, we will have a Layer variable, containing a string with the source of the selected layer.

Using these values, we use the PyQGIS API to perform the calculations and create a new layer. The layer is saved in the file path contained in the Output variable, which is the one that the user will select when running the algorithm.

Apart from using regular Python and the PyQGIS interface, Processing includes some classes and functions because this makes it easier to create scripts, and that wrap some of the most common functionality of QGIS.

In particular, the `processing.features(layer)` method is important. This provides an iterator over the features in a layer, but only considering the selected ones. If no selection exists, it iterates over all the features in the layer. This is the expected behavior of any Processing algorithm, so this method has to be used to provide a consistent behavior in your script.

## There's more...

Some of the core algorithms that are provided with Processing are actually scripts, such as the one we just created, but they do not appear in the scripts section. Instead, they appear in the **QGIS algorithms** section because they are a core part of Processing.

Other scripts are not part of processing itself but they can be installed easily from the toolbox using the **Tools/Get scripts from on-line collection** menu:

You will see a window like the following one:

![](img/2e88bf5938bf48a409c0a534a8775b21_757_0.png)

Just select the scripts that you want to install and then click on **OK**. The selected scripts will now appear in the toolbox. You can use it as you use any other Processing algorithm.

## Writing QGIS plugins

One of the main reasons of the popularity of QGIS is its extensibility. Using the basic tools and features provided by the QGIS API, new functionality can be implemented and added as a new plugin that can be shared by contributing it to the QGIS plugins repository.

## See also

-   - All the information about creating scripts and running Processing code from the QGIS Python console can be found in the corresponding section in the QGIS manual.

## Getting ready

To be able to develop a new QGIS plugin, you should be familiar with the Python programming language. If the plugin has a graphical interface, you should have some knowledge of the Qt framework, as this is used for all UI elements, such as dialogs. To access the QGIS functionality, it is required that you know the QGIS API.

A very handy resource for all these (plus a few others) is the GeoAPIs website, which is created by SourcePole at `http://geoapis.sourcepole.com/`.

To simplify the creation of a plugin, we will use an additional plugin named Plugin Builder. It should be installed in your QGIS application.

## How to do it...

The following steps create a new plugin that will print out detailed information about the layers currently loaded in your QGIS project:

-   1. Open Plugin Builder by navigating to **Plugin | Plugin Builder**.
-   2. Fill out the dialog that will appear, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_759_0.png)

3. Click on OK.

4. In the folder selector dialog that will appear, select the folder where you want to store your plugin. Click on OK and the plugin skeleton will be created. In the selected folder, you will now have a subfolder named LayerInfoPlugin, with the following content (items in square brackets indicate folders):

```
[help]
[i18n]
[scripts]
[test]
icon.png
layerinfo.py
layerinfo_dialog.py
layerinfo_dialog_base.ui
Makefile
metadata.txt
pb_tool.cfg
```

plugin_upload.py
pylintrc
README.html
README.txt
resources.qrc
__init__.py

5. Open the layerinfo.py file in a text editor. At the end of it, you will find the run() method, with the following code:

```
def run(self):
    """Run method that performs all the real work"""
    # show the dialog
    self.dlg.show()
    # Run the dialog event loop
    result = self.dlg.exec_()
    # See if OK was pressed
    if result: pass
```

6. Replace the run method with the following code:

```
def run(self):
    layers = self.iface.legendInterface().layers()
    print "---LAYERS INFO---"
    for layer in layers:
        print "Layer name: " + layer.name()
        print "Layer source " + layer.source()
        print "Extent: " + layer.extent().asWktCoordinates()
        print
```

7. Install the pb_tool application by opening a terminal and running easy_install pb_tool (you can also use pip install pb_tool or any other way of installing a library from PyPI).

8. Open a terminal in the folder where you have the plugin code and run pb_tool and compile. Then, run pb_tool deploy to install the plugin in your local QGIS.

9. Open QGIS. Go to Plugin Manager and make sure that the new plugin we have created is there and is enabled:

![](img/2e88bf5938bf48a409c0a534a8775b21_760_0.png)
