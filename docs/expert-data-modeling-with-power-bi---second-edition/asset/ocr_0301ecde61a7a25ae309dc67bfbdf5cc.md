![](bbox=[0.177, 0.091, 0.821, 0.433])

Figure 14.15: Reseller Sales data added to the Power BI dataset

So, the new data model contains the tables from the remote dataset and two imported tables from the data warehouse. The remote dataset in the Power BI service has RLS implemented, so we expect RLS to propagate to the new data model. The new report file is `Chapter 14, Composite Model Internet Sales.pbix`, which can be downloaded from here:

[https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2014/Chapter%2014,%20Composite%20Model%20Internet%20Sales.pbix](https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2014/Chapter%2014,%20Composite%20Model%20Internet%20Sales.pbix)

As discussed in *Chapter 11, Row-Level and Object-Level Security*, RLS kicks in only when the user has the **Viewer** role on the containing workspace or the report is shared with the user via a link, direct access, or an app. So, I published the new report to the Power BI service and made a desired security group **Viewer** role on the workspace. The following image shows the lineage view of the new report published to the Power BI service: