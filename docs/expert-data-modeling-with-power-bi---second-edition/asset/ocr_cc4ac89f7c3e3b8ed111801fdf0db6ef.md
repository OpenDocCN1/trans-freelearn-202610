Modeling-with-Power-BI-Second-Edition/blob/a39dd14eeac3fdceafd3daf4a45909f6693d68b6/Source%20Code/Chapter%2003/Chapter%203,%20Query%20Editor.pbix

After opening the `Chapter 3, Query Editor.pbix` file, we head to the **Power Query Editor** where we look at the **Quality** box of all columns. We quickly understand that we can remove the following columns:

+   1. Title
+   2. Suffix
+   3. AddressLine2

The following image shows that the preceding columns contain a lot of empty values:

![](bbox=[0.175, 0.422, 0.825, 0.632])

Figure 3.28: Using Column quality to identify columns with less than 10% valid data

We can remove those columns by following these steps:

+   1. Click the **Home** tab.
+   2. Click the **Choose Columns** button.
+   3. Uncheck the preceding columns.
+   4. Click **OK**.