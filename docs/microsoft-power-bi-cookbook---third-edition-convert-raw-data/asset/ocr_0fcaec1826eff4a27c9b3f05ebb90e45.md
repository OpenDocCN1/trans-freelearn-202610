The data integration projects created in the Power Platform admin center leverage the same Power Query technology as Power BI and dataflows. This can be seen by selecting the task in the data integration project created in this recipe and then selecting Advanced query and filtering. The familiar Power Query Editor interface is shown, including access to the Advanced Editor for the direct insertion of M code. This Advanced query and filtering interface can be used to further refine the source data query, such as the removal of unnecessary columns, to improve the overall efficiency of the data source query or perform required transformations.

# There’s more…

Using data integration projects is only one way to get data out of D365 F&SCM environments and into Dataverse, or other environments easily accessible to Power BI. Using Lifecycle Services, dual-write capabilities can be enabled and also exported to Azure Data Lake:

![](bbox=[0.12, 0.33, 0.9, 0.59])

Figure 12.49: Dual-write and export to Data Lake in Lifecycle Services

Dual-write technology enables near real-time, bidirectional integration between D365 F&SCM apps and Dataverse. Data changes in D365 F&SCM apps cause write operations to Dataverse, and vice versa.

Export to Data Lake copies information from D365 F&SCM tables and entities to low-cost Azure Data Lake Storage Gen2. As data changes in D365 F&SCM, changes, deletions, and additions are processed within minutes.

In addition, Power BI tiles, dashboards, and reports can be pinned to application workspaces within D365 F&SCM after a one-time integration within the D365 F&SCM environment.

Finally, Power BI reports can be deployed via Lifecycle Services. This approach leverages Power BI embedding capabilities.

# See also

+   - *Administer Microsoft Power Platform:* https://bit.ly/3bqaKTQ
+   - *Integrate data into Microsoft Dataverse:* https://bit.ly/3bpQykV