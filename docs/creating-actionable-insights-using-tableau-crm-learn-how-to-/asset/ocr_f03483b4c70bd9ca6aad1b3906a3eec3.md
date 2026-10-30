# Figure 5.13 – Augmenting granular and aggregated data

Notice in Figure 5.13 that the original dataset you ingested, Opportunities, was split into two streams, one for the aggregation and one for the granular data, and these are then combined into the dataset that drives the industry study insights.

A Transform node was added to the combined stream (see the top-right corner of Figure 5.13) to calculate the difference between the average deal size by industry for each sales team member with the average deal size by industry across the whole team.