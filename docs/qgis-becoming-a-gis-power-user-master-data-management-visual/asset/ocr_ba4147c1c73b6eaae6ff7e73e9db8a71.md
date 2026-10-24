# Visualizing GIS data

Although the data has been added to the GIS through the ETL process, it is of limited value without adding some visualization enhancements.

# The layer style

The layer style is configured through the Style tab in the Layer Properties dialog. The Single Symbol style is the default style type, and it simply shows all the geographic layer objects using the same basic symbol. This setting doesn't provide any visual differentiation between objects other than their apparent geospatial characteristics. The Categorized and Graduated style types provide different styles according to the attribute table field of your choosing. Graduated, which applies to quantitative data, is particularly powerful in the way the color and symbols size are mapped to a numerical scale. This is all accomplished through the Layer Properties | Style tab.

To configure a simple graduated layer style to the data, perform the following steps:

+   1. Under the Layer Properties | Style tab, select Graduated for the style type.
+   2. Select your quantitative field for Column (such as temperature_mean_temp).
+   3. Click on Classify to group your data into the number of classes specified (by default, 5) and to select the classification mode (Equal Interval, by default):

![](bbox=[0.183, 0.506, 0.816, 0.821])