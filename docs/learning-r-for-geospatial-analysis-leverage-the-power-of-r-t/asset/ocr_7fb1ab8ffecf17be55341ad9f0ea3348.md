Plotting the object will demonstrate that u is indeed a 2 x 2 raster with three layers. The following expression plots the raster u:

```
> levelplot(u, layout = c(3,1), par.settings = RdBuTheme)
```

Using the parameter layout, we specified that the bands should be arranged in a single row and three columns within the plot area. The following screenshot shows what the plot will look like:

![](bbox=[0.21, 0.275, 0.79, 0.485])

# Transforming a raster into a matrix or an array

At times, it can be useful to transform a raster into a simpler data structure, such as a matrix or an array. One of the reasons to do that is to perform faster calculations (see Chapter 6, Modifying Rasters and Analyzing Raster Time Series). The transformations can be achieved using functions such as as.matrix and as.array, respectively.

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
```