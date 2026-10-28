aggregations can boost performance. We use the **FactInternetSales** table with the following aggregations:

+   - **FactInternetSales** row count
+   - **SalesAmount**
+   - **OrderQuantity**
+   - **TaxAmount**
+   - **Freight**

We need the preceding aggregations at the **EnglishProductName**, **CalendarYear**, and **EnglishMonthName** levels. We also require the **DimCustomer** table for customer analysis to demonstrate the performance when the aggregation isn’t hit by the queries.

To implement the proof-of-concept, we have to think about the aggregation approach that can meet the requirements. Let us go through it:

+   - Relationship-based aggregation: The business needs to analyze the aggregations at the year and month level. So, to implement the aggregation with the relationship-based approach, we need to create the aggregation table containing **ProductKey** and **DateKey** at the year and month level and all aggregation columns mentioned in the scenario. We also have to create an extra **Date** table containing the **DateKey** and other date attributes at the year and month levels. Then we create the relationship between the aggregation table, the new **Date** table, and the **DimProduct** table. It sounds a bit too much. So relationship-based aggregation is not a feasible approach.
+   - `GroupBy` aggregation: In this approach, we have to denormalize the **CalendarYear**, **EnglishMonthName**, and **EnglishProductName** attributes in the aggregation table, which is fine. We `GroupBy` the aggregation columns by these columns. But what if the user wants to analyze sales by a different product attribute? In that case, the aggregation table does not get hit, instead, the query runs against the