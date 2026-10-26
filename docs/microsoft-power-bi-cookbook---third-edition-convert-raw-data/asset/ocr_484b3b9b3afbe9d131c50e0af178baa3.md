One of the top performance and scalability features of M's query engine is called Query folding. If possible, the M queries developed in Power BI Desktop are converted (“folded”) into SQL statements and passed to source systems for processing.

In fact, the query from this recipe was folded into a SQL statement and sent to the server for processing, as opposed to the M query engine performing the processing locally. To see how this works, perform the following:

+   1. Select the **Products** query in the **Queries** pane.
+   2. Right-click on the **Renamed Columns** step in the **APPLIED STEPS** section of the Query Settings pane, and then select **View Native Query**, as shown in Figure 2.9:

![](bbox=[0.36, 0.28, 0.745, 0.73])

Figure 2.9: View Native Query in Query Settings