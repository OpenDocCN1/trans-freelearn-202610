
![](img/5d2bce74fd6fceebed14fd1488bf7088_331_0.png)

Figure 6.17: Merged column results

Now, we must remove the Merged Column step from Applied Steps.

## Product

The Product dimension is the most obvious one, with three descriptive columns. We derive the Product dimension from the Sales table by referencing the Sales table. Then, we remove other columns to keep the Product, ProductSubcategory, and ProductCategory columns. As the next step, we must remove the duplicates from the Product table. Moreover, we need to generate a ProductKey column as the primary key of the Product table, which we will use in the data model later. Next, we merge the Sales table with the Product table to get a ProductKey in the Sales table. We also rename the columns to more user-friendly versions: ProductSubcategory to Product Subcategory and ProductCategory to Product Category.

> **Note**
>
> Moving forward, we reference the Sales table many times. Therefore, we rename the Sales table Sales Base.

The following steps show how to implement the preceding process in Power Query Editor:

- 1. Rename the **Sales** table **Sales Base**:

  ![](img/5d2bce74fd6fceebed14fd1488bf7088_332_0.png)

  Figure 6.18: Renaming the Sales table Sales Base

- 2. **Reference** the **Sales Base** table:

  ![](img/5d2bce74fd6fceebed14fd1488bf7088_332_1.png)

  Figure 6.19: Referencing the Sales Base table

- 3. **Rename** the referencing table **Product**.

- 4. Select the **ProductCategory**, **ProductSubcategory**, and **Product** columns, respectively.

  > **Note**
  >
  > Power Query places the sequence of the columns by order of our selection in *Step 4*.

- 5. Right-click one of the selected columns and click **Remove Other Columns**:

  Figure 6.20: The Remove Other Columns option

- 6. Click the table button from the top-left corner of the Data view pane.

- 7. Click Remove Duplicates:

  Figure 6.21: Removing duplicates from all rows

Remove Duplicates from the Power Query UI uses the Table.Distinct(table as table) function in the background. The Table.Distinct() function is equivalent to the Table.AddKey(table as table, columns as list, isPrimary as logical) function when the isPrimary operand is true. The Table.AddKey() function defines a column or multiple columns as the primary key columns for a table. But after loading the data into the data model, the xVelocity engine does not support composite primary keys.

With that, we have created the **Product** dimension.

## Currency

The **Currency** column in the **Sales Base** table holds the currency description for each transaction. So, by definition, it is a dimension. Let us raise the *Why* question here. Why do we need to create a separate table for **Currency**? We need to look at the situation more thoroughly to answer this question. The **column distribution** box, when set to work based on the entire dataset, shows that the **Currency** column’s cardinality is low, with only 6 distinct values, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_335_0.png)

Figure 6.24: Column Distribution for the Currency column over the entire dataset

Since the `Columnstore` indexing in the **xVelocity** engine provides better data compression and performance over low cardinality columns, we can expect minimal or no sensible performance or storage gains by creating a new dimension table for **Currency** in our scenario. Besides, we do have other attributes providing more descriptions for currencies. Lastly, **Currency** does not have any meaningful linkages to other dimensions. As a result, we can keep the **Currency** column as a **degenerate dimension** in the fact table.

Let us move forward and look at the other potential dimensions.

## Customer

We can derive the **Customer** table from the original **Customer** table from the source. To do this, we’ll rename the original **Customer** table **Customer Base**.

Let us look at the **Sales Base** table to see how each row relates to the **Customer Base** table. The **Sales Base** table has a **CustomerKey** column. We set **Column profiling based on entire data set** to have a more realistic view of **CustomerKey**. The **Column Quality Box** of **CustomerKey** in the **Sales Base** table reveals that a customer key exists for every single sales transaction in the **Sales Base** table (**0% Empty**). The following image shows the **Column Quality Box** for the **CustomerKey** column in the **Sales Base** table:

![](img/5d2bce74fd6fceebed14fd1488bf7088_336_0.png)

Figure 6.25: The Column Quality information shows 0% Empty for CustomerKey

Therefore, every row of the **Sales Base** table can relate to the descriptive data in the **Customer Base** table. To ensure that we can create the relationship between the two tables, we must ensure that the data type of **CustomerKey** column in both the **Sales Base** and **Customer Base** tables match. We also want the **CustomerKey** in the **Customer Base** table to be the primary key, meaning that the number of **distinct** and **unique** values must be the same. The following image shows that the data types of the **CustomerKey** column in both tables match and that **CustomerKey** in the **Customer Base** table is the primary key:

![](img/5d2bce74fd6fceebed14fd1488bf7088_337_0.png)

Figure 6.26: The data type of CustomerKey matches in both tables, and it is the primary key of the Customer Base table

Now that we have enough evidence that the Customer table is a dimension, let us create it by following these steps:

- 1. Reference the Customer Base table:

  ![](img/5d2bce74fd6fceebed14fd1488bf7088_337_1.png)

  Figure 6.27: Referencing the Customer Base table

- 2. Rename the referencing table to Customer.

We need to keep the following columns by removing the other columns; that is, CustomerKey, Title, FirstName, MiddleName, LastName, NameStyle, BirthDate, MaritalStatus, Suffix, Gender, EmailAddress, YearlyIncome, TotalChildren, NumberChildrenAtHome, Education, Occupation, HouseOwnerFlag, NumberCarsOwned, AddressLine1, AddressLine2, Phone, DateFirstPurchase, and CommuteDistance.

- 3. The simplest way to do so is to click **Choose Columns** from the **Home** tab.
- 4. Keep the preceding columns and deselect the rest.
- 5. Click **OK**:

  ![](img/5d2bce74fd6fceebed14fd1488bf7088_338_0.png)

  *Figure 6.28: Removing the unnecessary columns*

With that, we created the **Customer** dimension. Since the **Customer** dimension already has a primary key, **CustomerKey**, we do not need to take any more actions.

In the next section, we create a separate dimension containing geography-related data by referencing the **Customer Base** table; therefore, we do not need any geography-related columns in the **Customer** dimension.

need to remove one of them by double-clicking the **Remove Other Columns** step and unchecking the **CountryRegionName** column.

The next step is to remove the duplicate rows, which avoids getting any duplicate rows in the dimension in the future, even if there are currently no duplicate rows.

- 6. Select a column in the **Sales Demographic** table and press *Ctrl+A* on your keyboard to select all columns.

- 7. Click the **Remove Rows** option from the **Home** tab.

- 8. Click the **Remove Duplicates** option:

  ![](img/5d2bce74fd6fceebed14fd1488bf7088_340_0.png)

  Figure 6.30: Removing duplicates from the Sales Demographic table

The next step is adding an **Index column** to create a **primary key** for the **Sales Demographic** dimension.

- 9. Click the **Index Column** drop-down button from the **Add Column** tab.

- 10. Click **From 1**:

  ![](img/5d2bce74fd6fceebed14fd1488bf7088_341_0.png)

  Figure 6.31: Adding an Index column to the Sales Demographic table

- 11. Replace **Index** with **SalesDemographicKey** from the formula bar.

- 12. Click the **Submit** button ✓:

  ![](img/5d2bce74fd6fceebed14fd1488bf7088_341_1.png)

  Figure 6.32: Replacing Index with SalesDemographicKey

So far, we have created all the potential dimensions derived from the **Sales Base** and **Customer Base** tables. As discussed earlier in this chapter, we also need to create a `Date` dimension and a `Time` dimension. We do so in the following sections.

## Date

The outcomes of the discovery sessions with the business showed that we need to have `Date` and `Time` dimensions. We already discussed in *Chapter 2, Data Analysis Expressions and Data Modeling*, that we can create the `Date` dimension using DAX. We also discussed the advantages and disadvantages
