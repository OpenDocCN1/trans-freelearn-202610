The following screenshot shows the MTF of the same three time series as before:

![](bbox=[0.117, 0.131, 0.858, 0.421])

Figure 1.21 – Uncovering time series behavior with MTF

If you're interested in what insights this representation can bring you, feel free to dive deeper into it by walking through this GitHub repository: `https://github.com/michaelhoarau/mtf-deep-dive`. You can also read about this in detail in the following article: `https://towardsdatascience.com/advanced-visualization-techniques-for-time-series-analysis-14eeb17ec4b0`

# Modularity network graphs

From an MTF (see the previous section), we can generate a graph $G = (V, E)$: we have a direct mapping between vertex $V$ and the time index $i$. From there, there are two possible encodings of interest: flow encoding or modularity encoding. These are described in more detail here:

+   - **Flow encoding:** This representation helps us observe where the big information flow occurs.
+   
+     We map the flow of time to the vertex, using a color gradient from $T0$ to $TN$ to color each node of the network graph.
+   
+     We use the MTF weight to color the edges between the vertices.
+   
+   - **Modularity encoding:** Modularity is an important pattern in network analysis to identify specific local structures. This is the encoding that I find most useful in time series analysis.