# ## 2. Choose Refresh data when opening the file from the Usage tab, as shown in Figure 12.6:

![](bbox=[0.264, 0.142, 0.75, 0.564])

Figure 12.5 – The Query Properties dialog box

When you open your workbook, **Power Query** will refresh the connection automatically. This means that any new worksheets that have been added will appear automatically in the index. This applies to any connection that you create in **Power Query**.

# ## Updating the worksheet index

You may have noticed that the **Index** worksheet doesn’t appear in the **Power Query** results. In this case, the **Index** worksheet was created _after_ we established our **Power Query** connection, so **Power Query** isn’t aware of the new worksheet yet. As you’re about to see, for this particular type of query refreshing the list isn’t enough to make new worksheets appear on the list:

+   1. Right-click on cell **A1** of the **Index** worksheet and then choose **Refresh**. Notice that **Index** _still_ does not appear. **Power Query** is referring to the _saved_ version of our _Chapter 12 – Workbook Index.xlsx_ workbook, and _not_ the version present in memory.