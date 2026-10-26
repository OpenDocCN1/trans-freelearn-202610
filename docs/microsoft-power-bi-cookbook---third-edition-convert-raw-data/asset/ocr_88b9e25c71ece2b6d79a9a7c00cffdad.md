+   10. With the **Ctrl** key pressed, select the `ParentOrganizationKey`, `OrganizationName`, and `Index` columns, and then right-click one of these selected columns and choose **Group By....**

+   11. In the **Group By** dialog shown in *Figure 2.15*, under **New column name**, name the column `Amount` with **Operation** as `Sum` and choose the `Amount` column as shown:

![](bbox=[0.146, 0.159, 0.868, 0.555])

*Figure 2.15: Group By dialog*

+   12. Click the **OK** button.

+   13. Open **Advanced Editor** from the **Home** tab of the ribbon to review the expression created:

```
let
    Source = AdventureWorksDW2022,
    dbo_DimOrganization = Source{[Schema = "dbo", Item = "DimOrganization"]}[Data],
    #"Sorted Rows" = Table.Sort(
        dbo_DimOrganization,
        {{"CurrencyKey", Order.Descending}, {"OrganizationKey", Order.Ascending}}
    ),
    #"Added Index" = Table.AddIndexColumn(#"Sorted Rows", "Index", 1, 1, Int64.Type),
    #"Removed Other Columns" = Table.SelectColumns(
        #"Added Index",
        {"ParentOrganizationKey", "OrganizationName", "FactFinance", "Index"}
    )
```