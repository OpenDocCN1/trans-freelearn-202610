# Creating vector layers from a raster

The opposite operation to rasterization, which has been presented in the previous section, is the creation of vector layers from raster data. The procedure of extracting features of interest out of rasters, in the form of vector layers, is often necessary for analogous reasons underlying rasterization—when the data held in a raster is better represented using a vector layer, within the context of specific subsequent analysis or visualization tasks. Scenarios where we need to create points, lines, and polygons from a raster can all be encountered. In this section, we are going to see an example of each.

# Raster-to-points conversion

In raster-to-points conversion conversion, each raster cell center (excluding NA cells) is converted to a point. The resulting point layer has an attribute table with the values of the respective raster cells in it.

Conversion to points can be done with the rasterToPoints function. This function has a parameter named spatial that determines whether the returned object is going to be SpatialPointsDataFrame or simply a matrix holding the coordinates and the respective cell values (spatial=FALSE, the default value). For our purposes, it is thus important to remember to specify spatial=TRUE.

As an example of a raster, let's create a subset of the raster r, with only layers 1-2, rows 1-3, and columns 1-3:

```
> u = r[[1:2]][1:3, 1:3, drop = FALSE]
```

To make the example more instructive, we will place NA in some of the cells and see how this affects the raster-to-point conversion:

```
> u[2, 3] = NA
> u[[1]][3, 2] = NA
```

Now, we will apply rasterToPoints to create a SpatialPointsDataFrame object named u_pnt out of u:

```
> u_pnt = rasterToPoints(u, spatial = TRUE)
```

Let's visually examine the result we got with the first layer of u serving as the background:

```
> plot(u[[1]])
> plot(u_pnt, add = TRUE)
```