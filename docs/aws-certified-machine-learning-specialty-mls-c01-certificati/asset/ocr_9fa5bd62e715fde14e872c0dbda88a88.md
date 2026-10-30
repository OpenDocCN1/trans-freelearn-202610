Some other important hyperparameters of the linear learner algorithm are **L1** and **wd**, which play the roles of **L1 regularization** and **L2 regularization**, respectively.

L1 and L2 regularization help the linear learner (or any other regression algorithm implementation) avoid overfitting. Conventionally, we call regression models that implement L1 regularization **Lasso Regression** models, while for regression models with L2 regularization, we call them **Ridge Regression** models.

Although it might sound complex, it is not! Actually, the regression model equation is still the same; that is, **y = ax + b + e**. The change is in the loss function, which is used to find the coefficients that best minimize the error. If you look back at Figure 7.3, you will see that we have defined the error function as **e = (ŷ - y)2**, where **ŷ** is the regression function value and **y** is the real value.

L1 and L2 regularization add a penalty term to the loss function, as shown in the following formulas (note that we are replacing ŷ with ax + b):

L₁ = ( a x + b − y )² + λ | a |

L₂ = ( a x + b − y )² + λ a²

The λ (lambda) parameter must be greater than 0 and manually tuned. A very high lambda value may result in an underfitting issue, while a very low lambda may not result in expressive changes in the final results (if your model is overfitted, it will stay overfitted).

In practical terms, the main difference between L1 and L2 regularization is that L1 will shrink the less important coefficients to zero, which will force the feature to be dropped (acting as a feature selector). In other words, if your model is overfitting due to it having a high number of features, L1 regularization should help you solve this problem.

> **Important note**
> During your exam, remember the basis of L1 and L2 regularization, especially the key difference between them, where L1 works well as a feature selector.

Last but not least, many built-in algorithms can serve multiple modeling purposes. The linear learner algorithm can be used for regression, binary classification, and multi-classification. Make sure you remember this during your exam (it is *not only* about regression models).