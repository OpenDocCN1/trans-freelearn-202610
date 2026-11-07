# Hyperparameter tuning in Python

Let's get a taste of what hyperparameter tuning looks like in practice. We will use one of the most popular Python hyperparameter optimization libraries called Hyperopt. First, let's get a general idea of how to use it in practice.

Hyperopt is a Python library that provides an easy-to-use API that requires the following three objects:

+   - A search space
+   - An objective function
+   - An optimization method

Let's look at each of these requirements in detail:

+   - **Search space** is simply the space within which the optimizer will search for different hyperparameter options. The library provides the following parameter expressions:
+     - **Categorical parameters** – Parameter values that are purely categorical and can even be non-scalar (they do not need to be a number). They are provided via the `hyperopt.hp.choice` method.
+     - **Integer parameters** – Integer value parameters obtained via methods such as `hyperopt.hp.quniform` or `hyperopt.hp.qlognormal`.
+     - **Float parameters** – Decimal floating-point numbers obtained via methods such as `hyperopt.hp.normal` (which samples from normal distribution) and `hyperopt.hp.uniform` (which samples from uniform distribution).
+   - **An objective function** that determines how well a certain hyperparameter value combination performs. In deep learning, this is most often the loss function.
+   - **An optimization method** is an algorithm for performing a search within the search space. The options in hyperopt are: random search (`hyperopt.rand.suggest`), TPEs (`hyperopt.tpe.suggest`), and Adaptive TPEs (`hyperopt.atpe.suggest`).

The three preceding concepts are really all Hyperopt needs to find the optimal hyperparameters for you. Let's put this into practice as part of a hands-on exercise in Python.