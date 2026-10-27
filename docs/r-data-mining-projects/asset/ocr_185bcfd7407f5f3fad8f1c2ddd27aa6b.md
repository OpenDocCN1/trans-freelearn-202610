```
[5,] 77548 356 -50654 -38425 7801 20782 -19707 -28953 -88730 -12624 -9028 -4467
[6,] 34793 -42350 -40802 -22729 -2772 9948 -17731 -2654 -91543 37090 2685 -10960
PC13 PC14
[1,] -862 501
[2,] -238 -97
[3,] 2522 -4342
[4,] -1499 -4224
[5,] 3331 -9412
[6,] -1047 -10120
```

# Parametric approach to dimension reduction

To some extent, we touched upon model-based dimension reduction in Chapter 4, Regression with Automobile Data, Logistic regression, where we tried to implement regression modelling and in that process we tried to reduce the data dimension by applying Akaike Information Criteria (AIC). Bayesian Information Criteria (BIC) such as AIC can also be used to reduce data dimensions. As far as the model-based method is concerned, there are two approaches:

+   - **The forward selection method**: In forward selection method, one variable at a time is added to the model and the model goodness of fit statistics and error are computed. If the addition of a new dimension reduces error and increases the model goodness of fit, then that dimension is retained by the model, else that dimension is removed from the model. This is applicable across different supervised based algorithms, such as random forest, logistic regression, neural network, and support vector machine-based implementations. The process of feature selection continues till all the variables get tested.
+   - **The backward selection method**: In backward selection method, the model starts with all the variables together. Then one variable is deleted from the model and the model goodness of fit statistics and error (any loss function pre-defined) is computed. If the deletion of a new dimension reduces error and increases the model goodness of fit, then that dimension is dropped from the model, else that dimension is kept by the model.