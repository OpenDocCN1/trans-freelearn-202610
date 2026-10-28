
Our knowledge of Power Query and DAX is also an essential factor to consider. Some of us are more comfortable with one language than the other.

## Creating fact tables

Now that we have created all the dimensions, it is time to create a fact table that contains numeric values and the primary keys of the dimensions as foreign keys. The **Sales Base** and **Customer Base** data show that the **Sales Base** table holds many numeric values. Therefore, a fact table can be derived from the **Sales Base** table, which we name **Sales**, by following these steps:

- 1. **Reference** the **Sales Base** table and then rename the new table **Sales**.

We want to get **ProductKey** from the **Product** table. To do so, we can merge the **Sales** table with the **Product** table.

- 2. Click **Merge Queries**.
- 3. Select the **ProductCategory**, **ProductSubcategory**, and **Product** columns, respectively.
- 4. Select the **Product** table from the drop-down list.
- 5. Again, select the **ProductCategory**, **ProductSubcategory**, and **Product** columns.
- 6. Select **Left outer (all from first, matching from second)** from the **Join Kind** drop-down list.
- 7. Click **OK**:

![](img/5d2bce74fd6fceebed14fd1488bf7088_350_0.png)

Figure 6.37: Merging the Sales table with the Product table

- 8. The preceding steps create a new structured column named Product.1 and a new transformation step in Applied Steps. The default name for this step is Merged Queries. Rename it Merged Sales with Product.
- 9. Expand the Product.1 column.
- 10. Check the ProductKey column (keep the rest unchecked).
- 11. Uncheck the Use original column name as prefix option.
- 12. Click OK:

![](img/5d2bce74fd6fceebed14fd1488bf7088_350_1.png)

Figure 6.38: Expanding the Product.1 structured column

Now, we need to get **SalesDemographicKey** from the **Sales Demographic** table. The columns that make a unique identifier for each row in the **Sales Demographic** table are **SalesCountry**, **City**, **StateProvinceName**, and **PostalCode**. However, the **Sales** table does not contain all those columns. Besides, the **Sales Demographic** dimension is derived from the **Customer Base** table. Therefore, we have to merge the **Sales** table with the **Customer Base** table via the **CustomerKey** column and then merge it again with the **Sales Demographic** table to reach **SalesDemographicKey**.

- 13. From the **Applied Steps**, rename **Expanded Product.1** to **Expanded Product**.
- 14. Click **Merge Queries** again.
- 15. Select the **CustomerKey** column under the **Sales** section.
- 16. Select **Customer Base** from the drop-down list.
- 17. Select **CustomerKey**.
- 18. Select **Left Outer** for **Join Kind**.
- 19. Click **OK**:

![](img/5d2bce74fd6fceebed14fd1488bf7088_351_0.png)

Figure 6.39: Merging the Sales table with Customer Base

This creates a new structured column named **Customer Base**. It also creates a new transformation step in **Applied Steps** named **Merged Queries**.

- 20. Rename this step **Merged Sales with Customer Base**.
- 21. Expand the **Customer Base** structured column.
- 22. Keep the **SalesCountry**, **City**, **StateProvinceName**, and **PostalCode** columns checked and uncheck the rest.
- 23. Uncheck the **Use original column name as prefix** option.
- 24. Click **OK**:

![](img/5d2bce74fd6fceebed14fd1488bf7088_352_0.png)

Figure 6.40: Expanding the Customer Base structured column

Now, we must merge the results with the **Sales Demographic** table and get the **SalesDemographicKey**.

- 25. After clicking **Merge Queries** again, select the **SalesCountry**, **City**, **StateProvinceName**, and **PostalCode** columns.
- 26. Select the **Sales Demographic** table from the dropdown.
- 33. Uncheck the Use original column name as prefix option.
- 34. Click OK:

![](img/5d2bce74fd6fceebed14fd1488bf7088_354_0.png)

Figure 6.42: Merging Sales with Sales Demographic

So far, we have added SalesDemographicKey to the Sales table. Let us look at the columns in the Sales table with either Date or DateTime data types. The Sales table has three columns with the DateTime data type. Looking closer at the data shows that OrderDate is the only one with the DateTime data type, while the other two represent Date values (the Time part of all values is 12:00:00 AM). Therefore, it is better to convert the data type of those columns into Date. The OrderDate column represents both Date and Time. The remaining part is to split the Date and Time values into separate columns: Order Date and Order Time. These columns will be used in the data model relationships. Follow these steps to do so:

- 35. Select the OrderDate column.
- 36. Click the Split Column button from the Home tab of the ribbon.
- 37. Click By Delimiter.
- 38. Select Space as the delimiter.

Power Query Editor automatically adds a **Changed Type** step if the **Type Detection** setting is set to detect the data types. Keep this step. Now, we need to change the data type of the **DueDate** and **ShipDate** columns from `DateTime` to `Date`.

- 43. Click the **Changed Type** step from the **Applied Step** pane.
- 44. Select both **DueDate** and **ShipDate**.
- 45. Click the **Data Type** drop-down button from the **Home** tab.
- 46. Select **Date**:

![](img/5d2bce74fd6fceebed14fd1488bf7088_356_0.png)

Figure 6.45: Changing the DueDate and ShipDate data types

So far, we have added all the key columns from the dimensions to the fact table, which will be used to create the relationships between the dimensions and the fact table in the data model layer. The only remaining piece of the puzzle is to clean up the **Sales** table by removing all the unnecessary columns.

- 47. Click the **Choose Column** button from the **Home** tab.
- 48. Keep the following columns checked and uncheck the rest; that is, **CustomerKey**, **SalesOrderNumber**, **SalesOrderLineNumber**, **OrderQuantity**, **ExtendedAmount**, **TotalProductCost**, **SalesAmount**, **TaxAmt**, **Freight**, **Order Date**, **Order Time**, **DueDate**, **ShipDate**, **Currency**, and **ProductKey**.
- 49. Click OK:

![](img/5d2bce74fd6fceebed14fd1488bf7088_357_0.png)

## Figure 6.46: Removing unnecessary columns from the Fact table

The results of the preceding transformation steps show that the Sales fact table contains the following:

- The OrderQuantity, ExtendedAmount, TotalProductCost, SalesAmount, TaxAmt, and Freight columns, which are facts.
- CustomerKey, Order Date, Order Time, DueDate, ShipDate, ProductKey, and SalesDemographicKey are foreign keys that will be used in the data modeling layer to create relationships between the Sales table and its dimensions.
- SalesOrderNumber, SalesOrderLineNumber, and Currency are degenerated dimensions.

We have now successfully prepared the data supporting the Star Schema approach. We now apply the changes to load the data into the data model.

## Summary

In this chapter, we prepared the data in a Star Schema, which has been optimized for data analysis and reporting purposes on top of a flat data structure. We identified potential dimensions and discussed the reasons for creating or not creating separate dimension tables. We then went through the transformation steps to create the justified dimension tables. Finally, we added all the dimension key columns to the fact table and removed all the unnecessary columns, which gave us a tidy fact table that only contained all the necessary columns.

The next chapter covers an exciting and rather important topic: *Data preparation common best practices*. By following these best practices, we can avoid a lot of reworks and maintenance costs.

## Join us on Discord!

Join The Big Data and Analytics Community on the Packt Discord Server!

Hang out with 558 other members and enjoy free voice and text chat.

https://packt.link/ips2H

![](img/5d2bce74fd6fceebed14fd1488bf7088_358_0.png)
