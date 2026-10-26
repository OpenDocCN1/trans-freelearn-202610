# Modifying Rasters and Analyzing Raster Time Series

For example, using the slope and aspect layers that we previously calculated, the following expression creates a hillshade layer according to the sun's elevation of 20 degrees and the sun's direction of 235 degrees:

```
> hill = hillshade(slope, aspect, 20, 235)
```

Now, with the following expression, we will plot the resulting hill raster using grayscale (as indicated by the `par.settings` argument `GrTheme`):

```
> levelplot(hill, par.settings = GrTheme, margin = FALSE)
```

The following screenshot shows the graphical output:

![](bbox=[0.189, 0.327, 0.81, 0.807])