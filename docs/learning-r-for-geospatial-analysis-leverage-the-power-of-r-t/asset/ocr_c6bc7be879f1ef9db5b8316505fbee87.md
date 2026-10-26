The resulting table, named `spainT_tab`, looks as follows:

```
> spainT_tab
  year variable
1  2006      mmnt
2  2007      mmnt
3  2008      mmnt
4  2009      mmnt
5  2010      mmnt
6  2006      mmxt
7  2007      mmxt
8  2008      mmxt
9  2009      mmxt
10 2010      mmxt
```

We are ready to create the second loop that will go through the rows of `spainT_tab` and spatially interpolate the respective temperature data. Since our results are going to be rasters (rather than numeric values, as was the case with the cross-validation loop) in this case, they cannot be appended to the table itself. Instead, we are going to stack the results in a multiband raster where each layer is going to correspond to a row in `spainT_tab`. In other words, `spainT_tab` is going to function as a supplementary table for the resulting multiband raster (much as `dates` served the same purpose for the raster `r`; see Chapter 4, Working with Rasters). To construct the raster, we will first create an empty `RasterStack` to which the interpolation results will be appended during loop execution:

```
> spainT = stack()
```

The loop will go through the rows of `spainT_tab`, each time performing the following set of operations:

+   1. Creating a point layer named `dat` with the variable and year set according to the current row of `spainT_tab`.
+   2. Fitting a variogram model (`v`).
+   3. Creating a `gstat` object (`g`) according to `dat` and `v`.
+   4. Calculating the raster of predicted values (`z`) with `interpolate`, based on `g` and `dem_spain`.
+   5. Masking `z` according to `dem_spain` and attaching the result to `spainT`.