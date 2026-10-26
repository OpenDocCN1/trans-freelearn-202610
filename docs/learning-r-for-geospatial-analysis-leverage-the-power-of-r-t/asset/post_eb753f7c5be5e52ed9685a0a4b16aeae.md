
## Accessing raster values with the matrix notation

Since a raster band is a two-dimensional object, it is frequently more useful to access its values using a two-dimensional notation. As with matrix objects, the first element of the two-dimensional index refers to rows and the second element refers to columns. For example, values 1-5 (in vector terms) of raster `r[[1]]` occupy row 1, columns 1-5. We can refer to these same values using a two-dimensional notation as follows:

```
> r[[1]] [1, 1:5]
[1] 0.4242 0.3995 0.4190 0.4272 0.4285
```

Note that, even though we are using a two-dimensional notation to subset the raster, the values are still returned in the form of a one-dimensional numeric vector.

## Subsets involving more than one layer

In the last two sections, we accessed a subset of raster values confined to a single layer. What happens when we subset both the row/column and layer dimensions of a raster? In that case, we get a matrix object, rather than a vector, with columns referring to layers and rows referring to raster cells. For example, using the following expression, we are referring to the values occupying row 1, columns 1-5, and layers 1-3:

```
> r[[1:3]] [1, 1:5]
      modis.1 modis.2 modis.3
[1,]  0.4242  0.4518  0.4211
[2,]  0.3995  0.3334  0.4123
[3,]  0.4190  0.3430  0.4314
[4,]  0.4272  0.3430  0.4761
[5,]  0.4285  0.5814  0.4761
```

As a result, we get a matrix with three columns (corresponding to layers 1-3) and five rows (corresponding to the requested five cells, ordered from the top-left corner rightwards). Indeed, the values in the first column of the matrix are identical to the values of the vector we got in the previous two examples.

As another example, we can examine the course of NDVI over time at a single pixel—for example, in row 45, column 33—by omitting the band index this time (and thus referring to all bands at once):

```
> v = r[45, 33][1, ]
```

Note that with `r[45,33]`, we get a matrix object with 280 columns (since we access all 280 bands of raster `r`) and a single row (since we access a single cell). Then, with the `[1,]` part, we select the first (and only) row in that matrix, containing the values of the (45,33) cell across all bands. As we witnessed earlier, a single matrix row is by default simplified to a vector. Finally, we assign the vector of NDVI values to `v`.

To plot the resulting NDVI time series, now held in `v`, we will use the date column in the `dates` table (see the previous chapter), which lists the dates of acquisition for each band in `r`. We will also specify the labels for the x and y axes, using parameters `xlab` and `ylab` of the `plot` function, respectively:

```
> plot(v ~ dates$date, type = "l", xlab = "Time", ylab = "NDVI")
```

The resulting graphical output is shown in the following screenshot:

![](img/ebfd6294770b225fdeceda9a7b8b560e_135_0.png)

We can clearly see the periodical behavior of NDVI at the annual scale; NDVI increases in winter (the wet season), when vegetation is more abundant, and declines in summer (the dry season), when vegetation desiccates. Lower-than-usual NDVI values have been observed from 2009 to 2011 due to a drought period the region experienced at the time.

In all subset methods we have seen in the last three sections, the result was automatically converted to a simpler object, either to a vector (when dealing with values from a single band) or a matrix (when dealing with values from several bands). If we want to suppress the simplification, we can specify `drop=FALSE`, the same way we have seen regarding subsets of a `data.frame` object (see the previous chapter) and a `matrix` object. In the following example, using `drop=FALSE` yields a subset `RasterBrick` object named `u` that has the first two rows, two columns, and three layers of the original raster `r`:

```
> u = r[[1:3]][1:2, 1:2, drop = FALSE]
```

Plotting the object will demonstrate that `u` is indeed a 2 x 2 raster with three layers. The following expression plots the raster `u`:

```
> levelplot(u, layout = c(3,1), par.settings = RdBuTheme)
```

Using the parameter `layout`, we specified that the bands should be arranged in a single row and three columns within the plot area. The following screenshot shows what the plot will look like:

![](img/ebfd6294770b225fdeceda9a7b8b560e_136_0.png)

## Transforming a raster into a matrix or an array

At times, it can be useful to transform a raster into a simpler data structure, such as a matrix or an array. One of the reasons to do that is to perform faster calculations (see Chapter 6, Modifying Rasters and Analyzing Raster Time Series). The transformations can be achieved using functions such as `as.matrix` and `as.array`, respectively.

For example, a single layer of a raster can be transformed into a matrix as follows:

```
> as.matrix(u[[1]])
     [,1]   [,2]
[1,] 0.4242 0.3995
[2,] 0.4495 0.2925
```

A multiband raster can be transformed into an array as follows:

```
> as.array(u[[1:2]])
, , 1

     [,1]   [,2]
[1,] 0.4242 0.3995
[2,] 0.4495 0.2925

, , 2

     [,1]  [,2]
[1,] 0.4518 0.3334
[2,] 0.4846 0.3223
```

If we try to convert a multiband raster into a matrix with `as.matrix`, we will get a matrix with rows representing cells and columns representing layers, as we have seen earlier in the context of raster subsetting:

```
> as.matrix(u[[1:2]])
      layer.1 layer.2
[1,]  0.4242  0.4518
[2,]  0.3995  0.3334
[3,]  0.4495  0.4846
[4,]  0.2925  0.3223
```

## Overlay and reclassification of rasters

In this section, we will introduce two basic operations involving rasters: performing mathematical operations between overlapping rasters and reclassifying the values of a raster into new aggregated categories.

### Raster algebra and overlay operations

In many cases, when we have two or more overlapping rasters, we would like to apply a certain function on each pair, triplet, and so on of overlapping pixels in those rasters. As a result, we will get a new raster, where the value of each pixel is the result of the latter function on the respective pixels in the input rasters. Such operations are also referred to as raster algebra, usually when using straightforward arithmetic notation (such as `r+s`, where `r` and `s` are rasters) or overlay operations.

There are numerous functions that can be used in raster algebra expressions, including arithmetic operators (such as `+`, `-`, `*`, and `/`), logical operators (such as `>`, `>=`, `<`, `<=`, `==`, and `!`), and several simple functions (such as `min`, `max`, and `sum`). Numeric values can also be combined with rasters (as long as the first object in the expression is a raster), in which case the numeric values are recycled as if to form a raster where all the values are equal to the given numeric value. For example, if `r` is a raster, `r+1` would yield a new raster, where 1 is recycled and added to each of the values in raster `r`. Similarly, the expression `r*2` would yield a raster where each value is multiplied by 2. A single band raster can also be combined with a multiband raster, in which case the single band raster is recycled.

For example, the following expression will give us a new raster with the minimal NDVI values observed in each pixel over the 280 layers of raster `r`, excluding `NA` values:

```
> min_ndvi = min(r, na.rm = TRUE)
```

![](img/ebfd6294770b225fdeceda9a7b8b560e_138_0.png)

In cases where we would like to apply a certain function on the values of the raster (rather than performing an overlay operation), we need to apply the respective function on the vector of raster values, rather than on the raster object itself.

For example, to find out the minimum value among all cells in the first band of raster `r`, we will have to use the following expression:

```
> min(r[[1]][], na.rm = TRUE)
[1] 0.007
```

Applying the `min` function on the raster itself, with `min(r[[1]], na.rm=TRUE)`, is an overlay operation. It is not what we want to do in this case since an overlay would give us a new raster where each cell contains the minimal observed value among all bands of the input raster. The result will be `r[[1]]` itself since there is only one band to choose from.

Using the `range` function on the multiband raster `r` would give us a new raster with two bands (since the function returns a vector of length 2), one with the minimal values observed in each pixel and the other with the maximal values observed in each pixel:

```
> range_ndvi = range(r, na.rm = TRUE)
```

We can examine the `range_ndvi` result by plotting it with the `levelplot` function (this time, showing contours with `contour=TRUE`):

```
> levelplot(range_ndvi, par.settings = RdBuTheme, contour = TRUE)
```
