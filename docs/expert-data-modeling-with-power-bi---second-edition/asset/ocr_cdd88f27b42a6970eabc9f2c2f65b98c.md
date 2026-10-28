2. Create the following relationships between the Reseller_Sales and Dates tables. Keep the first relationship active:

+   a. Reseller_Sales(OrderDateKey) => Dates(DateKey)
+   b. Reseller_Sales(DueDateKey) => Dates(DateKey)
+   c. Reseller_Sales(ShipDateKey) => Dates(DateKey):

![](bbox=[0.175, 0.23, 0.825, 0.485])

Figure 10.55: Creating relationships between Reseller_Sales and Dates

We now need to create new measures, one for each role, and name the measures appropriately so they resemble the roles.

Keep in mind that the active relationship between the two tables remains the primary relationship. Therefore, all the measures that calculate values on top of the Reseller_Sales and Dates tables use the active relationship. So, it is essential to keep a relationship active that makes the most sense to the business. In our scenario, Order Date is the most important date for the business. Therefore, we will keep the Reseller_Sales(OrderDateKey) => Dates(DateKey) relationship active.

We can create the Reseller Sales by Order Date measure using the following DAX expression: