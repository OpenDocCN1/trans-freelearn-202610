
```
5   0.2925  0.3223
6   0.4998  0.5841
7     NA    0.5841
8   0.7126  0.5086
```

We can see that the seventh point feature, the one corresponding to the (3,2) raster position, indeed contains an NA value corresponding to layer 1.

## Raster-to-contours conversion

Creating points (see the previous section) and polygons (see the next section) from a raster is relatively straightforward. In the former case, points are generated at cell centroids, while in the latter, rectangular polygons are drawn according to cell boundaries. On the other hand, lines can be created from a raster using various different algorithms designed for more specific purposes. Two common procedures where lines are generated based on a raster are constructing contours (lines connecting locations of equal value on the raster) and finding least-cost paths (lines going from one location to another along the easiest route when cost of passage is defined by raster values). In this section, we will see an example of how to create contours (readers interested in least-cost path calculation can refer to the gdistance package, which provides this capability in R).

As an example, we will create contours from the DEM of Haifa (dem; see the previous chapter). Creating contours can be done using the rasterToContour function. This function accepts a RasterLayer object and returns a SpatialLinesDataFrame object with the contour lines. The rasterToContour function internally uses the base function contourLines, and arguments can be passed to the latter as part of the rasterToContour function call. For example, using the levels parameter, we can specify the breaks where contours will be generated (rather than letting them be determined automatically).

The raster dem consists of elevation values ranging between -14 meters and 541 meters:

```
> range(dem[], na.rm = TRUE)
[1] -14 541
```

Therefore, we may choose to generate six contour lines, at 0, 100, 200, ..., 500 meter levels:

```
> dem_contour = rasterToContour(dem, levels = seq(0, 500, 100))
```

Let's take a look at the attribute table of dem_contour:

```
> dem_contour@data
    level
C_1     0
C_2   100
C_3   200
C_4   300
C_5   400
C_6   500
```

Indeed, the layer consists of six line features—one for each break we specified with the levels argument.

## Raster-to-polygons conversion

As mentioned previously, raster-to-polygon conversion involves the generation of rectangular polygons in the place of each raster cell (once again, excluding NA cells). Similar to the raster-to-point conversion, the resulting attribute table contains the respective raster values for each polygon created. The conversion to polygons is most useful with categorical rasters when we would like to generate polygons defining certain areas in order to exploit the analysis tools this type of data is associated with (such as extraction of values from other rasters, geometry editing, and overlay).

Creation of polygons from a raster can be performed with a function whose name the reader may have already guessed, rasterToPolygons. A useful option in this function is to immediately dissolve the resulting polygons according to their attribute table values; that is, all polygons having the same value are dissolved into a single feature. This functionality internally utilizes the rgeos package and it can be triggered by specifying dissolve=TRUE.

In the previous chapter, we prepared a raster named l_rec_focal_clump, a categorical raster with consecutively numbered NDVI>0.2 patches. In our next example, which we will begin in this chapter and finish in Chapter 9, Advanced Visualization of Spatial Data, we will visually compare the average NDVI time series of Lahav and Kramim forests (see earlier), based on all of our Landsat (three dates) and MODIS (280 dates) satellite images. In this chapter, we will only prepare the necessary data by going through the following intermediate steps:

1. Creating the Lahav and Kramim forests polygonal layer.
2. Extracting NDVI values from the satellite images.
3. Creating a data.frame object that can be passed to graphical functions later.

The graphical output is shown in the following screenshot:

![](img/ebfd6294770b225fdeceda9a7b8b560e_240_0.png)

The preceding screenshot shows the continuous NDVI>0.2 patches, which are 1 km² or larger, within the studied area. Two of these, as expected, are the forests we would like to examine. How can we select them? Obviously, we could export pol to a Shapefile and select the features of interest interactively in a GIS software (such as QGIS), then import the result back into R to continue our analysis. The raster package also offers some capabilities for interactive selection (that we do not cover here); for example, a function named `click` can be used to obtain the properties of the pol features we click in a graphical window such as the one shown in the preceding screenshot. However, given the purpose of this book, we will try to write a code to make the selection automatically without further user input.

To write a code that makes the selection, we must choose a certain criterion (either spatial or nonspatial), that separates the features of interest. In this case, for example, we can see that the pol features we wish to select are those closest to Lahav Kibbutz. Therefore, we can utilize the towns point layer (see earlier) to find the distance of each polygon from Lahav Kibbutz, and select the two most proximate ones.

Using the gDistance function (see Chapter 5, Working with Points, Lines, and Polygons), we will first find out the distances between each polygon in pol and each point in towns:

```
> dist_towns = gDistance(towns, pol, byid = TRUE)
> dist_towns
          1        2
112 14524.94060 12697.151
114  5484.66695  7529.195
137  3863.12168  5308.062
203    29.48651  1119.090
240  1910.61525  6372.634
371 11687.63594 11276.683
445 12751.21123 14371.268
460 14860.25487 12300.319
```

The returned matrix, named dist_towns, contains the pairwise distances, with rows corresponding to the pol features and columns corresponding to the towns features. Since Lahav Kibbutz corresponds to the first towns feature (column "1"), we can already see that the fourth and fifth pol features (rows "203" and "240") are the most proximate ones, thus corresponding to the Lahav and Kramim forests. We could subset both forests by simply using their IDs—pol[c("203","240"),]. However, as always, we are looking for general code that will select, in this case, the two closest features irrespective of the specific IDs or row indices. For this purpose, we can use the order function, which we have not encountered so far. This function, given a numeric vector, returns the element indices in an increasing order according to element values. For example, applying order to the first column of dist_towns, we can see that the smallest element in this column is in the fourth row, the second smallest is in the fifth row, the third smallest is in the third row, and so on:

```
> dist_order = order(dist_towns[, 1])
> dist_order
[1] 4 5 3 2 6 7 1 8
```

The graphical output is shown in the following screenshot:

![](img/ebfd6294770b225fdeceda9a7b8b560e_243_0.png)

The preceding screenshot demonstrates that we did indeed correctly select the features of interest.

We can also assign the forest names to the attribute table of forests, relying on our knowledge that the first feature of forests (ID "203") is larger and more proximate to Lahav Kibbutz and corresponds to the Lahav forest, while the second feature (ID "240") corresponds to Kramim.

```
> forests$name = c("Lahav", "Kramim")
> forests@data
    clumps  area  name
203    281 9.5274  Lahav
240    314 6.7842  Kramim
```

We now have a polygonal layer named `forests`, with two features delineating the Lahav and Kramim forests, named accordingly in the attribute table. In the next section, we will proceed with extracting the NDVI data for these forests.

## Extracting raster values based on vector layers

So far, we have covered operations to transform a vector layer to a raster and vice versa. The third operation involving vector layers and rasters—and the focus of this final section—is the extraction of raster values according to vector layers. We are often interested in reducing or summarizing raster data using point, line, or polygon features, which is when this operation comes in handy. For example, we may wish to calculate the elevation profile covered by a GPS track (raster-to-line extraction) or the average NDVI of a given forest (raster-to-polygon extraction). In this section, we will see two examples involving extraction by points and extraction by polygons.

### Extracting by points

Extraction of raster values, according to a vector layer of any kind, can be done with the `extract` function. The first two parameters of this function are as follows:

- The raster whose values are to be extracted (x)
- The object (usually a vector layer) defining the locations to extract values (y) from

When extracting values according to points, which is the simplest extraction scenario, the returned object may be either of the following:

- *Vector* — when extracting values from a single band raster
- *Matrix* — when extracting values from a multiband raster

As an example of extracting single band raster values to points, we will use the `spain_stations.csv` file we previously created (see *Chapter 3, Working with Tables*), which contains a table with the spatial location records of 96 meteorological stations in Spain. We will create a `SpatialPointsDataFrame` object based on this table, and then use it to extract elevation values from a DEM of Spain. The DEM will be obtained from a file (available on the book's website) named `spain_elev.tif`.
