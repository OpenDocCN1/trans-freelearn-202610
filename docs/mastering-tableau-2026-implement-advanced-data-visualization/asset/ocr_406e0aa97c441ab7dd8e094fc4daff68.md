# # Other ways to improve performance

To conclude this chapter, let's consider a few other possibilities for improving performance.

+   - **Avoid overcrowding a dashboard**: Often, end users want to see everything at once on a dashboard. Although this may be perceived as beneficial, it often is not. Consider the inclusion of a large crosstab on a dashboard. Does scrolling through pages of details add to the analytical value of the dashboard? Perhaps the answer is "no." Furthermore, an excess of information on a dashboard may obscure important insights. Diplomatically arguing for leaner dashboards may lead to better decision making as well as better performance.

+   - **Fixing dashboard sizing**: Dashboards can be set to an exact size or to **Range** or **Automatic**. Exact size results in quicker performance because once Tableau Server has rendered a view for one end user, that render stays in the cache and can be reused for the next end user that accesses that dashboard.
+   
+   **Automatic** and **Range**, on the other hand, cause Tableau Server to attempt to determine the resolution size used by each end user and render the dashboard accordingly. This means that Tableau Server does not use the instance of the dashboard stored in the cache for the next end user. This, in turn, impacts performance.

+   - **Use Tableau Prep Builder**: This has been mentioned before but is very relevant, therefore I will repeat: If complex joins and complex data blending are required to generate useful results, it may be advisable to perform ETL work outside of Tableau, for example, in Tableau Prep Builder (see *Chapter 3, Using Tableau Prep Builder*, for more information on this service). ETL work will ideally lead to better data modeling, aggregated data, or data that contains additional columns already that you would otherwise have to create in Tableau Desktop, and thus using Prep Builder leads to easier authoring and quicker performance.

+   - **Setting expectations**: If an end user is expecting near-instantaneous performance, then, of course, anything less is disappointing. Explaining in advance that a complicated, detail-oriented dashboard may not be performant can help in at least two ways. First, upon explaining the likely performance problems, a compromise may be reached that results in the creation of a less complicated dashboard that still delivers valuable information. Second, if it is absolutely necessary for the dashboard to be complicated and detail-oriented, at least the end user has been warned that patience may be needed when interfacing it.

# ## Workbook optimizer

Finally, for the lazy ones among us – or one might say, efficient ones – Tableau released a fantastic feature not that long ago: the Workbook Optimizer. It checks your workbook based on a set of rules and will respond with three types of categories: take action, needs review, and passed. Thus, it completes performance checks using the well-known RAG (red-amber-green) or traffic light system for you.