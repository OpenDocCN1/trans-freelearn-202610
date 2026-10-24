+   3. Once the settings are configured, we can calculate our first route. Select the **Start** and **Stop** locations using the buttons marked with crosshair icons. Activate the crosshair button and then click in the map to select a location. This location will be marked on the map, and the coordinates will be automatically inserted into the **Start** or **Stop** input field.
+   
+   4. Click on **Calculate** to initiate route computation. Depending on the size of the network used, this step will either be very fast or it can take much more time. The route will be highlighted in the map, and the route length and travel time will be displayed, as shown in the following screenshot:

![](bbox=[0.176, 0.302, 0.825, 0.482])

+   5. If you want to store the computed shortest path, click on **Export** and you will be able to choose whether you want to create a new layer for the path or add the route to an existing line layer.
+   
+   6. To compute a new route, simply change the start and stop locations and click on **Calculate** again.
+   
+   7. Click on **Clear** to remove the route highlights when you are done.

# How it works...

The Road graph plugin uses the QGIS network analysis library, which implements Dijkstra's algorithm. For a given starting node, the algorithm finds the path with the lowest cost (that is, the shortest path if the cost criterion is length or the fastest path if the cost criterion is time) between this node and every other node in the network. This can also be used to find costs of the shortest paths from a start node to a destination node by stopping the algorithm once the shortest path to the destination node has been determined.