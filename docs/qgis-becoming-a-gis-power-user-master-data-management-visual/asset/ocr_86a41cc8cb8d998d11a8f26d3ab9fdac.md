# # Spatial Analysis

In this chapter, we will use QGIS to perform many typical geoprocessing and spatial analysis tasks. We will start with raster processing and analysis tasks such as clipping and terrain analysis. We will cover the essentials of converting between raster and vector formats, and then continue with common vector geoprocessing tasks, such as generating heatmaps and calculating area shares within a region. We will also use the Processing modeler to create automated geoprocessing workflows. Finally, we will finish the chapter with examples of how to use the power of spatial databases to analyze spatial data in QGIS.

# ## Analyzing raster data

Raster data, including but not limited to elevation models or remote sensing imagery, is commonly used in many analyses. The following exercises show common raster processing and analysis tasks such as clipping to a certain extent or mask, creating relief and slope rasters from digital elevation models, and using the raster calculator.

# ## Clipping rasters

A common task in raster processing is clipping a raster with a polygon. This task is well covered by the **Clipper** tool located in **Raster | Extraction | Clipper**. This tool supports clipping to a specified **extent** as well as clipping using a polygon **mask** layer, as follows:

+   - **Extent** can be set manually or by selecting it in the map. To do this, we just click and drag the mouse to open a rectangle in the map area of the main QGIS window.
+   - A mask layer can be any polygon layer that is currently loaded in the project or any other polygon layer, which can be specified using **Select...**, right next to the **Mask layer** drop-down list.