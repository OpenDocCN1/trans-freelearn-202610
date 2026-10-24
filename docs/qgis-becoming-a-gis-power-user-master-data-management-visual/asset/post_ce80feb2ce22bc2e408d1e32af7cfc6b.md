
## Enhancing your map view with a hillshade layer

You can combine the hillshade layer with your other layers to enhance their appearance.

As you used a DEM to compute the hillshade layer, it should be already in your QGIS project along with the hillshade itself. However, this will be covered by the hillshade because of the new layers produced by Processing are added on top of the existing ones in the layers list. Move it to the top of the layer list so that you can see the DEM (and not the hillshade layer) and style it to something like the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_618_0.png)

Lets see the steps to enhance the map view with a hillshade layer:

- 1. In the **Properties** dialog of the layer, move to the **Transparency** section, and set the **Global transparency** value to **50%**, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_618_1.png)

## How to do it...

- 1. In the **Processing Toolbox** option, find the **Fill Sinks** algorithm and double-click on it to open it:

![](img/2e88bf5938bf48a409c0a534a8775b21_620_0.png)

- 2. Select the DEM in the **DEM** field and run the algorithm. This will generate a new filtered DEM layer. From now on, we will just use this DEM in the recipe and not the original one.

3. Open **Catchment Area** and select the filtered DEM in the **Elevation** field:

![](img/2e88bf5938bf48a409c0a534a8775b21_621_0.png)

4. Run the algorithm. This will generate a catchment area layer:

![](img/2e88bf5938bf48a409c0a534a8775b21_621_1.png)

5. Open the **Channel network** algorithm and fill it in, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_622_0.png)

6. Run the algorithm. This will extract the channel network from the DEM, based on the catchment area, and it will then generate it as both a raster and vector layer:

![](img/2e88bf5938bf48a409c0a534a8775b21_623_0.png)

7. Open the **Watershed basins** algorithm and fill it in, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_623_1.png)

## Raster Analysis I

8. Run the algorithm. This will generate a raster layer with the watersheds calculated from the DEM and the channel network. Each watershed is a hydrological unit that represents the area that flows into a junction, which is defined by the channel network:

![](img/2e88bf5938bf48a409c0a534a8775b21_624_0.png)

## How it works...

Starting from the DEM, the preceding steps follow a typical workflow for hydrological analysis:

- First, the sinks are filled. This is a required preparation whenever you plan to perform a hydrological analysis. The DEM may contain sinks where a flow direction cannot be computed, which represents a problem to model the movement of water across these cells. Removing these sinks solves this problem.
- The catchment area is computed from the DEM. The values in the catchment area layer represent the area that is upstream of each cell. That is, the total area in which if water is dropped, it will eventually pass through the cell.
- Cells with high values of the catchment area will likely contain a river, while cells with lower values will have overland flow. By setting a threshold on the catchment area values, we can separate the river cells (the ones above the threshold) from the remaining ones and extract the channel network.
- Finally, we compute the watersheds associated with each junction in the channel network that was extracted in the last step.

## There's more...

The key parameter in the preceding workflow is the catchment area threshold. If a larger threshold is used, fewer cells will be considered as river cells, and the resulting channel network will be sparser. As the watershed is computed based on the channel network, this will result in a lower number of watersheds.

You can try this yourself with different values of the catchment area threshold. Here, you can see the result for threshold is equal to 1,000,000 in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_625_0.png)

The channel network has been added to help you understand the structure of the resulting set of watersheds.

Here, you can see the result for a threshold of 50,000,000 in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_625_1.png)

## Raster Analysis I

Note that in this last case, with a higher threshold value, there is only one single watershed in the resulting layer.

The threshold values are expressed in the units of the catchment area which, as the size of the cell is assumed to be in meters, are in square meters.

## Calculating a topographic index

As the topography defines and influences most of the processes that take place in a given terrain, the DEM can be used to extract many different parameters, which give us information about these processes. This recipe shows you how to calculate a popular one, which is called the Topographic wetness index, which estimates the soil wetness based on the topography.

### Getting ready

Open the DEM that we prepared in the *Preparing elevation data* recipe.

### How to do it...

- 1. Calculate a slope layer using the **Slope, aspect, curvature** algorithm from SAGA in the **Processing Toolbox** option. Calculate a catchment area layer using the **Catchment area** algorithm from the **Processing Toolbox** option. Note that you must use a sink-less DEM, such as the one that we generated in the previous recipe with the **Fill sinks** algorithm.
  
- Open the **Topographic wetness index** algorithm from the **Processing Toolbox** option and fill it in, as shown in the following screenshot:

## How it works...

The index combines slope and catchment area, two parameters that influence the soil wetness. If the catchment area value is high, this means that more water will flow into the cell, thus, increasing its soil wetness. A low value of slope will have a similar effect because the water that flows into the cell will not flow out of it quickly.

This algorithm expects the slope to be expressed in radians. This is the reason why the **Slope, aspect, curvature** algorithm has to be used because it produces its slope output in radians. The other **Slope** algorithm that you will find, which is based on the GDAL library, creates a slope layer with values expressed in degrees. You can use this layer if you convert its units using the raster calculator.

## There's more...

Other indices that are based on the same input layers can be found in different algorithms in the **Processing Toolbox** option. The Stream Power Index and the LS factor both use the slope and catchment area as inputs as well, and they can be related to potential erosion.

## Automating analysis tasks using the graphical modeler

Most analysis tasks involve using several algorithms. Repeating the same analysis with a different dataset or different input parameters requires using them one by one, making this task tedious and error-prone. You can automate analysis workflows using the Processing graphical modeler, which allows you to define a workflow graphically and wrap it in a single algorithm. This recipe introduces the main ideas about the modeler and creates a simple model as an example.

## Getting ready

No special preparation is needed in QGIS for this recipe, but make sure that you have read the previous recipe about computing a topographic index. This recipe will create a model based on the workflow in that recipe, so it is important that you understand it.

## Raster Analysis

3. Click on **OK** to add the input to the canvas:

![](img/2e88bf5938bf48a409c0a534a8775b21_630_0.png)

4. Move to the **Algorithms** tab. Double-click on the **Slope, aspect, curvature** algorithm and set the algorithm definition, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_630_1.png)
