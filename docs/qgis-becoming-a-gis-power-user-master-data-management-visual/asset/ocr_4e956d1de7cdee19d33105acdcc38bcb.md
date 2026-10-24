# See also

+   More information about PostGIS topology support can be found in the official PostGIS documentation at http://postgis.net/docs/manual-2.1/Topology.html

# The Topology Checker plugin

Topology is a set of rules that defines the spatial relationship between adjacent features, and it also defines and enforces data integrity and validity.

This recipe shows you how to use the built-in Topology Checker plugin to find topology errors in vector layers.

# Getting ready

To follow this recipe, load the census_wake2000_topology.shp and roadsmajor.shp layers from the sample data. Additionally, make sure that the Topology Checker plugin is enabled in Plugin Manager.

# How to do it...

The Topology Checker plugin allows us to test a vector layer or its part for different topology errors. Before we can use this, we should load all the layers that we want to test in QGIS and then configure topology rules:

+   1. Enable the Topology Checker panel in the View | Panels menu. This should add the plugin panel to the user interface, as shown in the following screenshot:

![](bbox=[0.374, 0.59, 0.627, 0.811])