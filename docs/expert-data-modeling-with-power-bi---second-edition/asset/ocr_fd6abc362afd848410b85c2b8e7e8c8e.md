In fact, all relationships between the DirectQuery tables and the imported tables are limited relationships. We touched on limited relationships in *Chapter 9, Star Schema and Data Modeling Common Best Practices*, in the *Dealing with many-to-many relationships* section, where we discussed the two relationship evaluation types: regular and limited. As a quick reminder, the relationship between two tables is regular when both have the same storage mode, and the relationship between the two is not many-to-many. If we violate any of the two conditions, the relationship between tables is limited.

When a relationship is limited, **table expansion** does not happen. Table expansion happens on the tables of the **many** side of a **many-to-one** relationship or the tables with a **one-to-one** relationship. Table expansion means that the columns of the table on the **one** side of the relationship are reachable by the table on the **many** side. Look at *Figure 14.16*, for instance; you can imagine a wide **FactResellerSales** that also contains all columns from the **DimReseller** table. It is like reaching to the **DimReseller** columns from the **FactResellerSales** table using the `RELATED()` function.

That is why when the relationship is limited, the `RELATED()` function is not supported. When the relationship is limited, the filter propagation from the **one** side to the **many** side of the one-to-many relationship does not work as expected. Therefore, when Rachel opens the report, she sees all reseller sales and not only hers. To fix the issue, we add another RLS rule on the **FactResellerSales** table to enforce the filter on the **SalesTerritoryKey** column using the following DAX expression:

```
VAR __SalesReps =
CALCULATETABLE(
    VALUES(DimEmployee[EmployeeKey]),
    TREATAS( {USERPRINCIPALNAME()}, 
DimEmployee[EmailAddress] )
)
RETURN
FactResellerSales[EmployeeKey] in __SalesReps
```

In composite models with DirectQuery for Power BI datasets or Azure Analysis Services, the remote tables are not available to create RLS rules. But we can still run DAX expressions to filter the imported tables. For