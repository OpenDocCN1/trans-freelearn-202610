As we can see from the features’ importance, three features stand out: blades_angle, motor_torque, and resistance. However, for two of the features, motor_torque and resistance, we could ask: Do these features lead to improved generated power, or do they result from an increase in the power generated? These features are examples of target leakage, as explained next.

# Target leakage

Target leakage, often called “leakage” is a common pitfall in designing and training machine learning models. It occurs when the model is inadvertently given access to the target variable (or some proxy of the target variable) during the training process. As a result, the model’s performance during training may seem impressive, but it performs poorly on new, unseen data because it has effectively “cheated” during training.

Some common examples of how leakage can occur are as follows:

+   - **Time-based leakage**: Suppose you’re trying to predict stock prices for tomorrow. If you include data from tomorrow in your training set (maybe accidentally), that would cause leakage. Similarly, data only available after the fact (such as aggregate data over all stocks) is another example of time-based leakage.
+   - **Preprocessing mistakes**: These happen when you perform a specific operation, such as scaling or normalizing, using statistics that include both the training and test set.
+   - **Incorrect data splits**: For time-series data, using a simple random split might result in future data being present in the training set.
+   - **Contaminated validation sets**: Sometimes, when creating validation or test sets, some data might overlap or be very closely related to the training data, causing optimistic and unrepresentative validation scores.

In our example, motor_torque and resistance are examples of time-based leakage: both metrics can only be measured after power is generated, which is what we are trying to predict. This also illustrates the importance of performing baseline training tests, as problems like these may not be easily found beforehand.

We fix this error by removing the features from our dataset. We can then proceed with model tuning to further improve the model’s performance.

# Model tuning

We utilize Optuna to perform our parameter optimization study. We’ll leverage Optuna’s Tree-structured Parzen Estimator (TPE) sampling algorithm with Hyperband pruning for efficiency. We define our objective function with the required parameters, a pruning callback, and measure the MSE:

```
def objective(trial):
    boosting_type = trial.suggest_categorical("boosting_type",
    ["dart", "gbdt"])
    lambda_l1 = trial.suggest_float (
```