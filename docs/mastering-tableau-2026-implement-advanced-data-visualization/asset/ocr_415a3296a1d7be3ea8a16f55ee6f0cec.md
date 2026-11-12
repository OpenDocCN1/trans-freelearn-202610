# Clustering in Tableau using R

**Clustering** is used to select smaller subsets of data with members sharing similar characteristics from a larger dataset. As an example, consider a marketing scenario. You have a large customer base to which you plan to send advertising material; however, cost prohibits you from sending material to every customer. Clustering the dataset will return groupings of customers with similar characteristics. You can then survey the results and choose a target group.

Major methods for clustering include hierarchical and k-means. Hierarchical clustering is more thorough and thus more time-consuming. It generates a series of models that range from one, which includes all data points, to n, where each data point is an individual model. K-means clustering is a quicker method in which the user or another function defines the number of clusters. For example, a user may choose to create four clusters from a dataset of a thousand members.

Clustering capabilities are included with Tableau. You can find this functionality under the **Analytics** tab. The clustering implementation in Tableau is based on four pillars:

+   • Solid methodology
+   • Repeatable results
+   • A quick processing time
+   • Ease of use

Be sure to check out more details here (https://help.tableau.com/current/pro/desktop/en-us/clustering.htm) and the tab **Clustering (Tableau native)** in the accompanying solutions workbook.

There are numerous ways the Tableau development team could have approached clustering. R, for instance, provides many different clustering packages that use different approaches. A Tableau author may have good reason to choose one of these different approaches. For example, clustering results are always identical when using the native Tableau clustering capabilities. But they do not have to be. By using R for clustering, the underlying data and the view may remain unchanged, yet clustering could differ with each refresh because the function will stop at the best result (local minimum) before it has seen the whole dataset. The trade-off between using a so-called local minimum versus a global minimum for performance has been proven to be worth it. But depending on the order of numbers, the local minimum can differ each time you run the function. This could be advantageous to you if looking for edge cases where marks may switch between clusters. The following example explores such a case.

Our goal in this exercise is to create four clusters out of the countries of the world based on birth rate and infant mortality rate:

+   1. Navigate to the Cluster (R) worksheet
+   2. Select the World Indicators data source (this dataset ships with Tableau and can be found under **Saved Data Sources**)
+   3. Build the initial view by placing **Infant Mortality Rate** on the **Columns** shelf, **Birth Rate** on the **Rows** shelf, and **Country/Region** on the **Details** shelf