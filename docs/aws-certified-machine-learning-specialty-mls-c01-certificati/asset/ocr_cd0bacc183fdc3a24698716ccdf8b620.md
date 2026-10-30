The simplest way to check the difference between a predicted value (*yhat*) and its actual value (*y*) is by performing a simple subtraction operation, where the error will be equal to the absolute value of *yhat – y*. This metric is known as the **Mean Absolute Error (MAE)**.

Since we usually have to evaluate the error of each prediction, *i*, we have to take the mean value of the errors. The following formula shows how this error can be formally defined:

```
MAE = (1/N) Σ_{i=1}^{N} |y_i - ŷ_i|
```

Sometimes, you might want to penalize bigger errors over smaller errors. To achieve this, you can use another metric, which is known as the **Mean Squared Error (MSE)**. MSE will square each error and return the mean value.

By squaring errors, MSE will penalize the bigger ones. The following formula shows how MSE can be formally defined:

```
MSE = (1/n) Σ_{i=1}^{n} (y_i - ŷ_i)^2
test set
predicted value actual value
```

There is a potential interpretation problem with MSE. Since it has to compute the squared error, it might be difficult to interpret the final results from a business perspective. The **Root Mean Squared Error (RMSE)** works around this interpretation issue, by taking the square root of MSE. Here is the RMSE equation:

```
RMSE = √[ Σ_{i=1}^{n} (ŷ_i - y_i)^2 / n ]
```

RMSE is probably the most used metric for regression models since it can either penalize bigger errors, yet still be easily interpreted.