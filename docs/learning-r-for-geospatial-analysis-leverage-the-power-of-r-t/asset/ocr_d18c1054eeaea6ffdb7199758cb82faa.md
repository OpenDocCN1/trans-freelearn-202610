The result, r_array, is an array object where the first dimension corresponds to r rows, the second dimension to r columns, and the third dimension to r layers. We can therefore use apply to utilize rowMeans on the third dimension, obtaining the vector of row means for each third-dimension element (layer), which is exactly what we need. The values of the resulting matrix can be directly transferred to s:

```
> s[] = apply(r_array, 3, rowMeans, na.rm = TRUE)
```

The raster s we just created is identical to the one created (and plotted) earlier. Although the same results were produced, an important difference between the two methods is in the speed of execution. Accessing one raster layer at a time using a loop is slow compared to an apply operation on an array, which is generally very fast. For example, on the computer this book is written on, calculating the values of s using the first method is performed in about 1.8 minutes as compared to 2.6 seconds using the second method (that is ~40 times faster!). It is therefore often useful to transform a raster into a simpler object (matrix or array) before doing intensive calculations.

# Summary

In this chapter, you learned additional methods and procedures for working with raster layers in R. We now know how to crop, aggregate, or reproject a raster to bring it to the desired extent, resolution, and CRS. We discussed how focal filtering and clumping can be applied to highlight patterns of interest in a raster. We also discussed how topography-related variables can be derived from a DEM, and how spatio-temporal raster data can be aggregated.

In the next chapter, you will learn about the interface between rasters and vector layers, and the different ways in which both can be combined in a spatial analysis.