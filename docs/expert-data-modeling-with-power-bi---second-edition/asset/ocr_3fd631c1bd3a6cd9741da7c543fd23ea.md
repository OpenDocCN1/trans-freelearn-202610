![](bbox=[0.177, 0.091, 0.822, 0.394])

Figure 8.33: Internet Sales layout in the Model view

We solved the same problem in *Chapter 2, Data Analysis eXpressions and Data Modeling*, in the *Understanding relationships in virtual tables* section. We want to solve the problem by creating a relationship between the **Exchange Rates** table and the **Internet Sales** table.

If we look at the data in the **Exchange Rates** table, we can see that **CurrencyKey** is not the table’s primary key, as it contains many duplicate values. The following image shows that the **Exchange Rates** table has **14,264 rows**, while the **CurrencyKey** column only has **14 distinct values**: