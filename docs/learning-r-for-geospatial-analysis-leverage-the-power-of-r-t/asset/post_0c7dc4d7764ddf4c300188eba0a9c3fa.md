
We will plot the results using the levelplot function of the rasterVis package. Contours are enabled with contour=TRUE to highlight the differences between methods:

```
```r
> library(rasterVis)
> levelplot(resample_results,
+ par.settings = RdBuTheme,
+ contour = TRUE)
```
```

The following screenshot shows the graphical output:

![](img/ebfd6294770b225fdeceda9a7b8b560e_205_0.png)

As we saw in the previous example with nearest-neighbor resampling (the left image), sets of adjacent cells that overlap with a single 500 meter pixel get the same value, and thus the contours follow the coarse-grained pattern of MODIS pixels. With bilinear interpolation (the right image), on the other hand, each 30 m cell gets the average of four MODIS pixels closest to it, weighted according to their respective distances; thus, most (if not all) cell values are unique and the image is much smoother.

## Raster reprojection

As mentioned earlier, raster reprojection— unlike vector layers reprojection— involves resampling. Unlike vector layers, where all points are independent and thus their coordinates can be individually transformed to a new CRS (refer to the previous chapter), raster pixel coordinates that are transformed to a different CRS will not form a legitimate grid on that CRS (that is, a grid with equal distances between all adjacent pixels and parallel to the CRS x and y axes); rasters therefore require resampling. Thus, raster reprojection consists of two steps: reprojection of the raster pixel coordinates to the new CRS (analogous to reprojecting a vector layer) and resampling of the pixel values to a grid defined in the new CRS.

There are two main ways of defining the new grid in raster reprojection: we can either provide only the new CRS definition and let the grid be generated automatically, or we can provide a specific grid of our own. In the latter case, the process is very similar to resampling; the only change is that the new grid has a different CRS from that of the original raster.

In R, the `projectRaster` function provides raster reprojection functionality and supports both previously mentioned ways of defining a new grid. In the first case, when we want the grid to be automatically generated, we need to supply:
- The raster to be reprojected (`from`)
- The target CRS, as a PROJ.4 string (`crs`)
- Optionally, the required resolution (`res`)

In the second case, when we want to define the grid ourselves, we need to supply:
- The raster to be reprojected (`from`)
- The raster defining the target grid (`to`)

In both scenarios, we also need to specify the resampling method, either "ngb" or "bilinear" (the default).

As an example, we will reproject `dem` from its geographic CRS to the UTM Zone 36N projection. The corresponding PROJ.4 string, as we have already done in the previous chapters, will be obtained from another raster (`r`). Prior to reprojection, let's print out the current properties of `dem` in order to compare them later with the properties of the reprojection result:

```
> dem
class      : RasterLayer
dimensions : 390, 384, 149760  (nrow, ncol, ncell)
resolution : 0.0008333333, 0.000833333  (x, y)
```

```
extent      : 34.83083, 35.15083, 32.63583, 32.96083  (xmin, xmax)
coord. ref. : +proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,
data source : in memory
names       : layer
values      : -14, 541  (min, max)
```

We see that, indeed, dem is defined in a geographic CRS, and has a resolution of 0.0008333333 degrees, longitude and latitude.

In case we do not have any special requirements for the projected dem to align with an existing layer, we can follow the first reprojection approach, supplying only the CRS and letting the grid be generated automatically. We can also supply the optional res argument, primarily to make sure the x and y axes resolution will be equal (90 m, for example). You can try and execute the following expression without specifying res, to verify that the default grid generated in this case will have a resolution of 78 m and 92.4 m on the x and y axes respectively:

```
> dem = projectRaster(from = dem,
+ crs = proj4string(r),
+ method = "ngb",
+ res = 90)
```

Printing the properties of the reprojected dem reveals several differences:

```
> dem
class       : RasterLayer
dimensions  : 417, 351, 146367  (nrow, ncol, ncell)
resolution  : 90, 90  (x, y)
extent      : 670666.3, 702256.3, 3611918, 3649448  (xmin, xmax, 
coord. ref. : +proj=utm +zone=36 +datum=WGS84 +units=m +no_defs +
data source : in memory
names       : layer
values      : -14, 541  (min, max)
```

We can see that the CRS definition has changed to UTM Zone 36N. Also, the extent, resolution, and dimensions are now different and characterize the new grid that has been generated.

Plotting the new raster with plot (dem) produces the following graphical output:

![](img/ebfd6294770b225fdeceda9a7b8b560e_208_0.png)

UTM Zone 36N coordinates are now shown along the axes. In addition, you may have noticed that the projected dem image is not exactly parallel to the axes, as the UTM grid was not strictly parallel to the geographic one. Since a raster must be rectangular (refer to the preceding screenshot), empty areas around image margins, consisting of NA values, have been generated.

## Filtering and clumping

In this section, and the following one, we move on from the subject of changing raster geometry to the subject of relations between neighboring raster cell values. These relations can be summarized in the form of a new raster using a variety of methods. In this section, we will introduce two such methods: focal filtering and clumping.

Focal filtering involves assigning in each cell of a raster (the focal cell) the result of a function, whose input is the set of values from a neighborhood of cells surrounding the focal one (including itself). The neighborhood size is predetermined (for example, a neighborhood of 3*3 cells is commonly used), and the input raster is scanned in a moving window manner until complete coverage has been reached. There are many appropriate functions that can be implemented in filters for various purposes. For example, using the mean function (also known as a low-pass filter) makes an image look smoother, while using a function that finds the most common value (also known as a majority filter) is useful for reducing noise in a categorical raster.

The focal function is used to apply a focal filter on a RasterLayer object. The three major parameters of this function are as follows:
- The RasterLayer object to be filtered (x)
- A matrix defining the neighborhood and the cell weights (w)
- A function to be applied on the neighborhood (fun)

The w argument should be a matrix defining the neighborhood. It should have odd dimensions (such as a 3*3 or 7*5), since its center defines the focal cell position. The matrix values define the weights that are applied on cell values before these are transferred to the focal function. However, using weights is optional—a matrix where all values are equal to 1 would imply having no weights. Weights of 0 can also be used to create a non-symmetric and/or non-rectangular window. The fun argument should be a function to be applied on the neighborhood values (the default is sum).

For the filtering example, we will return to 1_rec, a reclassified NDVI raster (0 for NDVI≤0.2 and 1 for NDVI>0.2) based on the Landsat image obtained on October 4, 2000 (refer to Chapter 4, Working with Rasters, for more information). This raster shows the locations of the more densely vegetated areas (such as planted pine forests); however, the image is quite grained, with 0 values appearing within the otherwise continuous high-vegetation (1) zones.

## Modifying Rasters and Analyzing Raster Time Series

To increase continuity, we will buffer the NDVI>0.2 zone by converting all cells having at least one immediate 1 neighbor to 1, thus reducing gaps of 0s between or within vegetated areas. This operation can be carried out by applying a focal filter with a 3*3 window and the max function with no weights. With max, each cell gets the maximal value of its 3*3 neighborhood. Thus, a 0 cell either retains its value (if all nine neighbors are 0) or converts to 1 (in case at least one of its neighbors is 1). All 1 cells retain their value, since they have at least one 1 neighbor (the cell itself). Since we do not need weights, we will use a 3*3 matrix consisting of plain 1s for the w argument. This can be created as follows:

```
```r
> matrix(1, nrow = 3, ncol = 3)
     [,1] [,2] [,3]
[1,]    1    1    1
[2,]    1    1    1
[3,]    1    1    1
```
```

Therefore, the complete expression that applies the filter on l_rec is as follows:

```
```r
> l_rec_focal = focal(l_rec,
+ w = matrix(1, nrow = 3, ncol = 3),
+ fun = max)
```
```

The result is assigned to l_rec_focal; comparing it with the original l_rec raster shows the buffering of vegetated areas that took place (refer to the next screenshot).

Our second example of an operation concerning cell neighbors is clumping. In clumping, we are interested in the detection of cell patches that have the same value. For example, you may wish to learn how many patches there are, what their size distribution is, what properties they have with respect to the values of other rasters, and so on. Clumping can be performed with function clump, which returns a new raster where a unique ID is assigned to every connected patch of cells that have the same value in the input raster (except for values of 0 or NA, which are ignored and used as background). The operation is only useful with categorical rasters—in continuous rasters, all (or most) cells usually have unique values. Thus, no patches larger than one cell can be formed.

The clump function accepts a RasterLayer object and returns a raster where each patch of similar-valued cells has a unique ID. Specifying gaps=FALSE will force these IDs to be consecutive integers, starting with 1 and going to n (where n is the total number of patches). For example, let's clump the l_rec_focal regions of 1:

```
```r
> l_rec_focal_clump = clump(l_rec_focal, gaps = FALSE)
```
```
