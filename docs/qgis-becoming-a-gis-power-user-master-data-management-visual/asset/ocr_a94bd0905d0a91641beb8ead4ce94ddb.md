+   3. You will be converting the point to the **EPSG:26911** CRS. Use the filter box to find it among the list of available CRSs and select it. Then click on **OK**.
+   4. Click on **OK** in the *Save as* dialog to create the layer. A new shapefile will be created with the projected lines.

# How it works...

Reprojecting is done by the OGR library when it saves the file because this is one of the options that it supports.

# There's more...

Raster layers can be reprojected in a similar way:

+   1. In the *Save as* dialog, for raster layers, you can find a CRS field with a **Browse** button.
+   2. Click on it to open the CRS selector, and select the destination CRS.
+   3. When you click on **OK**, the raster layer will be exported using the selected CRS instead of its original one.

# Batch format conversion

The *Save as* dialog can be used to convert the format of a single layer. When several layers have to be converted, it is a better idea to use some automation. This recipe shows you how to easily convert an arbitrary number of layers.

# Getting ready

No previous preparation is needed. Batch conversion is not performed based on open layers but performed directly on files, so there is no need to open layers in QGIS before converting them.