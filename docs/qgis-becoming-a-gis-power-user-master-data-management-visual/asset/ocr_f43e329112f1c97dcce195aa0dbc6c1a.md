Networks and routing are in no way a GIS-only topic. You will find a lot of math literature related to this, called **Graph Theory**. In this chapter, we will use the following terms to talk about networks:

+   - A **network** (also known as **graph**) is a collection of connected objects
+   - These objects are called **nodes** (also known as vertices)
+   - The connections between nodes are called **links** (also known as **edges**)

The following figure explains these terms:

![](bbox=[0.375, 0.3, 0.625, 0.475])

The two routing tools that are commonly used with QGIS are as follows:

+   - The Road graph plugin, which is one of the QGIS core plugins; that is, this plugin is available in every QGIS installation, but you may have to activate it in Plugin Manager
+   - The PostGIS extension pgRouting, which can be used directly through the QGIS DB Manager, or more comfortably through the pgRoutingLayer plugin, which can be installed from the QGIS plugin repository using Plugin Manager

# Creating a simple routing network

In this recipe, we will create a routing network from scratch using the QGIS editing tools. Even though more and more open network data is available, there will still be numerous use cases where necessary network data does not exist or is not available for use. Therefore, it is good to know how to create a network and what to pay attention to in order to avoid common pitfalls.

For the task of network creation, the main difference between the Road graph plugin and pgRouting is that pgRouting needs a network node (that is, link start or end node) at each intersection while the Road graph plugin will also use intermediate link geometry nodes to infer intersections if two links share a node. In this recipe, we will create a network, which can be used in both tools.