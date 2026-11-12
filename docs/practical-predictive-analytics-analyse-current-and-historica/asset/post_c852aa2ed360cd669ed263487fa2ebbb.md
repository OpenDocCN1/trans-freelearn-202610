
## Step 4 modeling

In the modeling stage, you will pick an appropriate predictive modeling technique that fits your problem and apply it to your data. There are several factors which influence the selection of a model:

- 1. Who will use the model?
- 2. How will the model be used?
- 3. What are the assumptions of the model?
- 4. How much data do I have?
- 5. How many variables do I need to use?
- 6. What is the accuracy level needed by the model?
- 7. Am I willing to trade some accuracy for interpretability?

Particularly related to the last point is the concept of **bias** and **variance**.

Bias is related to the ability of a model to approximate the data. Low bias algorithms are able to fit the data with little error. While this may seem to an advantage all of the time, it can result in a complex model which is unstable, and difficult to explain. On the other hand, a high bias model is relatively simple to explain (like linear regression), but may sacrifice some accuracy for explainability, and stability. You will usually start by looking at your data and matching it up with a choice of potential algorithms. A linear regression, for example, which tries to fit data which forms a U-shaped distribution would not be an appropriate algorithmic choice for that problem, regardless of the number of parameters selected or how the coefficients were adjusted. For example, 5-star ranking reviews are often skewed toward very high or very low ratings and linear regression would not be an appropriate choice.

Here is an example showing how linear regression would be a poor choice:

```
set.seed(1010)
x <- sample(c(1,2,3,4,5),100,replace=TRUE,prob=(c(.25,.05,.05,.3,
y <- data.frame(rating=x,custid=seq(1:100))
hist(y$rating, prob=TRUE, main="Customer Ratings")
lines(density(y$rating))
```

It would be a much better fit if a regression problem involved sales prediction from inventory. Low bias models tend to result in over fitting and high bias models have lower accuracy but are simpler to explain.

Variance is related to how a model would change when supplied with different data. Low variance is also desirable.

Decision trees are an example of an algorithm, which tends to have low bias (over fits) but can come up with completely different results when given a new training sample (high variance). Even adding one additional observation to an existing decision tree model can result in a totally different tree.

To develop a good predictive model, you acknowledge that there ultimately will be a compromise or tradeoff between bias and variance. To learn more about the bias-variance trade-off, check out some of the external references in *Wikipedia*.

### Description of specific models

Here are some short descriptions of some of the models that we will be covering, with some short code examples.

### Poisson (counts)

A Poisson model is used to model counts of things. That could be the number of insurance claims filed in a given month, the number of calls which are received in a call center in a given minute, or the number of orders which are sold for a particular item. The Poisson distribution is the appropriate way for modeling count data since all data is positive and the range of the distribution is bound by 0 and infinity. The classic way of modeling a Poisson model is through the R `glm()` function using a `poisson` link function:

```
model.poisson <- glm(count ~ v1+v2+v3, data=inputdata, family=poi
```

Note that the preceding model specified merely shows the model in a generalized form. Do not try to run it since there are no variables other than the existing ones `v1`, `v2`, `v3` or `count`. However, what the model specification says is that you will run a Poisson model via the following general steps:

- The model will be run via the `glm()` function with some dependent variable to the left of the ~
- The independent variable will be supplied to the right of the ~
- The `Data=` parameter will supply an input dataset
- The `family=` parameter will specify the type of general linear model that you will be running, and, in this case, it will be a Poisson model

To try a Poisson model on real data, we can use the `warpbreaks` data, which is included with R.

- First at the console, enter `help (warpbreaks)` to get a description of the dataset:

| [,1] | breaks | numeric | This is the number of breaks |
|---|---|---|---|
| [,2] | wool | factor | This is the type of wool (A or B) |
| [,3] | tension | factor | This is the level of tension (L, M, or H) |

Then, set up and execute the model using the `glm()` function. We are predicting the number of breaks, using the type of wool and level of tension. Note that we need to add a `summary()` function after the `glm()` function in order to see the output, since running the `glm()` function just assigns the output to an object named the `model.poisson` model.

```
model.poisson <-glm(breaks~wool+tension, data=warpbreaks, family=
summary(model.poisson)
```

### Logistic regression

Logistic regression is one of the oldest and stable techniques that one can use for classification. Logistic regression, linear regression, and Poisson regression are all considered **General Linear Models (GLM)**. However, in the case of logistic regression, the predicted value can only be 0, or 1.
Fortunately, this corresponds with many use cases, such as whether or not a customer will leave, or whether or not a hurricane will appear. If you are already familiar with multiple linear regressions, logistic regression should be easier to understand, since you should already be familiar with concepts such as specifying multiple independent variables, and the use of mathematical functions, such as log and exp, which can smooth the variables in the model and force it to be more linear.

Logistic regression is also useful, in that it produces an odd ratio. An odd ratio is the probability that an event will occur divided by the probability that the event will not occur.

A standard logistic regression is called in R via the `glm()` function.

### Note

The call to `glm()` contains a specification for a `link` function.
The `link` function specifies which kind of model or distribution will be used for the linear model. For logistic regression, use `family=binomial()` to specify logistic regression.

The simplest general form of the `glm()` function is:

```
Model.logistic <- glm(Target~v1+v2+v3,data=sourcedata,family=bino
```

### Support vector machines (SVM)

**Support vector machines (SVM)** can also be used to predict a binary class.
SVM projects the data into a higher dimensional space so that hyperplanes can be used to separate the classifiers. SVMs can be very accurate but difficult to interpret and computationally expensive. They are a classic example of a low bias algorithm.

Here is a simple example of using an SVM to predict whether a person is satisfied based upon the day of the week and whether or not it is a payday.

(The vector element is marked as `1` in the `payday` vector, which can be interpreted as Friday if you start counting from Sunday.)

```
library(e1071)
satisfied = factor(c(F,F,F,F,F,T,F))
day = c(1,2,3,4,5,6,7)
payday = c(0,0,0,0,0,1,0)
satisfaction.df = data.frame(day=day, payday=payday, satisfied=sa
model.svm = svm(satisfied ~ day + payday, data = satisfaction.df)
plot(model.svm, satisfaction.df, main="", col=c('grey','black'))
```

As you can see from the following plot, the decision line is a curve near the upper-right part of the quadrant, and the area where payday is close to 1 (yes), and day of week is close to 4, 5, 6, 7 (later part of the week). So we can interpret that as saying people are satisfied on payday and at the end of the week. However, this is a very low dimension example (rendered in two axes) that we used to illustrate the concept. Higher dimensional examples are not that easy to interpret:

### SVM classification plot

![](img/672c0cf8ba3c000a6529b117ed96945e_91_0.png)

### Decision trees
