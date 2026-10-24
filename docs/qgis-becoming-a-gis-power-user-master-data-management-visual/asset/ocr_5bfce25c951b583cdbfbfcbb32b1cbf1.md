+   2. Organize the new layers (optional step).
+     - Batch sometimes gives unfriendly names based on some bug in the dialog box.
+     - Change the layer names by doing the following for each layer created by batch:
+       1. Highlight the layer.
+       2. Navigate to **Layer | Properties**.
+       3. Change the layer name to the name of the vector layer from which this was created (for example, `applicants`). You should be able to find a hint for this value in the layer properties in the layer source (name of the `.tif` file).
+       4. Group the layers:
+          Press *Shift* + click on all the layers created by batch and the previous `roads` raster, in the **Layers** panel.
+          Right-click on the selected layers and click on **Group selected**.

# Raster analysis

Raster data, by organizing the data in uniform grids, is useful to analyze continuous phenomena or find some information at the subobject level. We will use continuous elevation and proximity data in this case, and we will look at the subapplicant object level — at the 30 meter-square cell level. You would choose a cell size depending on the resolution of the data source (for example, from sensors roughly 30 meters apart), the roughness of the analysis (regional versus local), and any hardware limitations.

First, let's make a few notes about raster data:

+   - Nodata refers to the cells that are included with the raster grid because a grid can't have completely undefined cells; however, these cells should really be considered *off the layer*.
+   - QGIS's raster renderer is more limited than in its proprietary competitors. You will want to use the **Identify** tool as well as custom styles (**Singleband Pseudocolor**) to make sense of your outputs.
+   - In this example, we will rely heavily on the GDAL and SAGA libraries that have been wrapped for QGIS. These are available directly through the processing framework with no additional preparation beyond the ordinary raster ETL. For additional functionality, you will want to consider the GRASS libraries. These are wrapped and provided for QGIS but require the additional preparation of a GRASS workspace.