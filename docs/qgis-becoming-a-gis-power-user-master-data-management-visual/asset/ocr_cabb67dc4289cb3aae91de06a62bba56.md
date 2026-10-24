> 💡 Note that there must not be a semicolon at the end of the SQL statement. Otherwise, loading the results as a new layer will fail.

+   4. Make sure that the **Geometry** column is selected correctly and click on the **Load now!** button to load the query result as a new layer, as shown in the following screenshot:

![](bbox=[0.203, 0.264, 0.806, 0.572])

# How it works...

As pgr_dijkstra only returns a list with the IDs of the route edges, we need to get the edge geometries from the original network table in order to display the route on the map. Therefore, we join the routing results with the network table on id2 (which contains the edge ID) and the network table's id column.

# See also

To make using pgRouting from within QGIS more convenient, the pgRoutingLayer plugin provides a GUI to access many of pgRouting's functions. You will find an introduction to this plugin in the *Using the pgRoutingLayer plugin for convenience* recipe.