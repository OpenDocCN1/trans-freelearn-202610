# GBM key concepts

The depth or shallowness of a decision tree refers to how many levels of hierarchies it has. The smaller the number of hierarchies, the more shallow the tree. GBM uses successive weak learners, which are trained on a measure of error from the previous model. A weak learner means it has some predictive power but not much. A shallow tree is typically weak, as it is only using one or a small number of features to split the training data.

The resulting predictions are added together to arrive at a final prediction. There are several variants that use different methods of estimating error and determining how shallow to make each tree.

GBMs work by successively dialing in on higher error areas using the error of the previous model to fit the next model, and so on and so forth. This addresses a problem when using a single decision tree. A single tree splits the data into smaller and smaller groups at each branch. This leads to overfitting as it begins to fit noise, especially when it gets into small sample sizes at the ends of the branches. GBMs avoid this by reusing the entire dataset at each successive learned tree:

![](bbox=[0.09, 0.5, 0.93, 0.75])

GBM concept diagram