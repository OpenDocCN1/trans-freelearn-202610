![](bbox=[0.135, 0.075, 0.913, 0.405])

# Figure 3.30: Host chart

In the preceding screenshot, we can see the distinct count of reviews per host_since date. Of course, in this specific example, Tableau Desktop could also count the reviews per host_since date if those were the only fields dragged onto the view without the help of Prep. But a benefit of aggregating prior to importing the data into Tableau Desktop is that you have less data to import; hence, it’s possible to achieve better performance. If you want to continue analyzing the dataset in either Prep or Tableau Desktop, you can now ask further questions, such as the following:

+   - Which type of accommodation has the highest discount for staying a week rather than a day?
+   - Is there a correlation between the size of the accommodation and the monthly price?
+   - How many hosts (as a percentage) have more than one listing?

# Multi-row Calculations

Let me share a feature that has quickly become one of my favorites in recent releases. Starting with Tableau Prep 2023.2 (and enhanced through 2024), you can now perform multi-row calculations directly in your Prep flow. Think of it as bringing table calculations, like moving averages, differences from previous rows, and percent differences, out of Tableau Desktop and into your data preparation layer.

In Tableau Desktop, table calculations are computed after the query returns. This works fine for small datasets, but with larger data, you might notice:

+   - Dashboards load more slowly because the calculation happens in the view
+   - Filters can change how your calculation behaves in unexpected ways
+   - You can’t use the result in subsequent calculations without creating complex nested table calcs