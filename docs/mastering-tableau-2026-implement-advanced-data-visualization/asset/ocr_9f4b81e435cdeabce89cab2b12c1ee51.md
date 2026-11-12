![](bbox=[0.1, 0.07, 0.85, 0.3])

# Figure 4.1: Relationships and joins on the data source canvas

A key difference is that the preview of the data, at the bottom, will show only data from the selected table in relationships, compared to all data when using joins. This makes sense because the granularity of data can change in relationships, depending on the fields you are using in your dashboard. Joins, however, have a fixed level of granularity, which is defined by the type of join and join clauses you choose.

Relationships are the default for Tableau Desktop. However, if you still want to make use of the join functionality, you can drag a dataset into the data source canvas, click on the drop-down arrow, and select **Open...**:

![](bbox=[0.22, 0.49, 0.66, 0.8])

# Figure 4.2: From relationship to join canvas

This will open the dataset as you used to see it in the **Data** pane in Tableau in versions before 2020.2, and you will be able to use joins the old way, as we will describe in the *Working with joins* section.