
# Spatial Analysis

+   4. Click on **Classify** to add the color map classes to the list on the left-hand side of the dialog.

+   5. Optionally, we can change the color of the first entry (for value 0) to white (by double-clicking on the color in the list) to get a smooth transition from the white map background to our heatmap.

![](img/2e88bf5938bf48a409c0a534a8775b21_114_0.png)

## Vector and raster analysis with Processing

The most comprehensive set of spatial analysis tools is accessible via the **Processing plugin**, which we can enable in the **Plugin Manager**. When this plugin is enabled, we find a **Processing** menu, where we can activate the **Toolbox**, as shown in the following screenshot. In the toolbox, it is easy to find spatial analysis tools by their name thanks to the dynamic **Search** box at the top. This makes finding tools in the toolbox easier than in the **Vector** or **Raster** menu. Another advantage of getting accustomed to the Processing tools is that they can be automated in Python and in geoprocessing models.

![](img/2e88bf5938bf48a409c0a534a8775b21_115_0.png)

In the following sections, we will cover a selection of the available geoprocessing tools and see how we can use the modeler to automate our tasks.

## Finding nearest neighbors

Finding **nearest neighbors**, for example, the airport nearest to a populated place, is a common task in geoprocessing. To find the nearest neighbor and create connections between input features and their nearest neighbor in another layer, we can use the **Distance to nearest hub** tool.

As shown in the next screenshot, we use the populated places as **Source points layer** and the airports as the **Destination hubs layer**. The **Hub layer name attribute** will be added to the `result`'s attribute table to identify the nearest feature. Therefore, we select `NAME` to add the airport name to the populated places. There are two options for **Output shape type**:

- **Point:** This option creates a point output layer with all points of the source point layer, with new attributes for the nearest hub feature and the distance to it.
- **Line to hub:** This option creates a line output layer with connections between all points of the source point layer and their corresponding nearest hub feature.

It is recommended that you use **Layer units** as **Measurement unit** to avoid potential issues with wrong measurements:

![](img/2e88bf5938bf48a409c0a534a8775b21_116_0.png)

## Converting between points, lines, and polygons

It is often necessary to be able to convert between points, lines, and polygons, for example, to create lines from a series of points, or to extract the nodes of polygons and create a new point layer out of them. There are many tools that cover these different use cases. The following table provides an overview of the tools that are available in the Processing toolbox for conversion between points, lines, and polygons:

| | To points | To lines | To polygons |
|---|---|---|---|
| From points | | Points to path | Convex hull<br>Concave hull |
| From lines | Extract nodes | | Lines to polygons<br>Convex hull |
| From polygons | Extract nodes<br>Polygon centroids<br>Random points inside a polygon | Polygons to lines | |

In general, it is easier to convert more complex representations to simpler ones (polygons to lines, polygons to points, or lines to points) than conversion in the other direction (points to lines, points to polygons, or lines to polygons). Here is a short overview of these tools:

- **Extract nodes:** This is a very straightforward tool. It takes one input layer with lines or polygons and creates a point layer that contains all the input geometry nodes. The resulting points contain all the attributes of the original line or polygon feature.
- **Polygon centroids:** This tool creates one centroid per polygon or multipolygon. It is worth noting that it does not ensure that the centroid falls within the polygon. For concave polygons, multipolygons, and polygons with holes, the centroid can therefore fall outside the polygon.
- **Random points inside polygon:** This tool creates a certain number of points at random locations inside the polygon.
- **Points to path:** To be able to create lines from points, the point layer needs attributes that identify the line (**Group field**) and the order of points in the line (**Order field**), as shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_117_0.png)

- **Convex hull**: This tool creates a convex hull around the input points or lines. The convex hull can be imagined as an area that contains all the input points as well as all the connections between the input points.
- **Concave hull**: This tool creates a concave hull around the input points. The concave hull is a polygon that represents the area occupied by the input points. The concave hull is equal to or smaller than the convex hull. In this tool, we can control the level of detail of the concave hull by changing the **Threshold** parameter between 0 (very detailed) and 1 (which is equivalent to the convex hull). The following screenshot shows a comparison between convex and concave hulls (with the threshold set to 0.3) around our airport data:
- **Lines to polygon**: Finally, this tool can create polygons from lines that enclose an area. Make sure that there are no gaps between the lines. Otherwise, it will not work.

![](img/2e88bf5938bf48a409c0a534a8775b21_118_0.png)

## Identifying features in the proximity of other features

One common spatial analysis task is to identify features in the proximity of certain other features. One example would be to find all airports near rivers. Using airports.shp and majrivers.shp from our sample data, we can find airports within 5,000 feet of a river by using a combination of the Fixed distance buffer and Select by location tools. Use the search box to find the tools in the Processing Toolbox. The tool configurations for this example are shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_119_0.png)

After buffering the airport point locations, the **Select by location** tool selects all the airport buffers that intersect a river. As a result, 14 out of the 76 airports are selected. This information is displayed in the information area at the bottom of the QGIS main window, as shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_119_1.png)

## Sampling a raster at point locations

Another common task is to sample a raster at specific point locations. Using Processing, we can solve this problem with a GRASS tool called v.sample. To use GRASS tools, make sure that GRASS is installed and Processing is configured correctly under Processing | Options and configuration. On an OSGeo4W default system, the configuration will look like what is shown here:

![](img/2e88bf5938bf48a409c0a534a8775b21_121_0.png)

At the time of writing this book, GRASS 7.0.3RC1 is available in OSGeo4W. As shown in the previous screenshot, there is also support for the previous GRASS version 6.x, and Processing can be configured to use its algorithms as well. In the toolbox, you will find the algorithms under GRASS GIS 7 commands and GRASS commands (for GRASS 6.x).

## Spatial Analysis

For this exercise, let's imagine we want to sample the **landcover** layer at the airport locations of our sample data. All we have to do is specify the vector layer containing the sample points and the raster layer that should be sampled. For this example, we can leave all other settings at their default values, as shown in the following screenshot. The tool not only samples the raster but also compares point attributes with the sampled raster value. However, we don't need this comparison in our current example:

![](img/2e88bf5938bf48a409c0a534a8775b21_122_0.png)

## Mapping density with hexagonal grids

Mapping the density of points using a hexagonal grid has become quite a popular alternative to creating heatmaps. Processing offers us a fast way to create such an analysis. There is already a pre-made script called **Hex grid from layer bounds**, which is available through the Processing scripts collection and can be downloaded using the **Get scripts from on-line scripts collection** tool. As you can see in the following screenshot, you just need to enable the script by ticking the checkbox and clicking OK:

![](img/2e88bf5938bf48a409c0a534a8775b21_123_0.png)

Then, we can use this script to create a hexagonal grid that covers all points in the input layer. The dataset of populated places (popp.shp), is a good sample dataset for this exercise. Once the grid is ready, we can run **Count points in polygon** to calculate the statistics. The number of points will be stored in the **NUMPOINTS** column if you use the settings shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_123_1.png)
