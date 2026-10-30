15. The final cell explains how we have compared the different scores of various modeling techniques to draw a conclusion mathematically. Figure 9.6 clearly shows that the best model to predict house prices in Boston is XGBoost:

```
models = pd.DataFrame({
    'Model': ['Linear Regression', 'Random Forest', 'XGBoost', 'Support Vector Machines'],
    'R-squared Score': [acc_linreg*100, acc_rf*100, acc_xgb*100, acc_svm*100],
    'Adjusted R-squared Score': [adj_R2_linreg*100, adj_R2_rf*100, adj_R2_xgb*100, adj_R2_svm*100],
    'MAE': [mae_linreg, mae_rf, mae_xgb, mae_svm],
    'MSE': [mse_linreg, mse_rf, mse_xgb, mse_svm],
    'RMSE': [rmse_linreg, rmse_rf, rmse_xgb, rmse_svm]})
models.sort_values(by='R-squared Score', ascending=False)
```

|  | Model | R-squared Score | Adjusted R-squared Score | MAE | MSE | RMSE |
|---|---|---|---|---|---|---|
| 2 | XGBoost | 85.799520 | 84.461793 | 2.530958 | 14.828152 | 3.850734 |
| 1 | Random Forest | 82.181317 | 80.502745 | 2.500316 | 18.606282 | 4.313500 |
| 0 | Linear Regression | 71.218184 | 68.506853 | 3.859006 | 30.053993 | 5.482152 |
| 3 | Support Vector Machines | 59.001585 | 55.139415 | 3.756145 | 42.810575 | 6.542979 |

Figure 9.6 – Comparing the models

16. Once you've completed the execution of this notebook, please feel free to shut down the kernel and stop your notebook instance from the SageMaker console. This is the best practice to save on cost.

In the next hands-on section, we will familiarize ourselves with Amazon SageMaker's training and inference instances. We will also use the Amazon SageMaker API to make this process easier. We will use the same notebook instance as we did in the previous example.

# Getting hands-on with Amazon SageMaker's training and inference instances

In this section, we will learn about training a model and hosting the model to generate predicted results. Let's dive in by using the notebook instance from the previous example:

+   1. Sign in to your AWS account at https://console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances.