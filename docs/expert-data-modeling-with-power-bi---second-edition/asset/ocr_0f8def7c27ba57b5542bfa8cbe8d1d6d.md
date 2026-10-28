Figure 5.9: Checking a column’s data type in Power BI Desktop

Looking at the data types of the OrderDateTime column in Internet Sales and the Date column in the Date table reveals that the data type of the Date column is Date, while the data type of the OrderDateTime column is Date/Time. Those data types are compatible from a data modeling perspective. Nevertheless, the data in the OrderDateTime column has a time element. Therefore, the only matching values from the OrderDateTime column are those with a time element of 12:00:00 AM. To ensure this is right, we can do a quick test.

We can put a table on the reporting canvas showing the Date column from the Date table and the OrderDateTime column from the Internet Sales table side by side.

The following screenshot shows the results:

![](bbox=[0.178, 0.084, 0.822, 0.301])

![](bbox=[0.178, 0.645, 0.822, 0.802])