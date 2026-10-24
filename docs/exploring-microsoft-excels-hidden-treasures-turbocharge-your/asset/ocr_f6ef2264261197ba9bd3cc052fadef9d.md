> Nuance
> You can sort data in Power Query in the same fashion as Excel, so that we could just as easily
> select the February file first and the January file second.

+   7. Type Fruit Sales in the Name field of the Query Settings task pane and then press Enter
+   to give the combined list a more descriptive Name than the default Name of Append1.

+   8. Now, let's import the vendor list so that we can replace the vendor ID numbers with vendor
+   Names in our Fruit Sales query. Click Home | New Source | File | Text/CSV, select the
+   Chapter 12 - Vendors.csv file, click Import, and then OK.

+   9. At this point, we're going to merge the data from the Chapter 12 - Vendors.csv file
+   into the Fruit Sales query, but first, we need to make sure that the ID numbers have the
+   same data type. You can't merge data from two different queries if the fields that you're matching
+   are of different types, such as text and numbers. Here's how to check this:

![](bbox=[0.168, 0.484, 0.821, 0.782])

# Figure 12.29 - Data types in Power Query

+   II. Choose Fruit Sales from the query list along the left-hand side of the Power Query
+   Editor and make note of the data type for the Vendor field, which is also 123, meaning
+   a whole number. We can now merge these two queries since the matching fields in both
+   queries are set to the same data type.