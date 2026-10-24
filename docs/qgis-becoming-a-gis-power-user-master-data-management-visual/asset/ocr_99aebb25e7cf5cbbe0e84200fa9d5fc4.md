# How to do it...

+   1. Start by loading `poi_name_wake`.
+   2. Zoom to the layer extent.
+   3. Open the Interpolation tool by navigating to **Raster** | **Interpolation** | **Interpolation**.
+   4. Select `poi_names_wake` for **Input**.
+   5. Select `elev_m` for **Interpolation attribute**.
+   6. Click on the **Add** button, your selection should appear in the box on the left-hand side.
+   7. Select **Inverse Distance Weighted (IDW)** for **Interpolation Method**.
+   8. Now, set the **Extent** and **Cell Size** properties. In **Cellsize X** and **Cellsize Y**, enter 100 and 100. This forces the output cells to be 100x100 units of the current projection.
+   9. Click on the **Set to current extent** button in the middle.
+   10. Next to **Output file box**, click on the button labeled ... to set the output path to save the results:
+   11. Pick the folder and type in a name with no file extension, such as `idw100m` (the result will be an ASCII raster `.asc` file), as shown in the following screenshot:

> Yes, it's on the **Raster** menu; the source data must be a vector, but the results are a raster.

> Generally, if this was for analysis, you would attempt to match the region of interest or other raster layers. In this case, we just want to go for sensibly-sized cells. As the map is in UTM, we will want cells to be integers that represent metric units; 100 meters by 100 meters makes interpreting the results easier.