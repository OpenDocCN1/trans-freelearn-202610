between the fact and dimension tables. Therefore, we create relationships between the aggregation table and its dimensions.

+   - **GroupBy-based aggregation:** In this approach, the data source is big data models avoiding relationships between large tables, so the dimension attributes are denormalized in the fact table. In such scenarios, we implement aggregations using GroupBy columns.
+   - **Hybrid aggregation:** In this approach, regardless of the data sources’ architecture, we denormalize some dimension attributes in the aggregation table. The aggregation table also has relationships with other dimensions. Therefore, we have a mixture of GroupBy and relationship-based aggregation in a single implementation.

With that in mind, the following steps explain the process of implementing user-defined aggregations in Power BI Desktop:

+   1. Create the aggregation table.
+   2. Load all the necessary tables in DirectQuery mode.
+   3. Create relationships between the aggregation table and the dimensions for the relationship-based and hybrid aggregations.
+   4. Set the storage model of the aggregation and its related dimensions.
+   5. Configure **Manage aggregation**.
+   6. Test the aggregation to make sure our queries hit the aggregation.

The preceding process may vary depending on our data platform architecture, but the principles remain the same.

Let us continue with a scenario. The AdventureWorks organization has an existing *Sales Analysis* solution in Power BI that doesn’t perform well. The business complains that the automatic data refresh on the Power BI service constantly fails by getting a time-out error. The organization has a data warehouse in SQL Server named AdventureWorksDW2019. The existing solution in Power BI contains a large fact table, **FactInternetSales**, in DirectQuery mode. We want to implement a proof-of-concept to show how