The following image shows how different scales of the variable could change the hyper plan's projection:

![](bbox=[0.11, 0.19, 0.9, 0.34])

Figure 3.8 – Plotting data of different scales in a hyper plan

On the left-hand side of the preceding image, we can see a single data point plotted in a hyper plan that has three dimensions (x, y, and z). All three dimensions (also known as features) were normalized to the scale of 0 and 1. On the right-hand side, we can see the same data point, but this time, the "x" dimension was not normalized. We can clearly see that the hyper plan has changed.

In a real scenario, we would have far more dimensions and data points. The difference in the scale of the data would change the centroids of each clusters and could potentially change the assigned clusters of some points. This same problem will happen on other algorithms that rely on distances calculation, such as KNN.

Other algorithms, such as neural networks and linear regression, will compute weighted sums using your input data. Usually, these types of algorithms will perform operations such as W1*X1 + W2*X2 + Wi*Xi, where Xi and Wi refer to a particular feature value and its weight, respectively. Again, we will cover details of neural networks and linear models later, but can you see the data scaling problem by just looking at the calculations that we just described? We can easily come up with very large values if X (feature) and W (weight) are large numbers. That will make the algorithm's optimizations much more complex.

I hope you now have a very good understanding about the reasons you should apply data normalization (and when you should not). Data normalization is often implemented in ML libraries as Min Max Scaler. If you find this term in the exam, then remember it is the same as data normalization.