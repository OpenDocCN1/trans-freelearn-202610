# Working with Rasters

The first argument we entered (1:24) defined the values, while the second argument (c(2,2,3)) defined the number of dimensions and their lengths. As opposed to creating a matrix with the matrix function, we need to explicitly specify the lengths of all dimensions (or else a one-dimensional object will be created by default) with the array function. In the previous example, we were interested in having three dimensions—two rows, two columns, and three layers (using raster terminology; see the following section). Thus, we specified their lengths as (2,2,3) using a vector of length 3.

Naturally, a three-dimensional array has a three-dimensional indexing system. For example, we can reach the (2,1,3) element in our array y as follows:

```
> y[2,1,3]
[1] 10
```

Working with arrays is very similar to working with vectors and matrices, and the application of many of the functions we have previously seen is intuitive. For example, we can use the apply function to find the means of all elements in each layer (or third dimension):

```
> apply(y, 3, mean)
[1]  2.5  6.5  10.5
```

We will see an example involving the rowMeans function and three-dimensional array objects in *Chapter 6, Modifying Rasters and Analyzing Raster Time Series*.

# Data structures for rasters in the raster package

A raster is a rectangular grid of numeric values, referenced to a certain geographical extent. As previously mentioned, spatial referencing is what differentiates a raster from the simpler data structures (matrices and arrays) we have seen previously. A raster can have a single value in each cell (a single band, or single layer, raster—analogous to a matrix) or several values (a multiband, or multilayer, raster—analogous to an array). Rasters conceptually differ from vector layers, which are data structures to represent non-gridded objects such as spatial points, lines, and polygons (these will be covered in the next chapter).