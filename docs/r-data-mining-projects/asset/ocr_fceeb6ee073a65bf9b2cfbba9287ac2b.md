![](bbox=[0.24, 0.14, 0.72, 0.48])

Correlation coefficients represented in bold blue color show higher degree of positive correlation and the ones with red color show higher degree of negative correlation.

The assumptions of a linear regression are as follows:

+   - **Normality**: The error terms from the estimated model should follow a normal distribution
+   - **Linearity**: The parameters to be estimated should be linear
+   - **Independence**: The error terms should be independent, hence no-autocorrelation
+   - **Equal variance**: The error terms should be homoscedastic
+   - **Multicollinearity**: The correlation between the independent variables should be zero or minimum

While fitting a multiple linear regression model, it is important to take care of the preceding assumptions. Let's look at the summary statistics from the multiple linear regression analysis and interpret the assumptions:

```
> #multiple linear regression model
> fit<-lm(MPG.Overall~.,data=Cars93_1)
> #model summary
> summary(fit)
Call:
lm(formula = MPG.Overall ~ ., data = Cars93_1)
```