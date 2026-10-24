# Network Analysis

```
```python
# write the results to a Shapefile
result = 'C:\\temp\\route.shp'
writer = VectorWriter(result,None,[],2,layer.crs())
fet = QgsFeature()
fet.setGeometry(QgsGeometry.fromPolyline(route_points))
writer.addFeature(fet)
del writer
processing.load(result)
```
```

+   2. If you are using your own network dataset instead of `network_pgr.shp`, which is provided with this book, adjust the coordinates of `from_point` and `to_point` for the route's starting and ending points.
+   3. Change the file paths for the result layer depending on your operating system.
+   4. Make sure that the network layer is loaded and selected in the QGIS layer list.
+   5. Save the script and run it.

# How it works...

On line 8, we created a `QgsLineVectorLayerDirector` object (http://qgis.org/api/classQgsLineVectorLayerDirector.html), which contains the network configuration. The constructor (`QgsLineVectorLayerDirector(layer,-1,'','','',3)`) parameters are as follows:

+   - The network line layer
+   - The ID of the direction field: we set it to -1 because this script does not consider one-ways
+   - The following three parameters are the values for the in link direction: reverse link direction, and two-way
+   - The last parameter is the default direction: 1 for the in link direction, 2 for the reverse direction, and 3 for the two-way

Line 10 creates the `QgsGraphBuilder` (http://qgis.org/api/classQgsGraphBuilder.html) instance, which will be used to create the routing graph on line 14.

On lines 11 and 12, we defined the starting and ending points of our route. To be able to route between these two points, they have to be matched to the nearest network link. This happens on line 13 in the `makeGraph()` function, which returns the so-called `tied_points`.

The actual route computation takes place on line 18 in the `QgsGraphAnalyzer.dijkstra()` (http://qgis.org/api/classQgsGraphAnalyzer.html) function.