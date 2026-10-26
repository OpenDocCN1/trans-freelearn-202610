13. Click Close & Apply in the ribbon of the Home tab to load the tables to the semantic model.

14. Create the single cross-filtering relationships shown in Figure 3.34; note that there are no relationships for the GrowthTiers or SubCatPlan tables:

| Active | From: Table (Column) | To: Table (Column) |
| --- | --- | --- |
| ☑ | Internet Sales (CustomerKey) | Customers (CustomerKey) |
| ☐ | Internet Sales (DueDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (OrderDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (ProductKey) | Products (ProductKey) |
| ☐ | Internet Sales (ShipDateKey) | Dates (DateKey) |

Figure 3.34: Model relationships

15. Switch to the Table view and hide the single column in the Calculations table.

16. Create the following three measures in the Calculations table:

```
Sales Current Year =
    CALCULATE(SUM('Internet Sales'[SalesAmount]),
        FILTER(ALL EXCEPT('Internet Sales','Customers'),
            YEAR('Internet Sales'[OrderDate]) = 2012 ) )
Sales Previous Year =
    CALCULATE(
        SUM('Internet Sales'[SalesAmount]),
        FILTER(ALL EXCEPT('Internet Sales','Customers'),
            YEAR('Internet Sales'[OrderDate]) = 2011 )
Sales Growth =
    DIVIDE([Sales Current Year] - [Sales Previous Year],
        [Sales Previous Year], BLANK() )
```

17. Right-click the Date table and choose Mark as date table using FullDateAlternateKey as the Date column.

# How to support virtual table relationships

To implement this recipe, use the following steps:

+   1. In Table view, create the Sales Growth Tier measure in the Calculations table:

```
Sales Growth Tier =
    VAR Growth = [Sales Growth]
    VAR Level1 =
        CALCULATE(MIN('GrowthTiers'[Max]), 'GrowthTiers'[GrowthTierKey] = 1)
    VAR Level2 =
```