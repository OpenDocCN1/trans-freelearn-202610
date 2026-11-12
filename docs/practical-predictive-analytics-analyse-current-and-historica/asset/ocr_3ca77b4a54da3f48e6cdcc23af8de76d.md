```
#always set seed before clustering
set.seed(1020)
clust3 <- kmeans( y,3)
clust3$betweenss/clust3$totss
clust5 <- kmeans( y,5)
clust5$betweenss/clust5$totss
clust7 <- kmeans( y,7)
clust7$betweenss/clust7$totss
```

After creating the three clusters, print the ratio of the cluster between sum of squares to the cluster total sum of squares. That will give you a starting metric to begin comparing clusters. A higher number implies better separation between the clusters:

```
> set.seed(1020)
> clust3 <- kmeans( y,3)
> clust3$betweenss/clust3$totss
[1] 0.8706681
> clust5 <- kmeans( y,5)
> clust5$betweenss/clust5$totss
[1] 0.9178001
> clust7 <- kmeans( y,7)
> clust7$betweenss/clust7$totss
[1] 0.9381889
```

So, based on the output above, cluster 7 has the highest betweenss/totss ratio, and having seven clusters would be considered the optimal number of clusters. But, if you run the analysis, increasing the number of clusters each time, you will always see an increase in the ratio. So, we need to determine where the law of diminishing returns kicks in.

# Cluster elbow plot

One way to determine this is by using an automated method known as the elbow method in which you plot the total within clusters sum of squares for all values within a range, and look at where an elbow method develops in the plot:

```
#elbow method
set.seed(1020)
# Compute and plot wss for k = 3 to k = 15
```