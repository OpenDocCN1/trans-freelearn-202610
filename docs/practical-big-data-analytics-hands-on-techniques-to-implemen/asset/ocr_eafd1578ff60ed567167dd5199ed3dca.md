These include:

+   - Regression models
+   - Association rules
+   - Decision trees
+   - Random forest
+   - Boosting algorithms
+   - Support vector machines
+   - K-means
+   - Neural networks

Note that in the examples, we have shown the basic use of the R functions using the entire dataset. In practice, we'd split the data into a training and test set, and once we have built a satisfactory model apply the same on the test dataset to evaluate the model's performance.

# Regression models

Regression models range from commonly used linear, logistic, and multiple regression algorithms used in statistics to Ridge and Lasso regression, which penalizes co-efficients to improve model performance.

In our earlier examples, we saw the application of linear regression when we created trend-lines. Multiple linear regression refers to the fact that the process of creating the model requires multiple independent variables.

For instance:

Total Advertising Cost = x* Print Ads, would be a simple linear regression; whereas

Total Advertising Cost = X + Print Ads + Radio Ads + TV Ads, due to the presence of more than one independent variable (Print, Radio, and TV), would be a multiple linear regression.

Logistic regression is another commonly used statistical regression modelling technique that predicts the outcome of a discrete categorical value, mainly for cases where the outcome variable is dichotomous (for example, 0 or 1, Yes or No, and so on). There can, however, be more than 2 discrete outcomes (for example, State NY, NJ, CT) and this type of logistic regression is known as multinomial logistic regression.