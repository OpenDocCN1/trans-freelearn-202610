
# 4

# Regression with Automobile Data

One of the primary objectives of data mining projects is to understand the relationship between various variables and establish a cause and effect relationship between the variable of interest and other explanatory variables. In data mining projects, performing predictive analytics not only entails insights on hidden messages in datasets but also helps in making future decisions which might impact the business outcomes. In this chapter, the reader will get to know the basics of predictive analytics using regression methods, including various linear and nonlinear regression methods using R programming. The reader will be able to understand the theoretical background as well as get practical hands-on experience with all the regression methods using R programming language.

In this chapter, we will cover the following topics:

- Regression formulation
- Linear regression
- Logistic regression
- Cubic regression
- Stepwise regression
- Penalized regression

## Regression introduction

Regression methods help in predicting the future outcomes of a target variable. As an example, here are a few business cases:

- In the sales and marketing domain, how can a business achieve a significant improvement in sales? Can we successfully predict using the if a necessary amendment is made to the drivers of sales?
- In the retail domain, can we predict the number of visitors to a website, so that necessary tech support can be aligned to help operate the site better?
- How can a retail/e-commerce owner predict the number of footfall to their store in a month/week/year?
- In the banking domain, how can a bank predict the number of people applying for home loans, car loans, and personal loans, so that they can maintain their liquid capital to support the demand?
- In the banking domain, prediction of default probability can be estimated using regression methods so that a bank can decide whether to approve a loan/credit card to a customer.
- In the automobile manufacturing domain, the sales unit of vehicles is indirectly proportional to the price of the vehicles and the price of a vehicle is decided by many factors such as usages of different metals/components, and various vehicle features such as RPM, mileage, length, width, and others. How can a manufacturer predict the unit of sales?

There are different methods to perform regression, including both linear as well as non-linear. Regression-based predictive analytics is being used in different industries in a different way. Regression methods support the prediction of a continuous variable, prediction of the probability of a success or failure of a variable, prediction of events based on features, and so on.

## Formulation of regression problem

The formulation of a regression problem is very essential in creating a good regression-based predictive model. A typical approach in building a good predictive model follows a process of converting a business problem to a statistical problem, then converting the statistical problem to a statistical solution, and finally converting the statistical solution to a business solution. The following steps are required to build a good predictive model using regression based methods:

```
Y = f(X) + ε.....( 1 )
Y = f(X₁, X₂....... Xₙ) + ε.....( 2 )
```

Equation 1 shows the formulation of a simple linear regression equation and equation 2 shows the multiple linear equations where many independent variables can be included. The dependent variable must be a continuous variable and the independent variables can be continuous and categorical. We are going to explain the multiple linear regression analysis by taking the Cars93_1.csv dataset, where the dependent variable is the price of cars and all the other variables are explanatory variables. The regression analysis includes:

- Predicting the future values of the dependent variable, given the values for independent variables
- Assessment of model fit statistics and comparison of various models
- Interpreting the coefficients to understand the levers of change in the dependent variable
- The relative importance of the independent variables

The primary objective of a regression model is to estimate the beta parameters and minimize the error term epsilon:

```
Y = β₀ + β₁X₁ + β₂X₂ + β₃X₃.....βₙXₙ ) + ε.....(3)
```

Let's look at the relationship between various variables using a correlation plot:

```
#Scatterplot showing all the variables in the dataset
library(car);attach(Cars93_1)
> #12-"Rear.seat.room",13-"Luggage.room" has NA values need to remove them
> m<-cor(Cars93_1[,-c(12,13)])
> corrplot(m, method = "ellipse")
```

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_125_0.png)

The preceding figure shows the relationship between various variables. From each ellipse we can get to know how the relationship: is it positive or negative, and is it strongly associated, moderately associated, or weakly associated? The correlation plot helps in taking a decision on which variables to keep for further investigation and which ones to remove from further analysis. The dark brown colored ellipse indicates perfect negative correlation and the dark blue colored ellipse indicates the perfect positive correlation. The narrower the ellipse, the higher the degree of correlation.

Given the number of variables in the dataset, it is quite difficult to read the scatterplot. Hence the correlation between all the variables can be represented using a correlation graph, as follows:

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_126_0.png)

Correlation coefficients represented in bold blue color show higher degree of positive correlation and the ones with red color show higher degree of negative correlation.

The assumptions of a linear regression are as follows:

- **Normality**: The error terms from the estimated model should follow a normal distribution
- **Linearity**: The parameters to be estimated should be linear
- **Independence**: The error terms should be independent, hence no-autocorrelation
- **Equal variance**: The error terms should be homoscedastic
- **Multicollinearity**: The correlation between the independent variables should be zero or minimum

While fitting a multiple linear regression model, it is important to take care of the preceding assumptions. Let's look at the summary statistics from the multiple linear regression analysis and interpret the assumptions:

```
> #multiple linear regression model
> fit<-lm(MPG.Overall~.,data=Cars93_1)
> #model summary
> summary(fit)
Call:
lm(formula = MPG.Overall ~ ., data = Cars93_1)
```

The estimated beta coefficients of the model:

```
> #estimated coefficients
> fit$coefficients
(Intercept) Price EngineSize Horsepower
2.808772930 -0.053419142 1.334180881 0.005005690
RPM Rev.per.mile Fuel.tank.capacity Length
0.001107897 0.002806093 -0.639270186 -0.059861997
Wheelbase Width Turn.circle Rear.seat.room
0.330572119 0.233123382 0.026694571 -0.031404262
Luggage.room Weight
0.206757968 -0.008001444
#residual values
fit$residuals
#fitted values from the model
fit$fitted.values
#what happened to NA
fit$na.action
> #ANOVA table from the model
> summary.aov(fit)
Df Sum Sq Mean Sq F value Pr(>F)
Price 1 885.7 885.7 110.224 7.20e-16 ***
EngineSize 1 369.3 369.3 45.959 3.54e-09 ***
Horsepower 1 37.4 37.4 4.656 0.03449 *
RPM 1 38.8 38.8 4.827 0.03143 *
Rev.per.mile 1 71.3 71.3 8.877 0.00400 **
Fuel.tank.capacity 1 147.0 147.0 18.295 6.05e-05 ***
Length 1 1.6 1.6 0.203 0.65392
Wheelbase 1 35.0 35.0 4.354 0.04066 *
Width 1 9.1 9.1 1.139 0.28969
Turn.circle 1 0.5 0.5 0.060 0.80774
Rear.seat.room 1 0.6 0.6 0.071 0.79032
Luggage.room 1 9.1 9.1 1.129 0.29170
Weight 1 63.4 63.4 7.890 0.00648 **
Residuals 68 546.4 8.0
---
Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
11 observations deleted due to missingness
```

From the preceding analysis of variance (ANOVA) table, it is observed that the variables Length, Width, turn circle, rear seat room, and luggage room are not statistically significant at 5% level of alpha. ANOVA shows the source of variation in dependent variable from any independent variable. It is important to apply ANOVA after the regression model to know which independent variable has significant contribution to the dependent variable:
