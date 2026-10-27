# Comparing clustering methods

Here are some of the advantages and disadvantages of various clustering methods:

For K-means clustering:

| Merits and demerits of K-means clustering |  |
|---|---|
| It is simple to understand and easy to interpret | It is not based on robust methodology. The big problem is initial random points |
| It is more flexible in comparison to other clustering algorithms | Different random points display different results. Hence, more iteration is required. |
| Performs well on a large number of input features | Since more iteration is required, it may not be efficient for large datasets from a computational efficiency point of view. It would require more memory to process. |

For SOM:

| Merits and Demerits of SOM |  |
|---|---|
| Simple to understand visually | Only works on continuous data |
| New data points can be predicted visually | Difficult to represent when the dimensions increase |

# References

Lichman, M. (2013). UCI Machine Learning Repository (http://archive.ics.uci.edu/ml). Irvine, CA: University of California, School of Information and Computer Science.

# Summary

In this chapter, we discussed various methods of segmentation for unsupervised data mining problems and their implementations. Clustering is a subjective segmentation method and it requires further investigation such as profiling by cluster and calculating different measures such as mean and median for different variables. If any cluster displays closeness to another cluster and from business point of view they make sense, then they can be combined together. We discussed which model to use where and what the data requirement for each of the models is. In this chapter, we specifically highlighted the importance of data visualization when it comes to representing clustering solutions.