is, regression problems [2]. With CART, decision nodes can also split continuous numerical input variables to support regression, typically using a threshold (for example, x <= 0.3). When reaching a leaf node, the mean or median of the remaining numerical range is generally taken as the predicted value.

When building classification trees, only impurity is used to determine splits. However, with regression trees, impurity is combined with other criteria to calculate optimal splits:

+   * The MSE (or MAE)
+   * Half Poisson Deviance

A detailed mathematical explanation of each is available at https://scikit-learn.org/stable/modules/tree.html#regression-criteria.

scikit-learn uses an optimized version of CART to build decision trees.

# Overfitting in decision trees

One of the most significant disadvantages of decision trees is that they are prone to overfitting. Without proper hyperparameter choices, C4.5 and other training algorithms create overly complex and deep trees that fit the training data almost exactly. Managing overfitting is a crucial part of building decision trees. Here are some strategies to avoid overfitting:

+   * **Pruning**: As mentioned previously, we can remove branches that do not contribute much information gain; this reduces the tree's complexity and improves generalization.
+   * **Maximum depth**: Limiting the depth of the tree also avoids overly complex trees and avoids overfitting.
+   * **Maximum number of leaf nodes**: Similar to restricting depth, limiting the number of leaf nodes avoids overly specific branches and improves generalization.
+   * **Minimum samples per leaf**: Setting a minimum limit on the number of samples a leaf may contain (stopping splitting when the sub-dataset is of the minimum size) also avoids overly specific leaf nodes.
+   * **Ensemble methods**: Ensemble learning is a technique that combines multiple models to improve the prediction over an individual model. Averaging the prediction of multiple models can also reduce overfitting.

These strategies can be applied by setting the appropriate hyperparameters. Now that we understand how to build decision trees and strategies for overfitting, let's look at building decision trees in scikit-learn.

# Building decision trees with scikit-learn

It is time to examine how we may use decision trees by training classification and regression trees using scikit-learn.