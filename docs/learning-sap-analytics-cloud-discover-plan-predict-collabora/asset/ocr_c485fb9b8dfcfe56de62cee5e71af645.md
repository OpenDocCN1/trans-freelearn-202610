# Intersecting data example

The scatter plot chart you created in the previous exercise displayed event data for 16 cities. However, the number of cities in the BestRun_Demo model is 18, as shown in the following screenshot, which is taken from the Data View panel. There is no event data in the Planned_Events model for the two highlighted cities: Sacramento and San Jose. This scenario will allow us to test the Intersecting data only join type.

Execute the following set of steps to test the effect of selecting the Intersecting data only join type:

![](bbox=[0.28, 0.32, 0.72, 0.75])

+   48. Add a blank chart by clicking on the chart icon on the Insert toolbar.
+   49. Select the BestRun_Demo model for Data Source.
+   50. Select the area chart from the Trend category.
+   51. Click on the Add Measure link, and select Net Revenue.
+   52. Click on the Add Dimension link in the Dimensions section, and select Location.
+   53. In Data Source, click on the Link dimensions icon, and select the Planned_Events model to select a second measure from this model.
+   54. In the Measures section, click on the Add Measure link, and select No of Events for 2015, as shown in the following screenshot: