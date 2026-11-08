# Creating a geomap with a choropleth layer

In this exercise, you will create a geomap based on a choropleth layer, which displays divided geographical areas in color. This provides a way to visualize values over a geographical area to show variation or patterns across the displayed location. Here are the steps to create this layer:

+   28. Click on the Add icon on the Insert toolbar, and select geomap.
+   29. In the Builder panel, select Topography from the Basemap list, and click on the Add Layer link.
+   30. Select the BestRun_Demo model for Data Source.
+   31. For layer Type, select Choropleth Layer.
+   32. Click on the Add Location Dimension link, and select Stores as the Location Dimension.
+   33. Click on the Add Measure link under the Choropleth Color section, and select Net Revenue from the list. Select a color scheme from the Palette list (A). Move the switch (B) in the Ranges section to use the actual values, and enter 200,000,000 (C) and 400,000,000 (D) for the warning and OK ranges respectively. Click on OK to save the layer settings.

![](bbox=[0.35, 0.55, 0.65, 0.79])

The map will refresh to show a country-level view. Since the value of net revenue for the country is 815.83 million, the OK status is applied to the whole country because this value is more than the 400 million range set for the OK status in the previous step. In the map’s title area, you will see a hierarchical navigation icon (E) using which you can navigate up and down in the hierarchy. Click on this icon, and expand the list by clicking on Layer 1 (F). As you can