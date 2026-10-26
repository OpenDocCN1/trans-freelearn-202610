The two merged tiles seem in perfect alignment, which is expected as they originate from the same (larger) DEM. The black rectangle delineates our areas of interest centered on Haifa, which indeed intersects the 35 degrees meridian.

# Cropping and trimming

Cropping is the production of a smaller raster from an existing one by selecting a certain range of rows and columns. While the manual subset methods we used in Chapter 4, Working with Rasters, are also considered cropping, in practice we usually would like to select the required range of rows and columns by overlaying the raster with another spatial layer, rather than by manually entering row and column indices.

The crop function, given a raster and an Extent object, returns a smaller raster with the required extent. Instead of an Extent object, a raster or a vector layer can also be provided, in which case their extent is extracted and used in cropping. For example, the following expression crops dem according to haifa_surrounding:

```
> dem = crop(dem, haifa_surrounding)
```

To examine the effect, we can visualize the raster once again using the plot(dem) expression. The following screenshot shows the graphical output that is produced as a result:

![](bbox=[0.27, 0.492, 0.73, 0.825])