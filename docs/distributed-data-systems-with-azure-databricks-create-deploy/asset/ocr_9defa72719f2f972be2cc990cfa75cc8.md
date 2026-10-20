In the next section, we will learn how to apply Hyperopt to the Azure Databricks Runtime for Machine Learning to find the best hyperparameters for a certain task, regardless of the algorithm that's applied.

# Hyperopt concepts

In this section, we will go through certain concepts of Hyperopt to identify its core functionalities, as well as how we can use them to efficiently scan the search space for hyperparameters that help minimize or maximize our objective function. As we mentioned previously, when we start using Hyperopt, we must define the following:

+   - The objective function
+   - The search space that we will scan
+   - The database that will be used to save the result of evaluating the points in the search space
+   - The search algorithm that will be used to scan the search space

Here, our main goal is to find the best set of scalars that represent the optimal hyperparameters. Hyperopt allows us to define the search space in detail to provide information regarding where in the search space the target function will perform better. Other optimization libraries regularly assume that the search space is a subset of a larger vector space with the same distribution, which is not usually the case. The way Hyperopt makes us define the search space allows us to perform a more efficient scan.

Hyperopt allows us to define the objective function very simply, but it also has an incremental complexity when we increase the number of parameters, we want to keep track of during our execution. The simplest example is as follows, where we find the value of x that minimizes a linear function, y(x) = x:

```
from hyperopt import fmin, tpe, hp
best_param = fmin(
    fn=lambda x: x,
    space=hp.uniform('x', 0, 1),
    algo=tpe.suggest,
    max_evals=100)
print(best_param)
```