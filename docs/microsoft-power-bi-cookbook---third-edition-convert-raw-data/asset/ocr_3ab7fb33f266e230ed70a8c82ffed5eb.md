![](bbox=[0.133, 0.067, 0.915, 0.394])

*Figure 2.35: Added conditional column M code*

# See also

+   - `Table.AddColumn`: http://bit.ly/3vGJZ6b
+   - `Table.SelectColumns`: http://bit.ly/38Qk7Lt
+   - DateTime functions: http://bit.ly/3tPtKlJ
+   - Add columns from examples: http://bit.ly/3eWTLv6

# Combining and Merging Queries

The full power of Power BI’s querying capabilities is in the integration of distinct queries representing different data sources, via its merge and append transformations. Retrieval processes that consolidate files from multiple network locations or integrate data from multiple data sources can be developed efficiently and securely. Additionally, the same join types and data transformation patterns that SQL and ETL developers are familiar with can be achieved with the M language. This recipe provides examples of combining sources into a single query and leveraging the table join functions of M to support common transformation scenarios.

# Getting ready

To follow along with this recipe, you can use the **Merge Queries** and **Append Queries** icons on the **Home** tab of Power Query Editor to generate the join expressions used in this recipe. However, as joining queries is fundamental to the retrieval process, it is recommended to learn how to use the `Table.Join`, `Table.NestedJoin`, and `Table.Combine` functions.

To get ready for this recipe, import the `DimCustomer` and `FactInternetSales` tables from the AdventureWorksDW2022 database by doing the following: