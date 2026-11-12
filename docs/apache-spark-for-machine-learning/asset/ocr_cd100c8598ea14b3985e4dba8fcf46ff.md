Now, let's find the optimal number of clusters:

```
wssse_values = []
evaluator = ClusteringEvaluator(predictionCol='cluster',
    featuresCol='scaled_features', \\
    metricName='silhouette', \\
    distanceMeasure='squaredEuclidean')
for i in range(2, 8):
    KMeans_mod = KMeans(k=i, featuresCol="scaled_features",
    predictionCol="cluster")
    KMeans_fit = KMeans_mod.fit(data_df)
    output = KMeans_fit.transform(data_df)
    score = evaluator.evaluate(output)
    wssse_values.append(score)
    print("Silhouette Score:","score")
```

This will generate the output as shown:

```
Silhouette Score: 0.7178670008193637
Silhouette Score: 0.7851032706829328
Silhouette Score: 0.6726486143551612
Silhouette Score: 0.5810267089116743
Silhouette Score: 0.5446619451331941
Silhouette Score: 0.5228633085259329
```

# Figure 6.6 – Silhouette scores output

The following code snippet plots the graph showing the WSSSE values for a range of cluster sizes:

```
# Plotting WSSSE values
plt.plot(range(1, 7), wssse_values)
plt.xlabel('Number of Clusters (K)')
plt.ylabel('Within Set Sum of Squared Errors (WSSSE)')
plt.title('Elbow Method for Optimal K')
plt.grid()
plt.show()
```

The graph for finding the optimal number of clusters using the elbow method is as shown: