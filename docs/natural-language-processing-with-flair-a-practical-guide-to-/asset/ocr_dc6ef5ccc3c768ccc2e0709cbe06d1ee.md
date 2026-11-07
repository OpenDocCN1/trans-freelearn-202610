+   - We can quickly experience the curse of dimensionality. Most real-world model trainers will govern training on more than just a single hyperparameter. This is where things get complicated. In our simplified example where we had a single hyperparameter, we had a search space of 10 possible options. Let's imagine a problem with two such hyperparameters. The number of possible parameter option combinations would be 10². Given three such hyperparameters, the number of possible combinations would be 10³. This clearly suggests exponential growth that can very quickly grow out of proportion. This problem is referred to as the curse of dimensionality because with each extra parameter, we are adding another dimension that needs exploration.

Luckily, there are more sophisticated optimization methods that help us mitigate these problems.

> **Important Note**
> When doing hyperparameter tuning evaluation, it's best to use a dataset that is completely distinct from the data that will be used in our final model. This dataset is called a development (also known as the validation) dataset. Keeping data used for hyperparameter tuning separate from data used for training and evaluation helps us in avoiding overfitting, which would result in choosing the hyperparameters that would overfit our test dataset.

To sum up, hyperparameter tuning in NLP is simply a process that takes in a search space, an optimization method, and an objective function. It then uses the optimization (search) method to train models using a number of different hyperparameter option combinations and returns a set of hyperparameters that result in the best-performing model.

# Optimization methods

Hyperparameter tuning utilizes some of the following commonly used optimization methods:

+   - Grid search
+   - Random search
+   - Bayesian optimization
+   - Evolutionary algorithms

Let's look at each of these methods in detail.