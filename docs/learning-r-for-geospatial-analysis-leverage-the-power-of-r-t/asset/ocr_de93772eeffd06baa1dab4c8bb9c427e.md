As we can see in the preceding diagram, the number of rows in s should be equal to the number of rows in r, while the number of columns in s should be equal to the number of layers in r. In addition to setting row and column numbers, using parameters xmin, xmx, ymin, and ymx, we will set the minimum and maximum coordinates on the x and y axes (to avoid the default, where the extent is -180 to 180 on the x axis and -90 to 90 on the y axis). The coordinate ranges on the x and y axes are going to be 0 to 280 and 0 to 100, respectively; moreover, since the number of columns and rows is accordingly 280 and 100, the raster resolution will be 1 (an arbitrary unit) on both axes:

```
> s = raster(nrows = nrow(r), ncols = nlayers(r),
+ xmin = 0, xmx = nlayers(r), ymin = 0, ymx = nrow(r))
```

Now that raster s is ready, we need to populate it with values. Following our first for loop method, we will define a new function called raster_rowMeans that, when provided with a raster (x) and the layer index (layer), returns a vector of row means in that layer. This function actually consists of a single expression as it simply applies the base function rowMeans on the matrix of values from a given layer, obtained with as.matrix:

```
> raster_rowMeans = function(x, layer) {
+ rowMeans(as.matrix(x[[layer]]),
+ na.rm = TRUE)
+ }
```

Now, all is left to be done is to go over the layers of r, each time calculating the row means in the current layer (with the raster_rowMeans function) and assigning the resulting vector to the appropriate column of s (exactly as depicted in the preceding diagram):

```
> for(i in 1:nlayers(r)) {
+ s[,i] = raster_rowMeans(r, i)
+ }
```

Now s is filled with the appropriate values, and we can use the levelplot function to display it. The additional at parameter of the levelplot function sets the breaks between color levels and contours. We use it to slightly reduce the quantity of contours and make the image clearer:

```
> levelplot(s,
+ par.settings = RdBuTheme,
+ contour = TRUE,
+ margin = FALSE,
+ at = seq(0,0.6,0.05))
```