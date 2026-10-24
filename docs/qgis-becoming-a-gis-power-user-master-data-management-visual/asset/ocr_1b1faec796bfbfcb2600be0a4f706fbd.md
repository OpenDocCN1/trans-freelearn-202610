# Spatial join for a performant operational layer interaction

A spatial join permanently relates two layers of geographic objects based on some geographic relationship between the objects. It is wise to do a spatial join in this way, and save to disk when possible, as the spatial queries can significantly increase the time of a database request. This is especially true for the tables with a large number of records or when your request involves multiple spatial or aggregate functions. In this case, we are performing a spatial join so that the end user can do the queries of the hydrological data based on the location of their choosing.

QGIS has less extensive options for the spatial join criteria than ArcGIS. The default spatial join method in QGIS is accessible via Vector | Data Management Tools | Join attributes by location. However, at the time of writing, this operation was limited to the intersecting features and did not offer the functionality for nearby features. The NNJoin plugin – NN standing for nearest neighbor – achieves what we want; it joins the geographic objects in two layers based on the criteria that they are nearest to each other.

# The NNJoin plugin

Perform the following steps:

+   1. Install the NNJoin plugin.
+   2. Open the NNJoin plugin from the Vector menu (Navigate to Vector | NNJoin).
+   3. Specify the following parameters:
+      - Input vector layer: Select this as toxic – layer of toxic sites.
+      - Join vector layer: Select this as Channels – hydrological lines.
+      - Output layer: Select this as toxic_channels. This operation only supports the output to memory. You'll need to click on Save as after running it to save it to disk.