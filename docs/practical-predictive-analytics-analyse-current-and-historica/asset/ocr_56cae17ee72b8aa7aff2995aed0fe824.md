summing them all together to determine the total distance.

Here is an example of how the centroid is determined for three cluster members that have been assigned to cluster 1. Centroid 1 has been calculated, the vector [186,45] where 186 is the average of the weight of the three members, 1, 2, and 3, and 45 is the average age of members 1, 2, and 3. These will not be the final cluster assignments, but merely a best guess at this point:

| Member | Weight | Age |
|---|---|---|
| 1 | 146 | 25 |
| 2 | 175 | 43 |
| 3 | 236 | 68 |
| Centroid 1 | 186 | 45 |

The next step is to get a better estimate for each cluster assignment. Each member's dimensions are then compared to the centroids of all of the clusters, and then reassigned to the cluster to which it is the closest. This is done using the concept of Euclidean distances.

The formula for Euclidean distance is as follows: