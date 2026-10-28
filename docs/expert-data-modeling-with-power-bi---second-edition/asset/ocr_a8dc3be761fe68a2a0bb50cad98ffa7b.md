# Testing calculation groups

As we mentioned previously, calculation groups only work with explicit measures. So, we must create at least one explicit measure in our sample to make the calculation groups work. We created the following measures in the sample file:

A measure to calculate SalesAmount:

```
Total Sales = SUM('Internet Sales'[SalesAmount])
```

A measure to calculate OrderQuantity:

```
Quantity Ordered = SUM('Internet Sales'[OrderQuantity])
```

Let’s test the calculation group we created to see if it works as expected, as follows:

+   1. In Power BI Desktop, put a Matrix visual on the report page. Put the Year, Month, and Date columns from the Dates table into Rows.
+   2. Put the Time Calculations column from the time intelligence calculation group into Columns.
+   3. Put the Total Sales measure from the Internet Sales table into Values:

![](bbox=[0.18, 0.67, 0.82, 0.89])