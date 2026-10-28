Here is the point: when the Power Query engine can fully fold a query, it passes the generated native query to the source system and gets the results back. On the contrary, when a query is partially folded, the Power Query engine sends the folded query parts back to the source system, gets the results back, and starts applying the unfolded transformation steps on our local machine itself. If the query is not folded at all, the Power Query engine itself must take care of all transformation steps.

# DirectQuery and Dual storage modes and query folding

The concept of query folding in programming is called server-side/client-side data processing. A fully folded query is processed on the server, which is much more efficient when the query is partially or fully processed client-side.

Therefore, the queries in either DirectQuery or Dual storage modes must be fully folded; in other words, they must be translatable into the native query supported by the data source system. Hence, when we use a Power Query function that is not foldable in DirectQuery or Dual storage modes, we get the following warning, as also shown in Figure 7.2:

![](bbox=[0.17, 0.6, 0.82, 0.63])

Figure 7.2: Switch all tables to Import mode warning in DirectQuery storage mode

# Data sources and query folding

Most of the data sources that have a native query language support query folding, including the following:

+   - Relational databases that are supported in Power BI
+   - OData feeds