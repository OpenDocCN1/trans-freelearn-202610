# How it works

Setting data categories for columns (*steps 1 and 2*) assists in geocoding location-based data. Similarly, setting **Location type** (*step 6*) provides a geographic hint that significantly improves the accuracy of the plotted points returned by the Esri service. Note that locations can also be represented as boundaries, such as states or postal codes. Almost all the advanced report development features provided by ArcGIS are exposed in the Power BI visualization.

If latitude and longitude columns are already available for the dimension to be mapped, then these columns should be used in the ArcGIS visual instead of the **Location** field well. Providing latitude and longitude source data significantly improves performance, as it eliminates the need for Esri to compute these values.

# There’s more…

Microsoft has recently added a powerful, native map visual to Power BI, the Azure map visual. While not as advanced as the ArcGIS map visual, the Azure map visual is significantly more powerful and flexible than the Map, Filled map, and Shape map visuals. To see the Azure map visual in action, do the following:

+   1. Make a duplicate of Page 1 by right-clicking Page 1 and choosing **Duplicate page**.
+   2. Select the visual and change the visual to an **Azure map** visual.
+   3. Remove **Full Address** from the Location field well and replace it with the **Postal Code** column.
+   4. Adjust the **Filters** pane to include the Northeast, Northwest, Southeast, and Southwest `SalesTerritoryRegion` values.
+   5. Configure the legend to be `SalesTerritoryRegion`, **Size** to be the Count of `CustomerKey`, and **Tooltips** to be the Sum of `YearlyIncome`, as shown in *Figure 9.13*: