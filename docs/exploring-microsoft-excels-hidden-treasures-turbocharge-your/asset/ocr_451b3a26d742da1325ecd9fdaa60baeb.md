# # Unpivoting data

Unpivoting data is sometimes referred to as flattening the data, which means that all similar values appear in columns instead of rows. For instance, the _Profit & Loss By Class_ report that I exported from QuickBooks Desktop, shown in Figure 12.24, spans 123 columns, running from column **A** to column **DS**. It’s hard to do much with a report like that since the data is so broadly dispersed. Let’s use **Power Query** in Excel for Windows or macOS to transpose 123 columns of data into three. Once again, I’ll move faster by not renaming anything in the **Applied Steps** list:

![](bbox=[0.133, 0.266, 0.855, 0.457])

Figure 12.24 – Data to be unpivoted

+   1. Press `Ctrl + N` (`⌘ + N`) to create a blank workbook (or choose **File | New** and choose **Blank Workbook**).
+   2. Click **Data | Get Data | From File | From Excel Workbook**, select _Chapter 12 – Unpivoting Data.xlsx_, and then click **Import**.
+   3. Select **Sheet1** from the list and then click **Transform Data**.
+   4. Click **Column1** and then hold down the _Shift_ key and click **Column6** to condense multiple columns that contain account numbers into one.
+   5. Right-click on the selection and choose **Merge Columns** (or choose **Transform | Merge Columns**), enter `Account` into the **New column Name** field, and then click **OK**.
+   6. Click on the **Account** column, which deselects the other columns.
+   7. Right-click the **Account** column, and then choose **Unpivot Other Columns** (or choose **Transform | Unpivot Columns**).
+   8. As shown in Figure 12.25, the data now appears in three columns. You can now build a **PivotTable**, filter the data, use **Slicers**, and conduct any number of other analytical tasks: