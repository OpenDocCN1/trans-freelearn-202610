```
cov(x, y) = (∑_{i=1}^{n} (x_i - x̄)(y_i - ȳ)) / (n - 1)
```

The sign of covariance is more important than its value; a positive covariance value indicates that both dimensions increase together and vice versa. Similarly, a negative value of covariance indicates that with an increase in one dimension, the other dimension decreases. If the covariance value is zero, it shows that the two dimensions are independent of each other and there is no relationship.

+   4. Calculate the eigen vectors and eigen values from the covariance matrix. All eigen vectors always come from a square matrix but all square matrix do not always generate an eigen vector. All eigen vectors of a matrix are orthogonal to each other.
+   5. Create a feature vector by taking eigen values in order from largest to smallest. The eigen vector with highest eigen value is the principal component of the dataset. From the covariance matrix, the eigen vectors are identified and the eigen values are ordered from largest to smallest:
+   6. Create low dimension data using the transposed version of the feature vector and transposed version of the normalized data. In order to get the transposed dataset, we are going to multiply the transposed feature vector and transposed normalized data.

```
Feature Vector = (Eigen1, Eigen2.....Eigen 14)
```

Let's apply these steps in the following section.

# Practical project around dimensionality reduction

We are going to apply dimensionality reduction procedure, both model-based approach and principal component-based approach, on the dataset to come up with less number of features so that we can use those features for classification of the customers into defaulters and no-defaulters.