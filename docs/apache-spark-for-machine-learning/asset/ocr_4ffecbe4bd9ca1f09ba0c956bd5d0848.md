# Linear SVM

A linear SVM is a powerful supervised machine learning algorithm that’s primarily used for classification tasks, and under certain conditions, for regression as well. It operates on the principle of finding the hyperplane that best separates the classes in the feature space. In the context of classification, the SVM aims to find the hyperplane with the maximum margin, which is defined as the maximum distance between the hyperplane and the nearest data point from either class. This approach helps to ensure that the classifier is not only accurate on the training data but also generalizes well to unseen data.

Imagine that you and your friends are in a large park, and you’ve brought a variety of balls to play with: footballs and basketballs. You want to organize a game but to make it fair and fun, you decide to separate the footballs and basketballs into two distinct groups. You lay a long rope on the ground, trying to create a line that divides the two types of balls. The goal is to position this rope so that all footballs are on one side and all basketballs are on the other, maximizing the space between the balls and the rope to avoid any mix-ups when choosing a ball for the game.

This rope is like the decision boundary in a linear SVM. Just as you adjust the rope to maximize the margin between the balls and the rope, the linear SVM algorithm seeks to find the hyperplane (in higher-dimensional space, the “rope” becomes a “plane” or “surface”) that best separates the different categories in the data (footballs and basketballs in this case) with the maximum margin. The balls closest to the rope, which directly influence its placement, are analogous to the “support vectors” in the SVM, which are the critical elements of the dataset that define the decision boundary. Just as the game becomes clearer and fairer when the balls are well-separated, a classification problem becomes more robust and accurate when the decision boundary separates the different classes with a wide margin.

The following figure shows support vectors that help to define the classification boundary:

![](bbox=[0.283, 0.572, 0.696, 0.892])

# Figure 5.7 – Working mechanism of the linear SVM classifier