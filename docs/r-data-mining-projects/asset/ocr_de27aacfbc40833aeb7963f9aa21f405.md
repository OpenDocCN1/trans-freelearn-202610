+   2. **Scaling the dataset:** Inclusion of only continuous variables in the K-means clustering exercise does not guarantee that all the variables would be on the same scale. For example, the age of a customer ranges from 0-100 and the income of a customer ranges from 0-100,000. Since both units of measurement are different, they might fall into different clusters, which becomes more complex when we have more data points to be represented. In order to compare segments in a clustering process, all the variables need to be standardized on a scale; here we have applied standard normal transformation (Z-transformation) for all the variables:

```
```r
> r_df_scaled<-as.matrix(scale(r_df[ ,c(-1,-2)]))
> head(r_df_scaled)
Fresh Milk Grocery Frozen Detergents_Paper Delicassen
[1,]  0.2351056 1.2829174 0.11321460 -0.96232799 0.1722993 0.1570550
[2,] -0.4151579 1.3233470 0.45658819 -0.30652122 0.4120701 0.6313698
[3,] -0.4967305 1.0597962 0.13425842 -0.03373355 0.4984496 1.8982669
[4,]  0.3041643 -0.9430315 -0.45821928 1.66113143 -0.6670924 0.6443648
[5,]  1.3875506 0.1657331 0.05110966 0.60623797 -0.1751554 1.8928669
[6,] -0.1421677 0.9153464 -0.30338465 -0.77070636 -0.1681831 0.2974239
```
```

+   3. **Choose initial cluster seeds:** The selection of initial random seeds decides the number of iterations required for convergence of the model. Typically, initial cluster seeds chosen at random are temporary means of the clusters. The objects closer to the centroid measured by the distance function are assigned the cluster membership. With the addition of a new member into the cluster, again the centroid is computed and each seed value is replaced by the respective cluster centroid. In K-means clustering, this process of adding more objects to the group and hence updating the centroid goes on until the centroids are not moving and the objects are not changing the group membership.

+   4. **Decide the number of cluster K:** In the R programming language, there are two libraries, Rcmdr (R Commander) and stats, that support K-means clustering and they use two different approaches. The Rcmdr library needs to be loaded to run the algorithm, but stats is already built into base R. To compute the number of clusters required to represent the data using K-means algorithm is decided by the scree plot using the following formula:

```
```r
> library(Rcmdr)
> sumsq<-NULL
> #Method 1
> par(mfrow=c(1,2))
> for (i in 1:15) sumsq[i] <-
sum(KMeans(r_df_scaled, centers=i, iter.max=500,
num.seeds=50)$withinss)
>
```
```