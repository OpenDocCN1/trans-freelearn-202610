Up and Coming

+   4. The **Threshold** field is used to define the snapping tolerance in map units. For our example, you can leave this unchanged.
+   5. Click on the **Run** button to remove overshoots and undershots. When the algorithm is finished, two new layers will be added to QGIS: the `Cleaned` layer contains features with fixed errors and the `Errors` layer contains original invalid features.

# How it works...

The rmdangle tool simply sequentially removes all dangling lines with length less than the given threshold. If the threshold is less than 0, then all dangles will be removed.

The break tool breaks lines at intersections, so all lines will have a common node. This tool does not need a threshold value to be specified.

The snap tool tries to snap vertices to another one within the given threshold, if no appropriate vertices are found, then no snapping is done. It is worth mentioning that large threshold values may break the topology of polygonal features.

# There's more...

If you need more control over the topology cleaning process, try to use **v.clean.advanced** from the **Processing Toolbox** menu or consider using the GRASS plugin.

Also, there are other ways to clean vector topology, for example, using the lwgeom functions or external tools such as prepair and pprepair. Both tools are available as **Processing** plugins, and they can be installed via **Plugin Manager**.

# See also

+   More information about the GRASS v.clean toolset can be found at http://grass.osgeo.org/grass64/manuals/v.clean.html

# Hunting for bugs

While QGIS developers do their best to make every QGIS release as stable as possible, sometimes you may encounter bugs or even crashes. To get them fixed in the future, it is necessary to inform the developers about issues.

This recipe shows you how to perform basic debugging and collect information that will help developers understand the problem better and help to fix it.