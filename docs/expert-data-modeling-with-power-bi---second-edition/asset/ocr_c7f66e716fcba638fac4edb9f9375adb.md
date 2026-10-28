![](bbox=[0.175, 0.08, 0.823, 0.265])

Figure 5.61: The results of pivoting Date by Sales values resulted in errors

As stated before, the pivot function aggregates the intersection of the column that represents column headers and all other columns in the table. So we just need to add a column that represents unique values for each row that forces the intersection of the pivoted column and all other columns to always have unique values. Therefore, adding an Index Column must resolve the issue. Follow these steps to add an Index Column before pivoting the table:

+   1. Select the previous step of the pivot column.
+   2. Click the Index Column dropdown from the Add Column tab.
+   3. Select From 1.
+   4. Click Insert when the Insert Step dialog box appears.

The following image shows the preceding steps: