![](bbox=[0.33, 0.067, 0.775, 0.535])

*Figure 2.26: Expanding DimGeography to include CountryRegionCode and DimSalesTerritory*

+   10. Now, expand the DimSalesTerritory column and only select the SalesTerritoryCountry column.

For additional details on performing these steps, see the *Importing Data* recipe in this chapter.

# How to apply multiple filters

To implement this recipe, take the following steps:

+   1. Right-click the Customers query, choose **Reference**, and then open the **Advanced Editor** window for this query. Use the `Table.SelectRows` function to apply the US query predicate and rename the query `United States Customers`. The finished query should appear the same as the following:

```
```text
let
    Source = Customers,
    USCustomers = Table.SelectRows(
        Source, each [CountryRegionCode] = "US")
in
    USCustomers
```
```