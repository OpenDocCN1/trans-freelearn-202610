6. Compare the results (colors are in a similar but different scale):

![](bbox=[0.281, 0.176, 0.719, 0.345])

# How it works...

For each pixel, the value is compared to its neighbors (there are different neighbor algorithms). When two pixels next to each other have the same value, they are lumped into a polygon. Additional neighbors of the same value get added to the polygon until pixels of differing values are encountered. As it's pixel-based, the edge of the result will usually follow the outline of the pixels, making for a jagged edge. This edge can be smoothed into straight lines with additional options or other tools, such as the QGIS smoothing tool.

# There's more...

The minimum number of pixels required to make a polygon or the maximum allowed value difference to be counted as the same can be altered to drop out isolated pixels or to allow for a range of values to be counted together.

Note that if each pixel is unique as compared to its adjacent neighbors, then you'll just end up with a polygon for each pixel. Or if your raster is sufficiently large and varied, this process could take days. You may want to reconsider whether you really need to convert or whether your analysis can be done in raster. Another option would be to resample or reclassify the raster to larger polygons first to decrease the data density. Or, you need to investigate remote sensing type tools that perform classification to create related groupings of pixels based on similarity.

If you want to convert raster data to points, then you probably want to use the points sampling tool. If you want to convert some portion of a raster to lines then you may need more sophisticated feature extraction tools found; for example, in SAGA and GRASS, either through the Processing toolbox or as standalone software. Or, you may even need to result to a mix of pixel extraction and hand digitizing.