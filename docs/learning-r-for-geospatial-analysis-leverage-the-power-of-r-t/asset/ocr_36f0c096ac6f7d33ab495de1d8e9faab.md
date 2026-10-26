# Working with Points, Lines, and Polygons

The dimensions function returns the number of spatial dimensions:

```
```r
> dimensions(airports)
[1] 2
```
```

> In this book, we only deal with two-dimensional vector layers (geometries on a plane). Three-dimensional layers can also be useful to represent certain types of data, such as points with (x,y) coordinates and (z) elevation.

# Accessing the attribute table of vector layers

The attribute table of a vector layer is, in fact, a `data.frame` object and some of the functions that work with `data.frame` objects have been defined to consistently work directly on vector layers as well. For example, the nrow, ncol, and dim functions applied to a vector layer refer to its attribute table to return its dimensions:

```
```r
> nrow(county)
[1] 3145
> ncol(county)
[1] 4
> dim(county)
[1] 3145    4
```
```

We see that the attribute table of county has 3,145 rows (thus, the layer has 3,145 features) and four columns. The columns contain the following information:

+   - `NAME_1`: The first-level name (for example, the state name)
+   - `NAME_2`: The second-level name (for example, the county name)
+   - `TYPE_2`: The feature type (for example "County" or "Water body")
+   - `FIPS`: The FIPS code

Individual columns of an attribute table, or subsets of these, can be accessed with the `$` and `[` operators. For example, the second-level names (held in the `NAME_2` column) of the first 10 features in `county` can be obtained as follows:

```
```r
> county$NAME_2 [1:10]
[1] "Litchfield" "Hartford"    "Tolland"    "Windham"
[5] "Siskiyou"  "Del Norte"   "Modoc"       "New London"
[9] "Fairfield" "Middlesex"
```
```