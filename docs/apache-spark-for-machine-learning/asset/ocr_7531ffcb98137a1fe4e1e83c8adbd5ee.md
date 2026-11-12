# # Defining evaluator and CrossValidator

The following code snippet sets up cross-validation for different machine learning algorithms. Cross-validation is a technique that's used to evaluate a machine learning model's predictive performance and ensure that it does not overfit the training data:

```
```python
# Define evaluator
evaluator = RegressionEvaluator(labelCol="label"
predictionCol="prediction", metricName="rmse")
```
```

The following code snippet is the cross-validator for linear regression:

```
```python
# Define CrossValidator for each algorithm
cv_lr = CrossValidator(estimator=pipeline_lr,
estimatorParamMaps=paramGrid_lr,
evaluator=evaluator,
numFolds=3)
```
```

The following code snippet is the cross-validator for generalized linear regression:

```
```python
cv_glr = CrossValidator(estimator=pipeline_glr,
estimatorParamMaps=paramGrid_glr,
evaluator=evaluator,
numFolds=3)
```
```

The cross-validator code snippet for the decision tree is as follows:

```
```python
cv_dt = CrossValidator(estimator=pipeline_dt,
estimatorParamMaps=paramGrid_dt,
evaluator=evaluator,
numFolds=3)
```
```

The following code snippet is the cross-validator for random forest:

```
```python
cv_rf = CrossValidator(estimator=pipeline_rf,
estimatorParamMaps=paramGrid_rf,
evaluator=evaluator,
numFolds=3)
```
```

The following code snippet is the cross-validator for gradient-boosted trees:

```
```python
cv_gbt = CrossValidator(estimator=pipeline_gbt,
estimatorParamMaps=paramGrid_gbt,
evaluator=evaluator,
numFolds=3)
```
```