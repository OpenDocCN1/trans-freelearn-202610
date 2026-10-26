# There’s more...

The DAX TREATAS function was added in early 2017 and provides both simpler syntax and better performance than alternative virtual relationship methods involving INTERSECT or FILTER with a CONTAINS function parameter. We can use the TREATAS function to integrate a summary table, SubCatPlan, into our Power BI semantic model. This is useful when a business wants to filter plan data via the same Products and Dates tables they use regularly and to create actual versus plan reports. The grain of the SubCatPlan table is Product Subcategory by Calendar Year and Calendar Month; this is shown in Figure 3.37.

| Product Subcategory | Calendar Year | Calendar Month | Plan Amt |
|---|---|---|---|
| Mountain Bikes | 2013 | January | $313,596.6261 |
| Bottles and Cages | 2013 | January | $2,284.3156 |

Figure 3.37: Plan summary table for virtual relationships

Relationships from the SubCatPlan table to the Date and Product tables cannot be created directly given the many-to-many relationship. Each physical relationship in Power BI is based on a single column from each table, with one of the relationship columns uniquely identifying all the rows of its host table. This is consistent with SSAS Tabular and Power Pivot for Excel models.

To create a virtual table relationship with DAX, follow these steps:

+   1. Create the Subcat Plan Amt measure in the Calculations table. This measure filters the subcategory plan measure by the Products table and the Dates table:

```
Subcat Plan Amt =
    VAR ProductSubCats = VALUES('Products'[Product Subcategory])
    VAR DateTbl =
        SUMMARIZE('Dates','Dates'[CalendarYear],'Dates'[EnglishMonthName])
    RETURN
        CALCULATE(
            SUM('SubCatPlan'[Plan Amt]),
            TREATAS(ProductSubCats,'SubCatPlan'[Product Subcategory]),
            TREATAS(DateTbl,'SubCatPlan'[Calendar Year],'SubCatPlan'[Calendar Month]))
```

+   2. Create a Sales vs Plan measure:

```
Sales vs Plan = SUM('Internet Sales'[SalesAmount]) - [Subcat Plan Amt]
```

+   3. Create a Matrix visualization with the Product Subcategory column from the Products table as Rows, the EnglishMonthName column from the Dates table as Columns, and the Subcat Plan Amt measure, Sales vs Plan measure, and the SUM of the SalesAmount column from the Internet Sales table as Values.