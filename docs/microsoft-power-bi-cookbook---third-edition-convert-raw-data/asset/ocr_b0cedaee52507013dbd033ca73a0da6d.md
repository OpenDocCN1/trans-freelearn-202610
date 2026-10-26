As per the query folding redesign example in this recipe, if the first step or Source step of the query is a native SQL statement, consider revising the M query steps to help the M engine form a SQL query (fold the M query). By default, any M transformation applied on top of a native SQL database query (via Value.NativeQuery) will not be folded to the source system. If native SQL queries are used, such as the stored procedure calls, the recommendation is to embed all query steps and transformations in the native SQL query itself. If this is not possible, embed the most resource-intensive operations in the stored procedure and pass filtering parameters from Power BI to the stored procedure to reduce the workload on the local M engine.

Note that it is possible to enable query folding on subsequent operations when using Value.NativeQuery through the use of an undocumented parameter. The format of using this version of Value.NativeQuery is as follows:

```
Value.NativeQuery( source, "query statement", null, [EnableFolding=true])
```

Here, the last (4th) parameter is a record that contains the parameter EnableFolding set to true. This could be used in the following manner at the start of our query:

```
let
    Source = Sql.Database("localhost", "AdventureWorksDW2022"),
    NativeQuery = Value.NativeQuery( Source, "SELECT * FROM dbo.DimCustomer",
null, [EnableFolding=true],
... // additional steps
    in
```

If there are required transformations or logic that are not supported by the query system for query folding, the recommendation is to move these steps to the very end of the query. For example, allow SQL Server to execute the filter, the derived columns, and other simple steps via query folding, and only then apply the complex steps locally on top of the SQL query result set.

Several common M functions are not supported by most relational database sources, such as Table.Distinct, which removes duplicate rows from tables, and Table.RemoveRowsWithErrors, which removes rows with errors from tables. If data sources are merged in the query, check their privacy settings (Data source settings | Edit Permissions...) to ensure that privacy is configured to allow folding, such as from an organizational source to a different organizational source.

In general, the following operations are not supported by query folding:

+   - Changing column data types
+   - Adding index columns
+   - Merging or appending queries from different sources
+   - Adding custom columns using Power Query functions that have no equivalent function in the data source