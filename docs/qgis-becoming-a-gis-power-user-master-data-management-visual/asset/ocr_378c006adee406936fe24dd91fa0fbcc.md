# Using measuring tools

Another core functionality of any GIS is provided by **measurement tools**. In QGIS, we find the tools needed to measure lines, areas, and angles in the **Attribute** toolbar, as shown in this screenshot:

![](bbox=[0.36, 0.23, 0.64, 0.34])

The measurements are updated continuously while we draw measurement lines, areas, or angles. When we draw a line with multiple segments, the tool shows the length of each segment as well as the total length of all the segments put together. To stop measuring, we can just right-click. If we want to change the measurement units from meters to feet or from degrees to radians, we can do this by going to **Settings | Options | Map Tools.**

# Editing attributes

There are three main use cases of attribute editing:

+   - First, we might want to edit the attributes of a specific feature, for example, to fix a wrong name
+   - Second, we might want to edit the attributes of a group of features
+   - Third, we might want to change the attributes of all features within a layer

# Editing attributes in the attribute table

All three use cases are covered by the functionality available through the **attribute table**. We can access it by going to **Layer | Open Attribute Table**, using the **Open Attribute Table** button present in the **Attributes** toolbar, or in the layer name context menu.

+   1. To change an attribute value, we always have to enable editing first.