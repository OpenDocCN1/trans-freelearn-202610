Keeping all other parameters constant, one unit change in the independent variable would bring a multiplicative change in the dependent variable and the multiplication factor is the cube of the beta coefficients.

# # Penalized regression

The problem of maximum likelihood estimation comes into picture when we include large number of predictors or highly correlated predictors or both in a regression model and its failure to provide higher accuracy in regression problems gives rise to the introduction of penalized regression in data mining. The properties of maximum likelihood cannot be satisfying the regression procedure because of high variability and improper interpretation. To address the issue, most relevant subset selection came into picture. However, the subset selection procedure has some demerits. To again solve the problem, a new method can be introduced, which is popularly known as penalized maximum likelihood estimation method. There are two different variants of penalized regression that we are going to discuss here:

+   - **Ridge regression:** The ridge regression is known as L2 quadratic penalty and the equation can be represented as follows:

![](bbox=[0.408, 0.521, 0.591, 0.594])

In comparison to the maximum likelihood estimates, the L1 and L2 method of estimation shrinks the beta coefficients towards zero. In order to avoid the problem of multicollinearity and higher number of predictors or both typically in a scenario when you have less number of observations, the shrinkage methods reduces the overfitted beta coefficient values.

Taking the `Cars93_1.csv` dataset, we can test out the model and the results from it. The lambda parameter basically strikes the balance between penalty and fit of log likelihood function. The selection of lambda value is very critical for the model; if it is a small value then the model may overfit the data and high variance would become evident, and if the lambda chosen is very large value then it may lead to biased result.

```
```r
> #installing the library
> library(glmnet)
```
```