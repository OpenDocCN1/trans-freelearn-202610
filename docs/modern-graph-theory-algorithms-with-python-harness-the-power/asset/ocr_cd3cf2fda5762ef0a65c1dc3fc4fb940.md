This should give a plot similar to Figure 6.10, which shows a more complex connectivity than the threshold = 1 results:

![](bbox=[0.2, 0.16, 0.75, 0.44])

Figure 6.10 – A visualization of the threshold = 10 results for two-way interactions

These results show that different radius thresholds yield different networks, just as our correlation thresholds produced different networks. We could build a full filtration from the first appearance of an edge until all possible edges exist in our network to track changes in network structure based on distances between points. We could also include three-way and four-way interactions in our construction of the Vietoris–Rips complex to extend our analysis to the fully simplicial complexes that exist in each slice of our dataset.

While simplicial complex metrics are beyond the scope of this book, many extensions of network metrics to simplicial complexes exist (such as Forman–Ricci curvature centrality) and may merit investigation in the analysis of time series data. Currently, very little work has elucidated the use of simplicial complex metrics or methods on stock market change point detection.

If you are interested, we encourage you to extend Scripts 6.1 and 6.2 to include an analysis of the full time series through the lens of simplicial complexes and calculate the extensions of the network metrics in terms of simplicial complexes. Our chapter reference section includes papers that experiment with these extensions.