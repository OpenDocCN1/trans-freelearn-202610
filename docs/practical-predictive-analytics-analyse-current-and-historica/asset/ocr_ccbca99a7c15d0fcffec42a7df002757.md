When you are considering reducing the dimensionality of *rows*, use clustering methods.

# Principal components

**Principal Component Analysis (PCA)** is useful in cases in which you have too many variables and you wish to capture all of the variation of data into one or two variables. Principal component does this using matrix algebra to create linear combinations of all of the variables.

PCA is also used to see which variables have the most influence within each of the principal components, so often it is a way to discard certain variables when a PCA analysis indicates certain variables have little effect on the outcome.

An example using the variables of height and weight is a good one. Since height and weight are positively correlated, choosing to use only height or only weight as an independent variable to predict body mass might not make much of a difference. So, we can create a new variable which is a linear combination of height and weight and use that as a variable instead.

# Clustering

Clustering is a method which groups data into different classes, so that each class is similar to each other. There are various methods that can be used to define similarity. K-Means clustering is probably the most popular method of clustering. This method uses distances measured to assign data observations to the closest class. Clustering is often used in marketing in order to develop different customer segments.

Clustering is an unsupervised algorithm and is subjective. You can specify beforehand how many groups you wish to cluster into. This number is somewhat arbitrary, and if the goal is interpretability, it can yield to different interpretations.

Scatterplots are often used to show data clusters using only two variables