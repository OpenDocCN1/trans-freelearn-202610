![](bbox=[0.17, 0.08, 0.83, 0.46])

Figure 9.5: The data model after creating a new relationship between the two tables

From a data modeling perspective, there is now a many-to-many relationship between the Internet Sales table and the Sales Reasons table via the bridge (the Internet Sales Reasons) table. Consequently, the Customer, Date, and Product dimensions also have a many-to-many relationship with the Sales Reason table.

To visualize the data, we need to create two new measures, Quantity Sold and Internet Sales Reasons, with the following DAX expressions:

```
Quantity Sold = SUM('Internet Sales'[OrderQuantity])
Sales Reasons Count = COUNTROWS('Internet Sales Reasons')
```

Let’s visualize the data and see how it works:

+   1. Put a Matrix visual on the reporting canvas.