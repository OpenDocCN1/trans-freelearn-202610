The correct y/x ratio is thus equal to y_range/x_range or 1.666667. As for the z/x ratio, since the z axis range of 527.0933 (the difference, in meters, between the lowest and highest points of the DEM) is much smaller than either the x or y axes ranges, drawing the 3D plot with realistic proportions between elevation and x-y distances (z_range/x_range or 0.02440247) would result in a very flat image, with the relief hardly protruding. Unless we make an image of a small area with very steep topography, it is very common to use an exaggerated z to x-y distance ratio in 3D topographic plots, thus making the topography more tangible. Choosing a z/x aspect ratio is really a matter of taste. For this particular plot, a seven-fold exaggeration was chosen, but lower or higher values are also appropriate. It is obviously important to declare the fact that exaggeration has been used when such an image is used in a publication.

Now that we have decided which aspect ratios we are going to use, we are ready to apply wireframe on dem_df. Let's first see the code and the image produced and then review its components:

```
> library(lattice)
> wireframe(z ~ x * y,
+ data = dem_df,
+ drape = TRUE,
+ colorkey = TRUE,
+ col.regions = terrain.colors(100),
+ screen = list(z = 165, x = -60, y = 0),
+ aspect = c(y_range/x_range, 7*(z_range/x_range)),
+ zoom = 1.1)
```