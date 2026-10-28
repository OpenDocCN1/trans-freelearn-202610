data/power-bi-data-sources?WT.mc_id=DP-MVP-5003466.

While the **Agg Awareness** feature takes care of the complexities of switching the calculations between the aggregate table and the detail table depending on the user interactions with the reports, not all data sources support DirectQuery. Therefore, we might not use the **Agg Awareness** feature. Still, it doesn’t mean we cannot implement the aggregation technique. The next few sections discuss two different ways of implementing aggregation tables.

The first method is implementing aggregation tables for non-DirectQuery data sources, such as Excel files. Therefore, not only do we need to implement the aggregation table, but also, we must take care of the control mechanism to switch between the aggregation and the detail table when the user drills down to a different grain of data.

The second method uses the **Manage aggregations** feature for the data sources supporting DirectQuery.

# Implementing aggregations for non-DirectQuery data sources

In many real-world scenarios, the data sources may not support DirectQuery mode. Therefore, we cannot use the **Agg Awareness (Manage aggregations feature)** in Power BI Desktop. However, we can manually implement aggregations by going through the following process:

+   1. Summarize the table at a lower grain.
+   2. Create relationships between the new summary table and the dimensions at the summary grain.
+   3. Create the desired measures (note that the new measures work at the summary grain).