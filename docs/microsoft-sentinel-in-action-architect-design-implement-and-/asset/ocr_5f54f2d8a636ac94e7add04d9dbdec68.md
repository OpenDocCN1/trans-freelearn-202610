> Tip
> Changing the date and time field here will change how the date and time are displayed whenever you show a time field in the results. You can also change the time zone used to display the time on a result-by-result basis in the header of the Results section. See The results window section for more information.

+   - **Tables pane**: Choose what happens when you double-click on a table in the **Tables** pane. If you select **Run preview query**, the KQL Query pane will have the table name followed by:
+   
+     ```
+     | where TimeGenerated > ago(24h)
+     | limit 10
+     ```
+   
+     If you select **Adds the table name to the query editor**, then only the table name will be placed in the KQL Query pane and not any of the code.
+   
+     You can also determine if you want empty tables to show up or not. If you enable the slider called **Show tables with no data**, then any table that is part of your workspace, even if it has no data, will show up.
+   
+     This can be useful if you need to see what tables you have available to query, but could be frustrating if you are expecting to see data in all the tables.

```
| where TimeGenerated > ago(24h)
| limit 10
```

+   - **Sort results**: Choose whether you want to automatically sort the results using the **TimeGenerated** field or not. If this is set to **On** (the default), then whenever the results are shown, they will be sorted using the **TimeGenerated** field. If this is set to **Off**, the results are shown in whatever order they were stored. Note that any sorting performed in the query will override this selection.

+   - **Table view**: How many rows to show on each page. The drop-down menu allows you to select **50**, which is the default, along with **100**, **150**, and **200**. Selecting higher values will allow more rows to be shown on a single page.

+   - **History Queries** (not shown in the screenshot): There is nothing to modify here; it is just to tell you how long the query history is stored for and how to clear the query history.

Let's look at the book icon next.