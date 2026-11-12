
## Unsupervised learning models

Unsupervised problems are more exploratory in nature. In an unsupervised learning context, one doesn’t specify a target variable, or even have any idea what something should be. A data scientist is usually given a set of attributes, and then asked to derive some relationships, or discover some new attributes which are not obvious from looking at the data.

As an example, during an exploratory customer analysis, you could look at different attributes of customers, such as age, gender, and sales history, which could then lead you to create new variables (which hadn’t existed before), which would describe each customer as best, good, or average. You could then choose an appropriate algorithm to suit this problem which might be a clustering algorithm, a decision tree, or a support vector machine.

Another example would be to use a neural network algorithm with some financial trading data in order to uncover some potential fraud or anomaly findings.

Usually, you will match up an unsupervised algorithm (such as neural networks) cast to an unsupervised setting (find fraud), but that is not always the case. You may find that you are able to incorporate traditional supervised techniques such as regression to find results which are totally unexpected using post-diagnostic tools which would examine such thing as residuals and outliers. That might lead you to other explorations which could cause you to reformulate your problem.

Finally, supervised learning and unsupervised learning often work hand in hand. It is possible to find yourself creating a new variable, discovered through an unsupervised process, which in turn would lead to incorporating this as a latent variable into a traditional supervised algorithm, such as a decision tree.

## Regression techniques

Regression analysis (which is considered a supervised learning algorithm) dates back to the early 1800s when Gauss and Legendre utilized these techniques to measure the trajectories of the planets around the sun ([https://en.wikipedia.org/wiki/Regression_analysis](https://en.wikipedia.org/wiki/Regression_analysis)). The regression algorithm's usage is still going strong within the predictive analytics community due to its large base of literature and its ability to adapt to a wide range of problems.

Linear regression is the basic regression technique to use when your target variable is continuous. Linear regression is built upon the concept of ordinary least squares, and the functional form of the model is:

```
$$y_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \dots + \beta_p x_{ip} + \varepsilon_i$$
```

The preceding formula illustrates that linear regression models are additive, that is, the results of the prediction are calculated by summing the cross-product values of all of the independent variables, and then adding an intercept (first term of the formula), and error term (last term of the formula). That calculation yields the *prediction* for the dependent or target variable (y). The linear regression line is fitted via the method of **ordinary least squares** (OLS), in which the squared (residual) differences between the observed value and the predicted distances on the regression line are minimized. Squared differences are used, rather than the actual differences, since we are really interested in the magnitude, not the direction of the differences. Squaring the results eliminates the negative or positive sign. The residuals are often used to test the errors resulting from the predictive model.

### Advantages of regression

One advantage to using linear regression is that it facilitates easy interpretation of the linear coefficients which are attributed to each variable. Specifically, the slope of each variable in a regression model (the $\hat{\beta}$ in front of each of the regression terms), will indicate the change in the prediction for each 1-unit change in the corresponding independent variable, with all other variables in the model held constant. There is a fair amount of flexibility in using linear models in that you can also model nominal (categorical) independent variables as well as continuous ones, and incorporate a fair degree of non-linear relationships by using variable transformations and interactions. The body of knowledge for linear regression is vast, and based heavily on probability theory.

While linear regression can be very powerful when used correctly, here are some of the major assumptions of linear regression which you should pay attention to:

- Independence: the observations have no relationship with each other. This assumption will often be violated when you are dealing with time-series data. Knowing how the data was sampled will help with understanding whether or not the data is independent and randomly sampled.

- Linearity: there is a linear relationship between the independent and dependent variables. Other kinds of relationships can exist; for example there can be a curvilinear relationship between the variables. The second plot of the first row in the Wikipedia reference for Anscombe's quartet shows how this can occur.

- Normality: the errors term in the regression model is normally distributed. Intuitively, normally distributed residuals are good to have since they imply that the correct variables are all accounted for in your model, and there is nothing left to explain. You are just left with random error.

- Equal variances of errors: the error terms also have constant variance. For example, non equality of variances can mean that the errors in prediction are increasing or decreasing as values of the independent variables grow larger or smaller.

If any of the assumptions are grossly violated, you probably should not be using linear regression. I say grossly since there is a certain degree of tolerance that any model has for assumptions, and there are statistical tests that can be used to measure that degree for some of the assumptions. However, if your data meets the assumptions of linear regression, the predictions can be quite powerful.

## Generalized linear models

Generalized linear models (GLMs) refer to a larger framework of prediction techniques which can include linear regression, logistic regression (used to predict binary outcomes), and poisson regression (used to predict counts). They are a generalization of linear regression techniques which allow you to work with other distributions which have non-normal error terms. GLMs can be implemented in R by using the glm package, in which you supply a link function to specify which distribution you are modeling. That makes it easier to work with different types of models within a single package, using standard syntax.

### Linear regression using GLM

In Chapter 1, Getting Started with Predictive Analytics, our original example that we used to predict women's heights based upon women's weights used the lm package. We could also have used the glm package, specifying family=Gaussian as the link function:

```
lm_output <- lm(women$height ~ women$weight)
```

or:

```
glm_output <- glm(women$height ~ women$weight, family=gaussian)
```

## Logistic regression

A very popular use of GLM is via logistic regression. This is the type of regression to use when your target variable is a binary value, that is, it can only take on two values. These two binary values usually take the form:

- Something occurred (binary value = 1)
- Something did not occur (binary value = 0)

These two values typically vary according to the type of industry. Here are some examples of binary responses in different industries:

- **Marketing**: Did the customer respond to a specific offer or not?
- **Health care**: Was a specific drug effective in treating a condition or was it not?
- **Financial**: Did a specific trading strategy lead to a profit or not?
- **Web tracking**: Did a customer abandon a shopping cart at the checkout or not?

Virtually every industry will have problems which can be formulated in terms of binary outcomes. In fact, sometimes standard linear regression problems are often recast, so instead of predicting the specific values for the target value, the problem will be reformulated into a binary problem. Often this is done to take advantage of some of the relaxed assumptions of logistic regression.

You may ask, "Why not use regular linear regression for these kinds of problems?" Unfortunately, linear regression itself will not work when you have a binary outcome variable for a variety of reasons. Here are two important ones:

- First of all, if regular regression is used, the results will extend way beyond the bounded ranges of 0 and 1, since a line is infinite in both directions.
- Second, one of the requirements of regular regression is that the residual
