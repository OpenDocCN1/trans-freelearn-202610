In the last line of the output, `Cumulative Proportion`, you can see that the first component explains 62% of the variation, and the first two principal components explain a total of 87% of the variation in the data.

This was based on four original variables. Principal Components have reduced the number of variables by 50%. Think how many variables could be reduced if you started with a feature set of over 100 variables!

# ## All subsets regression

All subsets regression is another method you can use for variable selection. It works by running separate regressions for different combinations of one variable, two variables, three variables at a time, until it has used all of the variables or until a specified stopping point. As part of the output, the best model for each of the combination of variables is then calculated. That will then give you an idea of which is the best single one variable model, two variable model, and so on, so that you can pare down a large list of variables into a smaller number of important ones using some plots and output statistics.

# ### An example à airquality

In this example, we will use the `regsubsets()` function contained within the `leaps` package to determine which variables are important for predicting temperature:

```
install.packages("leaps")
library(leaps)
data(airquality)
str(airquality)
```

In the following `regsubsets()` function call, we specify that we want the best model for each combination of one, two, and three variables. By default, the function will compute the best model for all variables in the model. In the function call, I illustrate specifying a max value, since I want to be able to stop the algorithm after a certain number of variables has been reached. This becomes important when you use data that has a large number of variables,