Now that we have computed our metrics, let’s examine the correlations between the metrics across our set of time series windows to see how the different metrics relate to each other. Given that Forman-Ricci curvature depends on degree centrality metrics, we’d expect to see a strong correlation. We can add to Script 6.1 to obtain these correlations:

```
#examine correlations among metrics across the time series
print(np.corrcoef(deg_ave,eig_ave))
print(np.corrcoef(deg_ave,bet_ave))
print(np.corrcoef(deg_ave,vcurv_ave))
print(np.corrcoef(eig_ave,bet_ave))
print(np.corrcoef(eig_ave,vcurv_ave))
print(np.corrcoef(bet_ave,vcurv_ave))
```

You should see a strong correlation between degree centrality and Forman–Ricci curvature centrality (-0.99 in our analysis) but fairly weak correlations among the other centrality metrics (-0.05 for degree and Pagerank centralities, 0.01 for degree and betweenness centralities, 0.04 for Pagerank and betweenness centralities, 0.04 for Pagerank and Forman–Ricci curvature centrality, and 0.05 for betweenness and Forman–Ricci curvature centrality). This suggests that degree centrality and Forman–Ricci curvature may be interchangeable in these analyses, though a more complex Forman–Ricci curvature metric may capture a bit more information that may be relevant to certain trends. It’s unclear if these correlations would hold for other datasets, though degree centrality and Forman–Ricci curvature on unweighted networks will correlate to some extent, given that the Forman–Ricci curvature formula depends on degree centrality.

Let’s visualize the network metric trends over time to see if any patterns emerge or extreme values stand out that might indicate behavior changes in our stock pricing. We’ll add visualization code to Script 6.1 to do this:

```
#plot metric averages across time slices
time=range(0,3927)
plt.plot(time, deg_ave, label = "Degree Average")
plt.plot(time, eig_ave, label = "Pagerank Average")
plt.plot(time, bet_ave, label = "Betweenness Average")
plt.plot(time, vcurv_ave, label = "Forman-Ricci Curvature Average")
plt.legend()
plt.show()
```