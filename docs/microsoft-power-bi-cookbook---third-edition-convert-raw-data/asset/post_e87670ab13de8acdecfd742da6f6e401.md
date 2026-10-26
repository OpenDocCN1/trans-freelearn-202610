
```
"CountryRegionName", "PostalCode"}
)
in
#"Expanded DimGeography"
```

+   8. Separate the dimension queries, Customers, Departments, and Dates into a Dimensions query group and the General Ledger query into a query group called Facts.
+   9. Create a Blank Query called Calculations in the Other Queries group.
+   10. Choose Close & Apply on the ribbon of the Home tab to load the tables to the semantic model.
+   11. Create the relationships shown in Figure 3.11, using the Manage relationships dialog box.

| Active | From: Table (Column) | To: Table (Column) |
| --- | --- | --- |
| ☑ | General Ledger (DateKey) | Dates (DateKey) |
| ☑ | General Ledger (DepartmentGroupKey) | Departments (DepartmentGroupKey) |

Figure 3.11: Relationships

## How to configure model metadata

To implement this recipe, use the following steps:

+   1. Select the Table view.
+   2. Use the Data pane on the right to navigate to the Dates table and select the FullDateAlternateKey column.
+   3. Use the Format drop-down in the ribbon of the Column tools tab to change the format to 3/14/2001 (Short Date) or some other format.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_153_0.png)

Figure 3.12: Column formatting

4. Right-click the Calculations table and click New measure to create a measure with the code:

```
Finance = SUM('General Ledger'[Amount])
```

5. Select the Finance measure and change the format to Currency using the Format option in the Measure tools tab.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_154_0.png)

Figure 3.13: Measure formatting

6. Right-click on the Calculations column in the Calculations table and choose Hide in report view.

7. While still in the Table view, select the StateProvinceName column in the Customers table, and from the Column tools tab change the Data category to State or Province.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_154_1.png)

Figure 3.14: Data category selection

8. Repeat this procedure, setting the City column to City, the CountryRegionName column to Country, and the PostalCode column to Postal code. Note that a globe icon appears next to all of these columns in the Data pane.

9. Switch to Report view and create a Map visualization with CountryRegionName and PostalCode in the Location field well and CustomerKey in the Bubble size field well. You can drill down to the postal code level and zoom in to investigate how many customers are in particular areas.

14. On the Column tools tab, change the default Summarization from Count to Don't summarize.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_156_0.png)

Figure 3.17: Default Summarization for Columns

## How it works

The metadata settings for Formatting, Data category, Summarization, and Sort by column are all available from the Column tools or Measure tools tabs, whether you are in the Table view or Report view. Note that Summarization and Sort by column are not available for Measures tools.

Formatting decisions should consider the impact of precision on visualizations. Fewer decimal places and more abbreviated date formats consume less space in reports and are easier to visually comprehend on dashboards. To understand how formatting works, consider a date data type. Date data types are actually stored as decimal numbers in the model engine. The integer portion of the number is the number of days since December 30, 1899. The decimal portion is the fraction of a day, with hours being 1/24 or 0.04166, minutes being 1/24/60 or 0.0006944, and seconds being 1/24/60/60 or 0.0000115740. Thus, the display formats for dates simply visually change how the base data is displayed to the user while keeping the underlying data unchanged. The user may see 1/1/1900 00:00:00 displayed on their screen, but internally in the semantic model, this is simply stored as 2.

Assigning a location Data category to columns assists Bing Maps with geolocation and changes the default visualization for such columns to the Map visualization. The Web URL is important for such things as mailto email address links and any URL links exposed in reports. When the column is used in Table visualizations, email and link icons are displayed, respectively. Similarly, assigning an Image URL to a column displays the actual image when using Table visualizations. The Barcode data category can be useful for mobile applications.

When considering the use of Sort by columns, identify columns requiring custom sorting. Calendar text columns such as Month and Weekday are the most common candidates. Other columns may represent an organizational structure or entity hierarchy, such as general ledger accounts. The Sort by column sort must contain only one distinct value for each value in the column to be sorted. It is recommended to embed the sorting logic as deep into the BI architecture as possible. For example, the sorting column could be added to a dimension table in a data warehouse or the SQL view used to load the semantic model. If these options are not available, M query transformations are recommended over DAX calculated columns.

Text and date data type columns are set to Don't summarize by default when first loaded to Power BI models. These data types have default summarizations of Count and Count (Distinct). In addition, the Report view provides the following aggregation options when using these data types in visualizations:

+   - Text: First, Last, Count (Distinct), or Count

+   14. Switch to the Data view and hide the single columns in the Reseller Measures, Pricing Measures, Customer Measures, and Currency Measures tables.
+   15. Right-click on the Date table and choose Mark as date table using FullDateAlternateKey as the Date column.

## How to enrich a model with DAX measures

To implement this recipe, use the following steps:

+   1. While in the Table view, create the following eight DAX measures in the Reseller Measures table:

```
Reseller Discount Amount = SUM('Reseller Sales'[DiscountAmount])
Reseller Gross Sales =
    SUMX('Reseller Sales',
        'Reseller Sales'[UnitPrice] * 'Reseller Sales'[OrderQuantity])
Reseller Net Sales =
    CALCULATE( [Reseller Gross Sales] - [Reseller Discount Amount],
        'Reseller Sales'[ShipDate] <> DATEVALUE("12/31/2099") )
Reseller Product Cost =
    SUMX('Reseller Sales',
        'Reseller Sales'[OrderQuantity]*'Reseller Sales'[ProductStandardCost])
Reseller Gross Margin % =
    DIVIDE([Reseller Gross Sales] - [Reseller Product Cost],
        [Reseller Gross Sales], 0)
Reseller Margin % =
    DIVIDE([Reseller Net Sales] - [Reseller Product Cost],
        [Reseller Net Sales], 0)
Reseller Count of Products Sold =
    CALCULATE(DISTINCTCOUNT('Products'[ProductAlternateKey]),
        'Reseller Sales')
Reseller Count of Sales Orders = DISTINCTCOUNT('Reseller Sales'[SalesOrderNumber])
```

+   2. Create the following seven DAX measures in the Pricing Measures table:

```
Effective Unit Price =
    DIVIDE(SUM('Reseller Sales'[ExtendedAmount]),
        SUM('Reseller Sales'[OrderQuantity]) )
25th Percentile Unit Price = PERCENTILE.INC('Reseller Sales'[UnitPrice],.25)
75th Percentile Unit Price = PERCENTILE.INC('Reseller Sales'[UnitPrice],.75)
Maximum Unit Price = MAX('Reseller Sales'[UnitPrice])
```

```
Median Unit Price = MEDIAN('Reseller Sales'[UnitPrice])
Minimum Unit Price = MIN('Reseller Sales'[UnitPrice])
Range of Unit Prices = [Maximum Unit Price] - [Minimum Unit Price]
```

3. Create the following three DAX measures in the Customer Measures table:

```
Count of Accessory But Not Bike Customers =
VAR BikeCustomers =
    SUMMARIZE(
        CALCULATETABLE('Internet Sales','Products'[Product Category] = "Bikes"),
        'Customers'[CustomerAlternateKey])
VAR AccessoryCustomers =
    SUMMARIZE(
        CALCULATETABLE('Internet Sales',
            'Products'[Product Category] = "Accessories"),
        'Customers'[CustomerAlternateKey])
VAR Result = CALCULATE(
    DISTINCTCOUNT('Customers'[CustomerAlternateKey]),
    EXCEPT(AccessoryCustomers,BikeCustomers) )
RETURN
    Result
Count of Bike Only Customers =
VAR BikeCustomers =
    SUMMARIZE(
        CALCULATETABLE('Internet Sales','Products'[Product Category] = "Bikes"),
        'Customers'[CustomerAlternateKey]
    )
VAR ClothesAndAccessoryCustomers =
    SUMMARIZE(
        CALCULATETABLE('Internet Sales',
            'Products'[Product Category] IN {"Accessories","Clothing"}
        ), 'Customers'[CustomerAlternateKey])
VAR Result = CALCULATE(
    DISTINCTCOUNT('Customers'[CustomerAlternateKey]),
    EXCEPT(BikeCustomers,ClothesAndAccessoryCustomers) )
RETURN
    Result
Count of Last Year Customers ex Current Year =
VAR CurrentYear = 2012
VAR LastYear = CurrentYear - 1
```
