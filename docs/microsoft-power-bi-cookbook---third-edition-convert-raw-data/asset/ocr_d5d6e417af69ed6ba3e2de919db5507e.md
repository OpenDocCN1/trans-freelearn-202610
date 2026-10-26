# How to perform grouping and binning

To implement this recipe, carry out the following steps:

+   1. Right-click the `DateFirstPurchased` column in the Customers table and choose **New Group**.
+   2. Set the **Name** to `Customer First Purchase Calendar Quarter`, the **Group type** to **Bin**, the **Bin type** to **Size of bins**, and the **Bin size** to 3, choose **Months** from the dropdown, and click **OK**.

![](bbox=[0.229, 0.204, 0.883, 0.607])

*Figure 9.23: Groups dialog for the FirstPurchaseDate column with a Bin size of 3 Months*

+   3. Create a **Matrix** visual that analyzes the sales of these quarterly customer bins using the newly created `Customer First Purchase Calendar Quarter` column in the Customer table for **Rows**, the `CalendarYear` column from the Dates table for **Columns**, and the `SalesAmount` column (Sum) from the `Internet Sales` table as **Values**.