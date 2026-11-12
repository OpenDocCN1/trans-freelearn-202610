![](bbox=[0.235, 0.115, 0.83, 0.505])

Figure 6.7 – Graph for finding the optimal K

Next, we will plot all the data points along with cluster centers to visualize the clusters:

```
plt.figure(figsize=(8, 6))
for i in range(3):
    cluster_data = predictions.filter(
        predictions["cluster"] == i
    ).select("scaled_features").collect()
    cluster_points = [point[0] for point in cluster_data]
    plt.scatter(*zip(*cluster_points), label=f"Cluster {i}")
plt.scatter(*zip(*model.clusterCenters()), c='black',
            s=200, alpha=0.5, label="Cluster Centers")
plt.title("K-Means Clustering")
plt.xlabel("PCA Component 1")
plt.ylabel("PCA Component 2")
plt.legend()
plt.show()
```