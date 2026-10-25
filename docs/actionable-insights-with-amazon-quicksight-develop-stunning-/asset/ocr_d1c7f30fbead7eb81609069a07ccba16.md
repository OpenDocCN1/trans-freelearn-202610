# # Working with ML insights

QuickSight has a special type of insight where the results are driven from ML-based computations. QuickSight supports two types of ML insights:

+   - Forecasting
+   - Anomaly detection

In the next section, we will learn how to configure each of these types of insights.

# # Working with forecasting insights

We learned how to add forecasting in a line graph visual. QuickSight also allows you to add forecasting as a narrative to display forecasted values. For example, how many miles is expected to be traveled between Manhattan and Queens on a specific date?

To better understand how to configure this type of visual, we will use our example _New York Taxi_ analysis to answer this question:

+   1. First, let’s create a new insight and select **Forecast ML-Powered Insight** from the drop-down list.

+   2. For this example, let’s assume that we want the user to choose the pick-up borough and the destination borough (drop-off borough). To achieve this, we will create two filter parameters (let’s name them `PickUpBoroughForecast` and `DropOffBoroughForecast`) with on-screen controls that allow the user to select the pick-up and drop-off boroughs, as we learned in _Chapter 4, Developing Visuals and Dashboards_. Make sure the filter applies only to the newly created forecast insight so that it is not affecting the other visuals in our analysis.

+   3. Now, let’s focus on the ML forecast visual. Let’s assume that we need to estimate the total miles driven between Manhattan and Queens on the 10 January, which is 10 days after our latest data point.

+   4. Select the `PickUp Time` value as the **Time** field and the `trip_distance(sum)` value as the **Value** field. Now, the forecasting visual should automatically display text like the following:
+   
+   ```
+   Total trip_distance is forecasted to be 4,546.560032279868 for Jan 14, 2021
+   ```

+   5. Now, we will need to customize the narrative to ensure it displays the text we need and the forecast time is what we expect.