
- **Raster**: This toolbar includes histogram stretch, brightness, and contrast control
- **Vector**: This currently only contains the **Coordinate Capture** tool, but it will be filled in by additional Python plugins
- **Web**: This is currently empty, but it will also be filled in by additional Python plugins
- **Help**: This toolbar points to the option for downloading the user manual and looks like this:

![](img/2e88bf5938bf48a409c0a534a8775b21_34_0.png)

On the left screen border, we place the **Manage Layers** toolbar. This toolbar contains the tools for adding layers from the *vector or raster files, databases, web services, and text files or create new layers*:

![](img/2e88bf5938bf48a409c0a534a8775b21_34_1.png)

Finally, on the right screen border, we have two more toolbars:

- **Digitizing**: The tools in this toolbar **enable editing**, basic feature creation, and editing
- **Advanced Digitizing**: This toolbar contains the **Undo/Redo** option, **advanced editing tools, the geometry-simplification tool**, and so on, which look like this:

![](img/2e88bf5938bf48a409c0a534a8775b21_34_2.png)

> All digitizing tools (except the **Enable advanced digitizing tools** button) are currently inactive. They will turn active only once we start editing a vector layer.

Toolbars and panels can be activated and deactivated via the **View** menu's **Panels** and **Toolbars** entries, as well as by right-clicking on a menu or toolbar, which will open a context menu with all the available toolbars and panels. All the tools on the toolbars can also be accessed via the menu. If you deactivate the **Manage Layers Toolbar**, for example, you will still be able to add layers using the **Layer** menu.

As you might have guessed by now, QGIS is highly customizable. You can increase your productivity by assigning shortcuts to the tools you use regularly, which you can do by going to **Settings | Configure Shortcuts**. Similarly, if you realize that you never use a certain toolbar button or menu entry, you can hide it by going to **Settings | Customization**. For example, if you don't have access to an Oracle Spatial database, you might want to hide the associated buttons to remove clutter and save screen estate, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_35_0.png)

## Finding help and reporting issues

The QGIS community offers a variety of different community-based support options. These include the following:

- **GIS StackExchange**: One of the most popular support channels is http://gis.stackexchange.com/. It's a general-purpose GIS question-and-answer site. If you use the tag qgis, you will see all QGIS-related questions and answers at http://gis.stackexchange.com/questions/tagged/qgis.
- **Mailing lists**: The most important mailing list for user questions is qgis-user. For a full list of available mailing lists and links to sign up, visit http://www.qgis.org/en/site/getinvolved/mailinglists.html#qgis-mailinglists. To comfortably search for existing mailing list threads, you can use Nabble (http://osgeo-org.1560.x6.nabble.com/Quantum-GIS-User-f4125267.html).
- **Chat**: A lot of developer communication runs through IRC. There is a #qgis channel on www.freenode.net. You can visit it using, for example, the web interface at http://webchat.freenode.net/?channels=#qgis.

> Before contacting the community support, it's recommended to first take a look at the documentation at http://docs.qgis.org.

If you prefer commercial support, you can find a list of companies that provide support and custom development at http://www.qgis.org/en/site/forusers/commercial_support.html#qgis-commercial-support.

If you find a bug, please report it because the QGIS developers can only fix the bugs that they are aware of. For details on how to report bugs, visit http://www.qgis.org/en/site/getinvolved/development/bugreporting.html.

## Summary

In this chapter, we installed QGIS and configured it by selecting useful defaults and arranging the user interface elements. Then we explored the panels, toolbars, and menus that make up the QGIS user interface, and you learned how to customize them to increase productivity. In the following chapter, we will use QGIS to view spatial data from different data sources such as files, databases, and web services in order to create our first map.

## 2
Viewing Spatial Data

In this chapter, we will cover how to view spatial data from different data sources. QGIS supports many file and database formats as well as standardized Open Geospatial Consortium (OGC) Web Services. We will first cover how we can load layers from these different data sources. We will then look into the basics of styling both vector and raster layers and will create our first map, which you can see in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_37_0.png)

We will finish this chapter with an example of loading background maps from online services.

## Viewing Spatial Data

For the examples in this chapter, we will use the sample data provided by the QGIS project, which is available for download from `http://qgis.org/downloads/data/qgis_sample_data.zip` (21 MB). Download and unzip it.

## Loading vector data from files

In this section, we will talk about loading vector data from GIS file formats, such as **shapefiles**, as well as from text files.

We can load vector files by going to **Layer | Add Layer | Add Vector Layer** and also using the **Add Vector Layer** toolbar button. If you like shortcuts, use *Ctrl + Shift + V*. In the **Add vector layer** dialog, which is shown in the following screenshot, we find a drop-down list that allows us to specify the encoding of the input file. This option is important if we are dealing with files that contain special characters, such as German umlauts or letters from alphabets different from the default Latin ones.

![](img/2e88bf5938bf48a409c0a534a8775b21_38_0.png)

What we are most interested in now is the **Browse** button, which opens the file-opening dialog. Note the file type filter drop-down list in the bottom-right corner of the dialog. We can open it to see a list of supported vector file types. This filter is useful to find specific files faster by hiding all the files of a different type, but be aware that the filter settings are stored and will be applied again the next time you open the file opening dialog. This can be a source of confusion if you try to find a different file later and it happens to be hidden by the filter, so remember to check the filter settings if you are having trouble locating a file.

We can load more than one file in one go by selecting multiple files at once (holding down *Ctrl* on Windows/Ubuntu or *cmd* on Mac). Let's give it a try:

1. First, we select `alaska.shp` and `airports.shp` from the shapefiles sample data folder.
2. Next, we confirm our selection by clicking on **Open**, and we are taken back to the **Add vector layer** dialog.
3. After we've clicked on **Open** once more, the selected files are loaded. You will notice that each vector layer is displayed in a random color, which is most likely different from the color that you see in the following screenshot. Don't worry about this now; we'll deal with layer styles later in this chapter.

![](img/2e88bf5938bf48a409c0a534a8775b21_39_0.png)

Even without us using any spatial analysis tools, these simple steps of visualizing spatial datasets enable us to find, for example, the southernmost airport on the Alaskan mainland.

> There are multiple tricks that make loading data even faster; for example, you can simply drag and drop files from the operating system's file browser into QGIS.
> 
> Another way to quickly access your spatial data is by using QGIS's built-in file *browser*. If you have set up QGIS as shown in *Chapter 1, Getting Started with QGIS*, you'll find the browser on the left-hand side, just below the layer list. Navigate to your data folder, and you can again drag and drop files from the browser to the map.
> 
> Additionally, you can mark a folder as *favorite* by right-clicking on it and selecting **Add as a favorite**. In this way, you can access your data folders even faster, because they are added in the **Favorites** section right at the top of the browser list.
