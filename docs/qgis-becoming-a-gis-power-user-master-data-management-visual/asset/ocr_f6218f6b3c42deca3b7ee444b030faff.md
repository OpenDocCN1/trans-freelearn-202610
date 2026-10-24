If we only want to clip a raster to a certain extent (the current map view extent or any other), we can also use the raster **Save as...** functionality, as shown in *Chapter 3, Data Creation and Editing*.

For a quick exercise, we will clip the hillshade raster (`SR_50M_alaska_nad.tif`) using the Alaska Shapefile (both from our sample data) as a mask layer. At the bottom of the window, as shown in the following screenshot, we can see the concrete `gdalwarp` command that QGIS uses to clip the raster. This is very useful if you also want to learn how to use **GDAL**.

In *Chapter 2, Viewing Spatial Data*, we discussed that GDAL is one of the libraries that QGIS uses to read and process raster data. You can find the documentation of gdalwarp and all other GDAL utility programs at http://www.gdal.org/gdal_utilities.html.

![](bbox=[0.176, 0.402, 0.824, 0.704])

The default **No data value** is the no data value used in the input dataset or **0** if nothing is specified, but we can override it if necessary. Another good option is to **Create an output alpha band**, which will set all areas outside the mask to transparent. This will add an extra band to the output raster that will control the transparency of the rendered raster cells.