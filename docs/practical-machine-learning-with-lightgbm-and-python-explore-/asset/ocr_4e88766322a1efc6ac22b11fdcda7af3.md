algorithm, it's easier to debug what might be going wrong or isolate data instances that may be causing the issue.

# Model selection

For our wind turbine data, we use a linear regression model to establish a baseline and validate the data's suitability for modeling.

We also train a random forest regression model as a point of comparison against our LightGBM model. It's also good practice, if budget allows, to train more than one model using a different learning algorithm, as specific problems may be better suited to particular algorithms.

Finally, we train a LightGBM regressor as our primary model.

# Model training and evaluation

From the EDA, we saw that the generator temperature is redundant (due to the correlation with motor torque). As such, we exclude it from the training data:

```
X = train_df.drop(columns=["windmill_generated_power (kW/h)", axis=1)
y = train_df["windmill_generated_power (kW/h)"]
```

Unlike LightGBM, neither linear regression nor scikit-learn's random forest regressor can automatically deal with categorical features.

We, therefore, use get_dummies from pandas to encode the features for training. The get_dummies operation performs a process known as **one-hot encoding**. The categorical value is decomposed into as many binary (0 or 1) columns as there are unique values. The corresponding value is marked with 1 (one-hot) for each pattern, and other values are marked with 0. For example, consider the cloud level feature: there are three categories (medium, low, and extremely low). A row in our dataset with a medium cloud level would be encoded as 100 (three separate columns). Similarly, a low cloud level is encoded as 010, and so on.

Performing one-hot encoding allows algorithms, such as linear regression, that only support numerical columns to model the data at the cost of increased memory usage for the additional columns.

As stated in the problem definition, we'll use two metrics to evaluate the models: the coefficient of determination and the MSE. Both are calculated using five-fold cross-validation.

We can now proceed to train our linear, random forest, and LightGBM regressors:

```
X_dummies = pd.get_dummies(X)
linear = LinearRegression()
scores = cross_val_score(linear, X_dummies, y)
scores = cross_val_score(linear, X_dummies, y, scoring="neg_mean_squared_error")
forest = RandomForestRegressor()
```