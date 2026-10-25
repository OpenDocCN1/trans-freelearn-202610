# # Visualizing time series

It is common for BI applications to visualize how a metric is changing over time. Time series data is usually represented with line charts. Time series visualizations need to ensure that temporal fields are stored as the `Date` field type in QuickSight. This will enable time-based aggregations; for example, day, week, month, quarter, or year.

In this section, we will use a simple line chart to create our first time series visualization. Let’s assume that we want to visualize the total amount of taxi fares over time so that we can observe how the fare amount is distributed over time.

A line chart requires you to define the following:

+   - **X axis**: In time series, this will be our temporal field.

+   - **Value**: The value you want to measure over time.

+   - Optionally, the **Color** field: When you want to visualize multiple categories, you can use the `Color` field. This will result in multiple lines on the same line chart.

For our example, you will need to pick `tpep_dropoff_datetime` as the x-axis and `total_amount (Sum)` as the value. This dataset is only for 3 months; therefore, a daily aggregate is likely to have enough data points for a meaningful data visualization. You will need to ensure that the daily aggregation is chosen. To do that, select the x-axis column and expand the drop-down menu, as shown in the following screenshot:

![](bbox=[0.2617, 0.5371, 0.707, 0.6898])

Figure 4.10 – Time-based aggregations