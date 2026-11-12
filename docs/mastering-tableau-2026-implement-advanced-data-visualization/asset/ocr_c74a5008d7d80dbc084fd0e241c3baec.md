5. Right-click on **SUM(Sales)** on the **Rows** shelf and select **Format**. In the resulting **Format** window, choose the **Pane** tab and select **Marks: Show at Default Value**:

![](bbox=[0.09, 0.11, 0.86, 0.49])

Figure 6.14: Filling Data Gaps II

Note that the distance between marks is now based on the length of time between dates. All dates with no value are displayed with **Sales** values of zero.

# Problems with domain padding

Since domain padding can be toggled on or off through specific commands in Tableau (that is, **Show Missing Values** and **Show Empty Rows/Columns**), it's not typically a problem. There are a few scenarios, however, when domain padding may cause confusion, one of which is covered in the following example.

# From a domain-padded visualization to a crosstab

Let's have a look at creating a crosstab from a domain-padded visualization:

+   1. Create a new workbook entitled **DP - From Viz to Crosstab**.
+   2. Select the **Sample - Superstore** data source.
+   3. Right-click on **Discount** in the **Data** pane and select **Create | Bins**. In the resulting dialog box, choose a bin size of **0.05**.
+   4. Place the newly created **Discount (bin)** dimension on the **Columns** shelf.