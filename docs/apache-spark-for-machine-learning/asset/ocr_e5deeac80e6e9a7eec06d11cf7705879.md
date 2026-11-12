```
plt.ylabel('Feature 2')
plt.title('Clusters from GMM')
plt.show()
```

Let’s see the output:

![](bbox=[0.27, 0.195, 0.74, 0.51])

Figure 6.16 – Visualization of clusters using GMM

# Power Iteration Clustering (PIC)

**Power Iteration Clustering (PIC)** is a scalable and efficient graph clustering algorithm that employs the concept of power iteration, a method commonly used in eigenvalue solvers, to project high-dimensional data points into a low-dimensional space. This projection is based on the similarity matrix of the data, representing the pairwise similarities between data points as edges in a graph, thereby transforming the clustering problem into a graph partitioning problem.

# _How PIC works_

PIC starts by constructing a similarity matrix that represents the dataset as a graph, where nodes correspond to data points and edges reflect the similarities between these points. The algorithm then performs power iterations on this graph to compute the principal eigenvector of the modified normalized Laplacian matrix of the graph. The resulting eigenvector reflects the steady-state distribution of a random walk on the graph, capturing the essential structure of the data. Data points are then mapped to a low-dimensional space based on their components in the principal eigenvector, and traditional clustering techniques, such as K-means, can be applied in this reduced space to identify clusters.