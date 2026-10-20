# chapter_10 (Python)

```
from pyspark.ml.evaluation import MulticlassClassificationEvaluator
model_evaluator = MulticlassClassificationEvaluator(labelCol="indexLabel", metricName="weightedPrecision")
from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
hyperparam_grid = ParamGridBuilder() \
    .addGrid(model.maxDepth, [2, 6]) \
    .addGrid(model.maxBins, [2, 4]) \
    .build()

cross_validator = CrossValidator(
    estimator=pipeline,
    evaluator=model_evaluator,
    estimatorParamMaps=hyperparam_grid,
    numFolds=3
)
import mlflow
import mlflow.spark
with mlflow.start_run():
    cv_model = cross_validator.fit(train_data)
    test_metric = model_evaluator.evaluate(cv_model.transform(test_data))
    mlflow.log_metric(f"model_metric_{model_evaluator.getMetricName()}", test_metric)
    mlflow.spark.log_model(spark_model=cv_model.bestModel, artifact_path="best-model")
```

* (63) Spark Jobs
MLlib will automatically track trials in MLflow. After your tuning fit() call has completed, view the MLflow UI to see logged runs.

Use the MLflow tracking API to record runs from this notebook. Learn more

Figure 10.1 – Tracking a training run using MLflow