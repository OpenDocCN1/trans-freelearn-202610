of view. But when we reference a query, we are referencing the final results of the query. Therefore, we do not get the transformation steps in the new query (the referencing query). Referencing a query is a common way to break down the transformation activities. This is the preferred way of preparing the data for most **Extract, Transformation, and Load (ETL)** experts and data warehousing professionals. In that sense, we can do the following:

+   - We have base queries connected to the source system that resembles the **Extract** part of the ETL process.
+   - We reference the base queries and go through the **Transformation** steps of the ETL process.
+   - Finally, we reference the transformation queries to prepare our Star Schema. This is the **Load** part of the ETL process.

In the preceding approach, we unload all the queries for the first two points as they are our transformation steps, so we only enable data loading for the queries of the **Load** part (the last point). Duplicating and referencing a query is simple; right-click a desired query from the **Queries** pane and click either **Duplicate** or **Reference**, as shown in the following screenshot:

![](bbox=[0.17, 0.58, 0.83, 0.86])

Figure 5.45: Referencing or duplicating a query