Given this limitation and the additional complexity the M-based transforms would add to the solution, it is recommended that you embed all the necessary logic and transformations in the source relational layer. Ideally, the base tables in the source database themselves would reflect these needs. As a secondary option, a layer of views can be created to support the DirectQuery model.

If the database objects themselves cannot be revised, the Value.NativeQuery M function can be used to directly pass the SQL statement from Power BI Desktop to the source database. For example:

```
let
    Source = Sql.Database("localhost", "AdventureWorksDW2022"),
    Query  = Value.NativeQuery(Source, "select * from DimCustomer", null, [EnableFolding = true])
in
    Query
```

As report visualizations are refreshed or interacted with in Power BI, the DAX queries from each visualization are translated into SQL statements, utilizing the source SQL statements to return the results. Be aware that Power BI does cache query results with DirectQuery models. Therefore, when accessing a recently utilized visual, a local cache can be used rather than a new query sent to the source.

The SQL statements passed from Power BI to the DirectQuery data source include all columns from the tables referenced by the visual. For example, a Power BI visual with SalesAmount from the FactResellerSales table, grouped by ResellerName from DimReseller, would result in a SQL statement that selects the columns from both tables and implements the join defined in the model. However, as the SQL statement passed embeds these source views as derived tables, the relational engine generates a query plan that only scans the required columns to support the join and aggregation.

# There’s more…

The performance and scalability of DirectQuery models are primarily driven by the relational data source. A denormalized star schema with referential integrity and a system that is isolated from OLTP workloads is recommended if near real-time visibility is not required. Additionally, in-memory and columnar features available to supported DirectQuery sources are recommended for reporting and analytical queries.