![](bbox=[0.178, 0.091, 0.822, 0.385])

Figure 15.22: Getting a native query from the diagram view in Datamarts

One of the use cases for the generated native query is to reuse it later to create new tables or SQL queries.

As we can see in the preceding image, we can look at the query plans to investigate the queries in more detail. The next section explains how.

# View query plan

One of the coolest new features available for public preview in 2023 for Power Query Online is named **View query plan**. This feature provides more details about the query evaluation, which can be used for query performance tuning and seeing where things went wrong from a query folding perspective. We can get to the query plan by right-clicking a transformation step and clicking the **View query plan** option from the context menu, as shown in the following image: