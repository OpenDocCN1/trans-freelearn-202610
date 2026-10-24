# Identifying the Best Places

> 💡 You can select multiple layers to add by pressing **Shift** and clicking on the contiguous files or pressing **Ctrl** and clicking on the noncontiguous files.

+   3. Import the Digital Elevation Model from `c2/data/original/dem/dem.tif`.
+      1. Navigate to **Layer | Add Layer | Raster Layer**.
+      2. From the dem directory, select `dem.tif` and then click on **Open**.

+   4. Even though the layers are in a different CRS, QGIS does not warn us in this case. You must discover the issue by checking each layer individually. Check the CRS of the county layer and one other layer:
+      1. Highlight the county layer in the **Layers** panel.
+      2. Navigate to **Layer | Properties**.
+      3. The CRS is displayed under the **General** tab in the **Coordinate reference system** section:

![](bbox=[0.185, 0.431, 0.814, 0.76])

> 🗺️ Note that the county layer is in **EPSG: 26957**, while the others are in **EPSG: 2776**.