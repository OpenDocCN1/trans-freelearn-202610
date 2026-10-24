
### There's more...

You'll notice that with this particular example, an issue that arises with converting rasters to nonrectangular shapes; the edges are jagged as compared to the vector. If you need it to be really smooth, there are a few options. You can decrease the pixel size, splitting current pixels into multiple pixels using the `-tr` option. As with other Raster tools, you can use the pencil icon to override the GDAL command-line options to add features not included in the interface. In this case, the `-tr` option is inline with the rest of the already formatted command:

```
gdalwarp -tr 100 100
```

This would make each pixel 100 x 100 units instead of the current 500 x 500.

![](img/2e88bf5938bf48a409c0a534a8775b21_518_0.png)

> Some important options to remember when saving TIFF files with GDAL are number type (Integer versus Float) and compression. Both of these can greatly impact the final file size. Refer to the Converting Vectors to Rasters recipe later in this chapter for an example. Also, if you have a multicore CPU, add `-multi` to take advantage of your CPU cores for faster processing of most raster operations.

### See also

- For a full list of the gdalwarp options refer to http://gdal.org/gdalwarp.html

## Clipping vectors

Like rasters, occasionally you only need vector data to cover a certain area of study (area of interest). Also, like rasters, you can use a layer defining the extent that you want to select only a portion of a vector layer to make a new layer. The tool that is used for this job is Clip; that is, 'Cookie Cutter' because of how the results look afterwards.

### Getting ready

For the example in this recipe, we will use `geology.shp` and clip it to the extent of Wake County using `census_wake2000.shp`. Any vector layer with the aggregation of polygons covering all of the county will work.

### How to do it...

1. Load the two layers, `geology.shp` and `census_wake2000.shp`.
2. Open the clipping tool from **Vector | Geoprocessing Tools | Clip**:

![](img/2e88bf5938bf48a409c0a534a8775b21_519_0.png)

3. Input layer is the layer that has to be cut; this is `geology.shp`.
4. Clip layer defines the boundaries that have to be cut.

   > There is no requirement that the clip layer be contiguous. You can cut any combination of shapes that you want, circles, squares, triangles, and so on. They just need to be polygons.

5. (Optional) Check **Add result to canvas** so that you see the results immediately.
6. Select **OK** to run the tool:

![](img/2e88bf5938bf48a409c0a534a8775b21_520_0.png)

### How it works...

All clipping is based on the principle of intersection features. For each feature in Input, the tool checks to see whether it intersects with the overall shape of the clip layer. When it does intersect, the algorithm then checks whether any part falls outside the intersection. When a part lands outside, it is cut off.

### There's more...

You have to be careful when using clip. If the original table contained columns that included measurements such as area and perimeter, these values are copied from the original. Therefore, they may not reflect the new sizes and shapes that were cut.

Generally, all geometry operations and analysis should be done with layers in the same projection in order to ensure consistent results. Also, many tools are not projection aware and won't compensate for two source layers being in different projections.

Tools that create the intersection of objects (for example, in PostGIS's and SpatiaLite's `ST_Intersection`) can provide you with similar results. However, you may need to perform multiple steps: Intersect, then select by contains or intersection to eliminate unwanted data.

### See also

Refer to the next recipe in the chapter if you want a way to limit features without altering the original spatial data.

## Extracting vectors

Clipping is great, except when you don't want to alter the original geometries, such as when you want to select overlapping features. Or, in other cases, you just want to filter the geometries based on nonspatial attributes. To achieve both of these results, you can utilize the Selection tools in combination with **Save Layer As...** to extract just the features of interest. This recipe uses spatial selection methods to extract a subset of original polygons without altering them.

### Getting ready

We'll use the same data as the previous recipe, `geology.shp` and `census_wake2000.shp`.

### How to do it...

1. Select polygons from `geology.shp` that overlap with Wake County (`census_wake2000.shp`) by navigating to **Vector | Research Tools | Select by Location**.
2. Select the features in **geology**.
3. Intersect the features in `census_wake2000`.
4. Modify the current selection by creating new selection.
5. Click on **OK**:

![](img/2e88bf5938bf48a409c0a534a8775b21_522_0.png)

6. Now, you will see the matching features highlighted (by default in yellow):

![](img/2e88bf5938bf48a409c0a534a8775b21_522_1.png)

7. If the selection looks good, use the **Layer context menu** (right-click) and click on **Save As...** to create a file containing only the selection.

> When in the **Save As...** dialog (as described in *Chapter 1, Data Input and Output*), make sure to check the box next to **Save only selected features**.

### How it works...

This really goes back to the same fundamental concept of Intersection that most vector analyses rely on. When you can test whether two features overlap, there are many different operations possible based on the answer. You can select, deselect, or, as in the previous recipe, select then cut to fit. In these cases, each polygon is tested for at least a partial intersection, and the matches are then highlighted as the results.

### There's more...

While this recipe demonstrates how to select a subset of data based on location, you can also do the same thing based on attributes of the features with a query on the attribute table. Or, you can combine attribute-based selection, spatial selection, and hand selection graphically on the map—any selection combination that you want can be saved as a new layer.

You may also notice in the **Select by Location** tool that vectors can also be added or removed from existing selections in case you want to perform more complicated operations involving more than one type of criteria.

### See also

- Refer to the documentation on PostGIS, SpatiaLite, or the *PostGIS Cookbook*, by Packt Publishing, for how to perform similar operations using SQL in PostGIS (SpatiaLite queries are very similar).

## Converting rasters to vectors

Sometimes, you need to convert data that is originally in raster format to a vector format in order to perform vector-based analysis methods. Generally speaking, as rasters are continuous datasets, converting them to polygons is more common than converting them to lines or points.

### Getting ready

You need a raster layer, preferably one with groups of the same-valued pixels next to each other. For this example, we'll use `geology_30m.tif`, as a 30-meter x 30-meter pixel should give decent results.

> The smaller the pixels, the smoother looking the resulting vector will appear when zoomed out.

### How to do it...

1. Load `geology_30m.tif`.
2. Go to **Raster | Conversion | Polygonize**:

![](img/2e88bf5938bf48a409c0a534a8775b21_524_0.png)

3. Name the output `geology_30m.shp`.
4. (Optional) Name the output column `geology`, `class`, or `value`.
5. Press **OK** to run the process.
6. Compare the results (colors are in a similar but different scale):

![](img/2e88bf5938bf48a409c0a534a8775b21_525_0.png)

### How it works...

For each pixel, the value is compared to its neighbors (there are different neighbor algorithms). When two pixels next to each other have the same value, they are lumped into a polygon. Additional neighbors of the same value get added to the polygon until pixels of differing values are encountered. As it's pixel-based, the edge of the result will usually follow the outline of the pixels, making for a jagged edge. This edge can be smoothed into straight lines with additional options or other tools, such as the QGIS smoothing tool.

### There's more...

The minimum number of pixels required to make a polygon, or the maximum allowed value difference to be counted as the same, can be altered to drop out isolated pixels or to allow for a range of values to be counted together.

Note that if each pixel is unique as compared to its adjacent neighbors, then you'll just end up with a polygon for each pixel. Or, if your raster is sufficiently large and varied, this process could take days. You may want to reconsider whether you really need to convert or whether your analysis can be done in raster. Another option would be to resample or reclassify the raster to larger pixels first to decrease the data density. Or, you need to investigate remote sensing-type tools that perform classification to create related groupings of pixels based on similarity.

If you want to convert raster data to points, then you probably want to use the points sampling tool. If you want to convert some portion of a raster to lines, then you may need more sophisticated feature extraction tools found, for example, in SAGA and GRASS, either through the Processing toolbox or as standalone software. Or, you may even need to resort to a mix of pixel extraction and hand digitizing.
