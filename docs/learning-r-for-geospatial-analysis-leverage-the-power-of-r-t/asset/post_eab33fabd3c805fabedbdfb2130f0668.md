
## Changing the spatial extent or resolution of rasters

With reference to raster geometry, so far we have only dealt with operations where the raster extent is manually reduced by selecting a certain combination of rows and columns to retain (refer to *Chapter 4, Working with Rasters*). In this chapter, we will review more operations that provide us with the freedom to modify the raster geometry of datasets according to our specific requirements.

In this section, we will see how we can change the extent or resolution of rasters without modifying the underlying grid arrangement. This category includes operations such as merging rasters, cropping, or aggregating/disaggregating raster cells. In the next section, we will see how the underlying grid (and possibly the CRS) can be modified through resampling and reprojection.

In the first few examples of this chapter, we will work with a DEM of the area surrounding Haifa, and experiment with the modification of raster extent and resolution. A DEM is a raster holding elevation values, thus representing the topography of the specific area that it covers. There are many ways of creating a DEM, including interpolation from a limited set of elevation measurements (such as points or contours) or conducting continuous measurements of the surface (with instruments such as **Light Detection and Ranging (LIDAR)** or stereoscopy). Here, we will use a part of the near-global DEM created in 2000, in an effort called the **Shuttle Radar Topography Mission (SRTM)**. In this mission, elevation data has been collected using an instrument on board the Space Shuttle Endeavour, covering the Earth's surface up to 60 degrees latitude, north and south.

SRTM data is available online for free (visit `http://srtm.csi.cgiar.org/`). It is also directly accessible through R, using the getData function of the raster package (we already used this function in the previous chapter to obtain the Haifa administrative borders layer). The SRTM DEM is divided into tiles of 5 x 5 degrees of longitude/latitude that can be downloaded separately. With getData, we need to set the dataset name to "SRTM" and specify geographic coordinates of interest (longitude and latitude). The returned object is a RasterLayer covering the corresponding tile.

Incidentally, the longitude of Haifa is 34.99 degrees (you can check this quickly with geocode ("Haifa"); refer to the previous chapter), and thus it is located at the intersection of two 5 x 5 degrees tiles. To cover Haifa, we will download both tiles:

- longitude 30-35, latitude 30-35
- longitude 35-40, latitude 30-35

Certain operations with rasters (such as merging or reprojection) can be time-consuming. A useful additional parameter to the merge function, and many other functions in the raster package, is progress. Setting progress="text" or progress="window" will show a progress bar (in the textual output or in a separate window) while the function code runs. This way, the user can assess how much time a given operation will take (and perhaps modify the code to bypass extremely time-consuming steps). The rasterOptions(progress="text") expression sets the progress parameter to "text" globally for the current R session.

We now have a DEM of the whole area of longitude 30-40 and latitude 30-35 (as shown in the following screenshot). Our next step will be to isolate the area of interest surrounding Haifa. We will define the area as the bounding box of haifa_buildings (refer to the previous chapter), supplemented with 0.25 degrees on all four sides. For this purpose, we will calculate the bounding box of haifa_buildings (using the extent function) and then add 0.25 degrees to it, as follows. Beforehand, we will read the haifa_buildings layer once again to have it in a geographic CRS:

```
> haifa_buildings = readOGR("C:\\Data", "haifa_buildings")
> haifa_surrounding = extent(haifa_buildings) + 0.25
```

The created object haifa_surrounding is an Extent object defining our rectangular area of interest. Let's plot dem and the Extent object using a pair of plot function calls:

```
> plot(dem)
> plot(haifa_surrounding, add = TRUE)
```

The graphical output is shown in the following screenshot:

![](img/ebfd6294770b225fdeceda9a7b8b560e_196_0.png)

The two merged tiles seem in perfect alignment, which is expected as they originate from the same (larger) DEM. The black rectangle delineates our areas of interest centered on Haifa, which indeed intersects the 35 degrees meridian.

## Cropping and trimming

Cropping is the production of a smaller raster from an existing one by selecting a certain range of rows and columns. While the manual subset methods we used in Chapter 4, Working with Rasters, are also considered cropping, in practice we usually would like to select the required range of rows and columns by overlaying the raster with another spatial layer, rather than by manually entering row and column indices.

The crop function, given a raster and an Extent object, returns a smaller raster with the required extent. Instead of an Extent object, a raster or a vector layer can also be provided, in which case their extent is extracted and used in cropping. For example, the following expression crops dem according to haifa_surrounding:

```
> dem = crop(dem, haifa_surrounding)
```

To examine the effect, we can visualize the raster once again using the plot(dem) expression. The following screenshot shows the graphical output that is produced as a result:

![](img/ebfd6294770b225fdeceda9a7b8b560e_197_0.png)

## Modifying Rasters and Analyzing Raster Time Series

The high-elevation area in the lower half of the preceding screenshot is Mount Carmel. The city of Haifa is located on its northern slopes.

Another useful function within the context of raster cropping is `trim`. This function removes the outer rows and columns of a raster that all have the same value. For example, using `trim(x)`, we can automatically remove unnecessary NA margins from the raster x.

> A raster (in R, and in general) is by definition rectangular; rasters where values cover a non-rectangular area are obtained by surrounding the area of interest with NA values (such as the Mediterranean sea area in the preceding screenshot). Both `crop` and `trim` are used to carve smaller rectangular extents from existing rasters by removing whole rows and columns from their margins. When we are interested in carrying non-rectangular shapes, we need, in fact, to fill the unnecessary areas with NA, which is made possible with the `mask` function (we'll see this in the next chapter).

## Aggregating and disaggregating

Aggregation is the creation of a lower-resolution raster by grouping rectangular sets of cells in the original raster into individual, larger cells in the new raster. Aggregation might be necessary when we are ready to lose detail to gain processing efficiency or noise reduction. For example, MODIS satellite data products are distributed in several spatial (and temporal) resolutions, from 250 to 5600 m, with the lower-resolution products usually being the results of aggregation from the original higher-resolution data. The lower-resolution data are useful since as we increase the spatial and temporal extent that is being analyzed, it becomes increasingly unfeasible (and unnecessary) to work with high resolutions. For example, an analysis of NDVI trends over time at a 250 meter resolution on a global scale would involve huge amounts of data; a 5,600 meter resolution is more reasonable for such a task (it would reduce the amount of data 500-fold!). In addition, lower-resolution products are typically associated with greater confidence since noisy pixels are averaged out during aggregation.

The aggregate function performs aggregation by grouping adjacent pixels into new ones, given an input raster and the aggregation factor fact. The aggregation factor controls the size of the new raster cells in units of the original cells. It can either be a single integer (in which case, aggregation is equal on both axes) or a vector of length 2 (in which case, different levels of aggregation are applied on the x and y axes). For example, using fact=8 means that each set of 8*8 cells in the original raster becomes a single cell in the new raster. Using fact=c(5,10) means that each rectangle of 5*10 cells (5 along the x axis, 10 along the y axis) becomes a single cell. In addition, the fun parameter specifies which function will be used to calculate the new values based on existing ones (the default is mean), and the na.rm parameter specifies whether NA values are removed from each set prior to calculation (the default is TRUE). To demonstrate the behavior of aggregate, we will apply it on our dem raster, aggregating 8*8 and 16*16 sets of cells:

```
> dem_agg8 = aggregate(dem, fact = 8)
> dem_agg16 = aggregate(dem, fact = 16)
```

The following expressions sequentially plot the original dem, and the aggregation results dem_agg8 and dem_agg16, along with the appropriate plot titles specified using the main parameter:

```
> plot(dem, main = "Original image")
> plot(dem_agg8, main = "8x8 aggregated")
> plot(dem_agg16, main = "16x16 aggregated")
```

The following screenshot shows the resulting individual plots from left to right:

![](img/ebfd6294770b225fdeceda9a7b8b560e_199_0.png)

We can see that the preceding screenshot becomes increasingly blurred as the degree of aggregation increases.
