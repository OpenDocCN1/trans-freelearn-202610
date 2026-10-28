As you can see in the following figure, we created a virtual table on top of the **Internet Sales** table. We get the **ProductStandardCost** values grouped by **ProductKey** from the **Product** table:

![](bbox=[0.175, 0.22, 0.825, 0.295])

Figure 2.14: A virtual table created on top of two tables

How is that possible? Here is how it works.

As mentioned earlier, the `AVERAGEX()` function is an iterator function. It goes through each row of the virtual table created by the `SUMMARIZE()` function. The `SUMMARIZE()` function gets the values of the **ProductStandardCost** column from the **Internet Sales** table grouped by related **ProductKey** instances from the **Product** table via the existing relationship between the two tables. When we run the `SUMMARIZE()` part of the preceding calculation in DAX Studio, we see that `SUMMARIZE()` retrieves only 158 rows, while the **Internet Sales** table has more than 60,000 rows. The reason is the existing relationship between the **Internet Sales** table and the **Product** table.

The following figure shows the results of running the `SUMMARIZE()` section of the preceding calculation in DAX Studio: