Another way to review the usage is to run a report called API Usage Last 7 Days, which shows the usage for the last 7 days as seen in the following screenshot. This report is only available in the Classic view:

![](bbox=[0.196, 0.166, 0.768, 0.476])

Figure 6.11 – The report is showing the number of API calls that have been made in the last 7 days

These reports can be a useful tool to gain insight on how APIs are being used in your org and whether any proactive intervention is required to prevent API limit-related errors.

# Query Plan tool

This is used to access query plans for Structured Object Query Language (SOQL) and Salesforce Object Search Language (SOSL) queries, reports, and list views to optimize them for performance. This is a valuable resource at your disposal that you can use to proactively avoid problems or quickly rule out SOQL queries as the cause of performance issues. The Query Plan tool is accessed from the Developer Console and helps in tuning SOQL queries. You can use the tool to understand the different query plans and whether your indexed query filters will be used by the Query Optimizer or whether it will perform a full table scan.

> Query plan
> A query plan is essentially the map showing a sequence of steps that can be used to access data from the database. The Query Optimizer, upon submission of a query, evaluates some of the different approaches it can take to fetch the data and returns the plan with the least cost. This means that the returned plan will get the data in the least amount of time.