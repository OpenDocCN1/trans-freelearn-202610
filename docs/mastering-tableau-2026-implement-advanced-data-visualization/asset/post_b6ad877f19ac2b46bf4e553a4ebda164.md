
## Other ways to improve performance

To conclude this chapter, let's consider a few other possibilities for improving performance.

- **Avoid overcrowding a dashboard**: Often, end users want to see everything at once on a dashboard. Although this may be perceived as beneficial, it often is not. Consider the inclusion of a large crosstab on a dashboard. Does scrolling through pages of details add to the analytical value of the dashboard? Perhaps the answer is "no." Furthermore, an excess of information on a dashboard may obscure important insights. Diplomatically arguing for leaner dashboards may lead to better decision making as well as better performance.

- **Fixing dashboard sizing**: Dashboards can be set to an exact size or to **Range** or **Automatic**. Exact size results in quicker performance because once Tableau Server has rendered a view for one end user, that render stays in the cache and can be reused for the next end user that accesses that dashboard.

**Automatic** and **Range**, on the other hand, cause Tableau Server to attempt to determine the resolution size used by each end user and render the dashboard accordingly. This means that Tableau Server does not use the instance of the dashboard stored in the cache for the next end user. This, in turn, impacts performance.

- **Use Tableau Prep Builder**: This has been mentioned before but is very relevant, therefore I will repeat: If complex joins and complex data blending are required to generate useful results, it may be advisable to perform ETL work outside of Tableau, for example, in Tableau Prep Builder (see *Chapter 3, Using Tableau Prep Builder*, for more information on this service). ETL work will ideally lead to better data modeling, aggregated data, or data that contains additional columns already that you would otherwise have to create in Tableau Desktop, and thus using Prep Builder leads to easier authoring and quicker performance.

- **Setting expectations**: If an end user is expecting near-instantaneous performance, then, of course, anything less is disappointing. Explaining in advance that a complicated, detail-oriented dashboard may not be performant can help in at least two ways. First, upon explaining the likely performance problems, a compromise may be reached that results in the creation of a less complicated dashboard that still delivers valuable information. Second, if it is absolutely necessary for the dashboard to be complicated and detail-oriented, at least the end user has been warned that patience may be needed when interfacing it.

## Workbook optimizer

Finally, for the lazy ones among us – or one might say, efficient ones – Tableau released a fantastic feature not that long ago: the Workbook Optimizer. It checks your workbook based on a set of rules and will respond with three types of categories: take action, needs review, and passed. Thus, it completes performance checks using the well-known RAG (red-amber-green) or traffic light system for you.

At the time of writing, rules include: calculation length, calculation using multiple data sources, dashboard size not fixed, filter uses conditional logic, filter uses “only relevant values,” live data connections, multiple connections in a data source, nested calculations, non-materialized calculations, number of data sources, number of filters, number of layout containers, number of LOD calculations, number of views in a dashboard, number of workbook sheets, unused data sources, unused fields, uses data blending, uses date calculations, and uses grouping. I am sure, however, there are more rules to come, and it will become increasingly easy to have a high–performing dashboard. You can access the Workbook Optimizer feature by selecting Server | Run Optimizer:

![](img/66cf6b89349fda3581a4f196cb3c98a4_572_0.png)

### Figure 12.22: Run Optimizer

You will then be able to drill down per category and see where your dashboard needs improvement. For most of the rules, some human intervention is required but for some, a click of a button is sufficient, like in Figure 12.23. Observe the icons below the rule – the wrench indicates that Tableau can fix this for you.

![](img/66cf6b89349fda3581a4f196cb3c98a4_573_0.png)

### Figure 12.23: Unused fields

For each rule, Tableau also provides a **Learn More** hyperlink that will redirect you to one of the Tableau help pages for further explanation.

![](img/66cf6b89349fda3581a4f196cb3c98a4_574_0.png)

### Figure 12.24: Number of data sources

Isn't this feature amazing!? From now on, always make sure to use the Run Optimizer button!

## Follow-along dashboard

In this follow-along dashboard, we will use a New York City bike dataset and apply performance-enhancing features.

- 1. Download one month of data from https://citibikenyc.com/system-data (e.g., January 2023, located in 2023-citibike-tripdata.zip) and connect Tableau Desktop to the dataset. January 2023 is split into two CSVs; you can select either one.
- 2. Immediately create an Extract (in the top right corner)
> **Note**
> Performance Tip: Never connect live to multi-million-row CSVs unless absolutely necessary.
- 3. Open a new worksheet. Drop Started At, Start Station Name, Member Casual, and Rideable Type onto the Text field in the marks card.
- 4. Click on the arrow in the data pane and choose Hide All Unused Fields:

![](img/66cf6b89349fda3581a4f196cb3c98a4_575_0.png)

### Figure 12.25: Hide unused fields

> **Note**
> Performance Tip: Fewer fields = smaller extract = faster queries.

- 5. Next, create a new worksheet called "KPI Sheet (Total Trips)"
- 6. Create a calculated field called "Trip Count": 1
- 7. Drag the Trip Count to Text in the Marks shelf

> **Note**
> Performance Tip: Counting rows using SUM(1) is faster than COUNT([Trip ID]).

- 8. Create a new worksheet called "Trips by User Type"
- 9. Drag Member Casual to Rows
- 10. Drag Trip Count to Columns
- 11. Sort descending

> > Performance Tip: Dimensions with low cardinality (2 values here) are very fast.

- 12. Create another Worksheet called "Monthly Trips Trend"
- 13. Drag **Started At** to Columns
- 14. Change to **DAY**
- 15. Drag **Trip Count** to Rows

> > Performance Tip: Avoid lower granularity (here e.g. Minute or Second) that explodes the mark count.

- 16. Create a new worksheet called "Top 10 Start Stations"
- 17. Drag **Start Station Name** to Rows
- 18. Drag **Trip Count** to Columns
- 19. Sort descending
- 20. Add Top N Filter: Top 10 by SUM(Trip Count)

![](img/66cf6b89349fda3581a4f196cb3c98a4_576_0.png)

### Figure 12.26: Filter

> > Performance Tip: Always limit high-cardinality dimensions such as station names.

- 21. Add the following Filters: Member Casual and Rideable Type and make them a dropdown filter

> **Note**
> Performance Tip: Dropdowns are faster than checklists, especially with large datasets.

- 22. Build the Dashboard by choosing a fixed size of 1200 x 800 and the following layout:
  - Top: KPI - Total Trips
  - Middle Left: Monthly Trips Trend
  - Middle Top Right: Trips by User Type
  - Middle Bottom Right: Top 10 Start Stations

- 23. Lastly, format as desired

![](img/66cf6b89349fda3581a4f196cb3c98a4_577_0.png)

### Figure 12.27: Final Dashboard

This was a quick and easy dashboard where all important performance-enhancing features were applied.
Besides, can you create a map with the most used bike tracks in New York from this dataset?

## Summary

We began this chapter with a discussion of the performance-recording dashboard. This was important because many of the subsequent exercises utilized the performance-recording dashboard to examine underlying queries. Next, we discussed hardware, and on-the-fly techniques, where the intent was to communicate hardware considerations for good Tableau performance and, in the absence of optimal hardware, techniques for squeezing the best possible performance out of any computer.

Then we covered working with data sources, including joining, blending, and efficiently working with data sources. This was followed by a discussion on generating and using extracts as efficiently as possible. By focusing on data sources for these three sections, we learned best practices and what to avoid when working with either remote datasets or extracts. The next sections explored performance implications for various types of filters and calculations. Lastly, we looked at additional performance considerations, where we explored a few more thoughts regarding dashboard performance as well as setting expectations.

In the next chapter, we will turn our attention to Tableau Server. Tableau Server is a dense topic worthy of its own book; therefore, our exploration will be truncated to only focus on Tableau Server from the Tableau Desktop author’s perspective.

## Join our community on Discord

Join our community’s Discord space for discussions with the authors and other readers.

https://discord.gg/VXpz9YPUTW

![](img/66cf6b89349fda3581a4f196cb3c98a4_578_0.png)
