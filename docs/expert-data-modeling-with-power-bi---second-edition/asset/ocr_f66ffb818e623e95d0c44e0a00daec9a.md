49. Click OK:

![](bbox=[0.175, 0.115, 0.825, 0.37])

# Figure 6.46: Removing unnecessary columns from the Fact table

The results of the preceding transformation steps show that the Sales fact table contains the following:

+   - The OrderQuantity, ExtendedAmount, TotalProductCost, SalesAmount, TaxAmt, and Freight columns, which are facts.
+   - CustomerKey, Order Date, Order Time, DueDate, ShipDate, ProductKey, and SalesDemographicKey are foreign keys that will be used in the data modeling layer to create relationships between the Sales table and its dimensions.
+   - SalesOrderNumber, SalesOrderLineNumber, and Currency are degenerated dimensions.

We have now successfully prepared the data supporting the Star Schema approach. We now apply the changes to load the data into the data model.