
You will notice that the style doesn't look perfect yet. This is because each line feature is drawn separately, one after the other, and this leads to a rather disconnected appearance. Luckily, this is easy to fix; we only need to enable the so-called symbol levels. To do this, select the Line entry in the symbol layers list and tick the checkbox in the Symbol Levels dialog of the Advanced section (the button in the bottom-right corner of the style dialog), as shown in the following screenshot. Click on Apply to test the results.

![](img/2e88bf5938bf48a409c0a534a8775b21_60_0.png)

Before we move on to styling polygons, let's take a look at the other symbol layer types for lines, which include the following:

- **Simple line**: This is a solid or dashed line
- **Marker line**: This line is made up of point markers located at line vertices or at regular intervals
- **Geometry Generator**: This enables us to manipulate geometries and even create completely new geometries using the built-in expression engine.

Whenever we create a symbol that we might want to reuse in other maps, we can save it by clicking on the Save button under the symbol preview area. We can assign a name to the new symbol, and after we save it, it will be added to the saved symbols preview area on the right-hand side.

## Creating polygon styles – an example of a landmass style

In this exercise, we will create a style for the alaska.shp file. The goal is to create a simple fill with a blue halo. As in the previous river style example, we will combine two symbol layers to create this style: a Simple fill layer that defines the main fill color (white) with a thin border (in gray), and an additional Simple line outline layer for the (light blue) halo. The halo should have nice rounded corners. To achieve these, change the Join style option of the Simple line symbol layer to Round. Similar to the previous example, we again enable symbol levels; to prevent this landmass style from blocking out the background map, we select the Multiply blending mode, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_62_0.png)

Before we move on, let's take a look at the other symbol layer types for polygons, which include the following:

- **Simple fill**: This defines the fill and outline colors as well as the basic fill styles
- **Centroid fill**: This allows us to put point markers at the centers of polygons
- **Line/Point pattern fill**: This supports user-defined line and point patterns with flexible spacing
- **SVG fill**: This fills the polygon using SVGs
- **Gradient fill**: This allows us to fill polygons with linear, radial, or conical gradients
- **Shapeburst fill**: This creates a gradient that starts at the polygon border and flows towards the center
- **Outline: Simple line or Marker line**: This makes it possible to outline areas using line styles
- **Geometry Generator**: This enables us to manipulate geometries and even create completely new geometries using the built-in expression engine.

A common use case for Point pattern fill symbol layers is topographic symbols for different vegetation types, which typically consist of a Simple fill layer and Point pattern fill, as shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_63_0.png)

## Viewing Spatial Data

When we design point pattern fills, we are, of course, not restricted to simple markers. We can use any other marker type. For example, the following screenshot shows how to create a polygon fill style with a **Font** marker pattern that shows repeating alien faces from the **Webdings** font:

![](img/2e88bf5938bf48a409c0a534a8775b21_64_0.png)

As an alternative to simple fills with only one color, we can create **Gradient** fill symbol layers. Gradients can be defined by **Two colors**, as shown in the following screenshot, or by a **Color ramp** that can consist of many different colors. Usually, gradients run from the top to the bottom, but we can change this to, for example, make the gradient run from right to left by setting **Angle** to 270 degrees, as shown here:

![](img/2e88bf5938bf48a409c0a534a8775b21_64_1.png)

The **Shapeburst fill** symbol layer type, also known as a "buffered" gradient fill, is often used to style water areas with a smooth gradient that flows from the polygon border inwards. The following screenshot shows a fixed-distance shading using the **Shade to a set distance** option. If we select **Shade whole shape** instead, the gradient will be drawn all the way from the polygon border to the center.

![](img/2e88bf5938bf48a409c0a534a8775b21_65_0.png)

## Loading background maps

Background maps are very useful for quick checks and to provide orientation, especially if you don't have access to any other base layers. Adding background maps is easy with the help of the **QuickMapServices** plugin. It provides access to satellite, street, and hybrid maps by different providers.

If you load the OSM TF Landscape layer, your map will look like what is shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_67_0.png)

An alternative to the QuickMapServices plugin is OpenLayers Plugin, which provides very similar functionality but offers fewer different background maps.

## Dealing with project files

QGIS project files are human-readable XML files with the filename ending with .qgs. You can open them in any text editor (such as Notepad++ on Windows or gedit on Ubuntu) and read or even change the file contents.

When you save a project file, you will notice that QGIS creates a second file with the same name and a .qgs~ ending, as shown in the next screenshot. This is a simple backup copy of the project file with identical content. If your project file gets corrupted for any reason, you can simply copy the backup file, remove the ~ from the file ending, and continue working from there.

![](img/2e88bf5938bf48a409c0a534a8775b21_68_0.png)

By default, QGIS stores the relative path to the datasets in the project file. If you move a project file (without its associated data files) to a different location, QGIS won't be able to locate the data files anymore and will therefore display the following Handle bad layers dialog:

![](img/2e88bf5938bf48a409c0a534a8775b21_68_1.png)

> If you are working with data files that are stored on a network drive rather than locally on your machine, it can be useful to change from storing relative paths to storing absolute paths instead. You can change this setting by going to Project | Project Properties | General.

To fix the layers, you need to correct the path in the **Datasource** column. This can be done by double-clicking on the path text and typing in the correct path, or by pressing the **Browse** button at the bottom of the dialog and selecting the new file location in the file dialog that opens up.

> A comfortable way to copy QGIS projects to other computers or share QGIS projects and associated files with other users is provided by the **QConsolidate** plugin. This plugin collects all the datasets used in the project and saves them in one directory, which you can then move around easily without breaking any paths.

![](img/2e88bf5938bf48a409c0a534a8775b21_69_0.png)

## Summary

In this chapter, you learned how to load spatial data from files, databases, and web services. We saw how QGIS handles coordinate reference systems and had an introduction to styling vector and raster layers, a topic that we will cover in more detail in *Chapter 5, Creating Great Maps*. We also installed our first Python plugin, the QuickMapServices plugin, and used it to load background maps into our project. Finally, we took a look at QGIS project files and how to work with them efficiently. In the following chapter, we will go into more detail and see how to create and edit raster and vector data.
