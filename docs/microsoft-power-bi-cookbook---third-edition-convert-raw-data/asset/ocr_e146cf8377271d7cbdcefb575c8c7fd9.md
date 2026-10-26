# Query Caching

Capacity default: query caching Off

Off: Do not cache query results for this semantic model

On: Speed up reports by using previously saved query results.

Apply

Discard

# Scheduled refresh and performance optimization

To improve the performance of exploring reports, enable sche caching can improve query response times.

# Automatic aggregations training

To speed up exploring reports, Power BI can generate aggrega run faster, and visuals with cached data will load more quickly.

Off

# Automatic dashboard tile and metric refresh

Adjust the automatic refresh frequency of each dashboard tile More. Learn more

On

Refresh schedule:

Refresh frequency

15 minutes

Apply

Discard

Figure 11.21: DirectQuery semantic model settings

By default, the dashboard tiles are refreshed each hour for DirectQuery and Live Connection semantic models. In this process, queries are sent by the Power BI service through the gateway to the semantic model sources. In this scenario, the organization is comfortable with the more frequent queries, but in other scenarios, simply a daily or even a weekly dashboard refresh would be sufficient to avoid adding workload to the data source.

# See also

+   - Power BI REST API Datasets—Refresh Dataset: https://bit.ly/3xWvE6J
+   - Add or remove a gateway data source: https://bit.ly/3f7GvSG
+   - How to configure a Power BI report scheduled refresh: https://bit.ly/3evfZU7