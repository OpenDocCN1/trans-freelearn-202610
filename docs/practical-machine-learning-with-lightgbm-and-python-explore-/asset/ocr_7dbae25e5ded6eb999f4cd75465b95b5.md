To maintain the data distribution, GOSS is applied as follows:

+   1. The data samples are sorted by the absolute value of their gradients.
+   2. The top a × 100% instances are then selected (instances with large gradients).
+   3. A random sample of b × 100% instances is then taken from the rest of the data.
+   4. A factor is added to the loss function (for these instances) to amplify their influence: 1/b^a, thereby compensating for the underrepresentation of data with small gradients.

Therefore, GOSS samples a large portion of instances with large gradients and a random portion of instances with small gradients and amplifies the influence of the small gradients when calculating information gain.

The downsampling enabled by GOSS can significantly reduce the amount of data processed during training (and the training time for the GBDTs), especially in the case of large datasets.

# Best-first tree growth

The most common method for building decision trees is to grow the tree by level (that is, one level at a time). LightGBM uses an alternative approach and grows the tree leaf-wise or best-first. The leaf-wise approach selects an existing leaf with the most significant change in the loss of the tree and builds the tree from there. The downside of this approach is that if the dataset is small, the tree is likely to overfit the data. A maximum depth has to be set to counteract this. However, if the number of leaves to construct is fixed, leaf-wise tree building is shown to outperform level-wise algorithms [5].

# L1 and L2 regularization

LightGBM supports both L1 and L2 regularization of the objective function when training the regression trees in the ensemble. From Chapter 1, Introducing Machine Learning, we recall that regularization is a way to control overfitting. In the case of decision trees, simpler, shallow trees overfit less.

To support L1 and L2 regularization, we extend the objective function with a regularization term, as follows:

```
obj = L(y, F(x)) + Ω(w)
```

Here, L(y, F(x)) is the loss function discussed in Chapter 2, Ensemble Learning – Bagging and Boosting, and Ω(w) is the regularization function defined over w, the leaf scores (the leaf score is the output calculated from the leaf as per step 2.3 in the GBDT algorithm defined in Chapter 2, Ensemble Learning – Bagging and Boosting).

The regularization term effectively adds a penalty to the objective function, where we aim to penalize more complex trees prone to overfitting.