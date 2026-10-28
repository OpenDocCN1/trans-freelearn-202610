To do so, we add a new column to get textual values from TypeName. We name the new column Datatypes, and the expression is the following:

```
Table.Schema(
    Table.FromValue([Value])
)[TypeName]{0}
```

The following screenshot shows the results:

![](bbox=[0.17, 0.28, 0.83, 0.64])

Figure 7.21: Power Query has only one numeric type, which is Number.Type

As the results show, all numeric values are of type Number.Type and how we present them in the Power Query Editor with different facets does not affect how the Power Query mashup engine treats those types. Here is the critical point: what happens after we load the data into the data model? Power BI uses the xVelocity in-memory data processing engine to process the data. The xVelocity engine uses columnstore indexing technology that compresses the data based on the cardinality of the column, which brings us to a critical point: although the Power Query engine treats all the numeric