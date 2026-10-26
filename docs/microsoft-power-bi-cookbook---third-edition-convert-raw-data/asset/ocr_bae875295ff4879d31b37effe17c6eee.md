+   11. In the **Queries** pane, right-click the AdventureWorksDW2022 query and deselect **Enable load**. Notice that the query name is now italicized. Disabling the loading of queries prevents those queries from loading data into the data model.

+   12. Now, right-click the AdventureWorksDW2022 query again and select **Move To Group, New Group...**, as shown in *Figure 2.5*:

![](bbox=[0.244, 0.178, 0.862, 0.604])

Figure 2.5: Grouping queries

+   13. In the **New Group** dialog, type Data Sources for the name of the query, and then click the **OK** button.

Grouping queries helps you organize and maintain your queries. In complex data models, there may be dozens or even hundreds of individual queries, so it is important to maintain proper hygiene in order to manage your queries effectively.

+   14. Recall that we used the AdventureWorksDW2022 query as a reference for our Products query. We can observe this dependency by clicking on **Query Dependencies** in the **View** tab of the ribbon. This shows the dependency graphically, as shown in *Figure 2.6*.