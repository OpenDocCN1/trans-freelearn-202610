The opposite operation from aggregation is disaggregation; it can be performed with the disaggregate function. For example, the disaggregate(dem_agg8,fact=8) expression would yield back a higher-resolution raster with the same resolution of dem. However, the information that has been lost in aggregation is not recovered (each set of 8*8 adjacent cells would have the same value) and the disaggregated raster will visually appear identical to dem_agg8.

# # Raster resampling and reprojection

In this section, we are going to transfer the values of a given raster from its own grid to a different grid. This operation is known as resampling. Raster reprojection is a closely related operation; it is basically resampling to a grid having a different CRS than that of the original raster.

# # Raster resampling

Raster resampling can be performed using the `resample` function. The required parameters of this function are as follows:

+   - The raster whose values are to be resampled (x)
+   - A raster that defines the grid to which the values will be transferred (y)
+   - The resampling method (`method`)

The `resample` function currently provides two resampling methods. The method that we use determines the way in which a cell in the new raster gets its value, based on the old raster's values:

+   - `"ngb"`: Nearest-neighbor; assigns the value of the nearest cell
+   - `"bilinear"`: Bilinear interpolation; assigns a weighted average of the four nearest cells (the default)

![](bbox=[0.231, 0.655, 0.768, 0.704])