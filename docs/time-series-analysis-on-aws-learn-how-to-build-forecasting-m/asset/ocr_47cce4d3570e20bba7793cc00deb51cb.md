+   - **Forecast key:** When you defined the schema for your target dataset at ingestion time (see *Chapter 3, Creating a Project and Ingesting Your Data*), you defined a target key named `item_id`. This is the key you can use to filter your forecast. Fill in `MAC000574` to request an energy consumption forecast for this particular household.

> **Note**
> 
> If you had other dimensions in your target time series data, they would also appear in the forecast key to let you query an aggregated forecast along the selected dimensions.

4. Click on the **Get Forecast** button at the bottom right of the form to visualize your forecast:

![](bbox=[0.116, 0.369, 0.85, 0.659])

Figure 6.8 – Predictions for household MAC000574

5. On this plot, you can visualize the last part of the historical data associated with `item_id` `MAC000574` (in gray on the left) in particular and the predictions for each quantile (or forecast type) you selected when you generated the forecast:

+   - The bottom line is the **p10 forecast**. If you hover your mouse over the first peak, you can read a value of **5.215**, meaning that there is a 10% chance that the actual value at this date will be below **5.215**.
+   - The middle line is the **p50 forecast**. If you hover your mouse over the first peak, you can read a value of **7.595**, meaning that there is a 50% chance that the actual energy consumption at this date will be below **7.595**.