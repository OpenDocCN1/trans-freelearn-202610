
## Automating MLflow tracking with MLlib

In this section, we will exemplify the use of MLflow for tracking the performance of a PySpark MLlib DecisionTreeClassifier model. MLflow will keep track of the model's learning and will also allow us to store all the artifacts used in the process. We will center our attention on examining the hyperparameters that yielded the best results to find the optimal settings. The model will be trained using the MNIST dataset, which is included in the Databricks example datasets. The dataset is in LIBVSM format and is divided into train and test data. It contains two columns – one for the label and the other for the image encoded in 784 features. Let's get started:

- 1. First, we will load the data while specifying the number of features and cache the data in the worker memory:

```
train_data = spark.read.format("libsvm") \
    .option("numFeatures", "784") \
    .load("/databricks-datasets/mnist-digits/data-001/
    mnist-digits-train.txt")
test_data = spark.read.format("libsvm") \
    .option("numFeatures", "784") \
    .load("/databricks-datasets/mnist-digits/data-001/
    mnist-digits-test.txt")
train_data.cache()
test_data.cache()
```

- 2. After this, we can display the number of records in each dataset and display the training data:

```
print("Train images:{} ; test images {}".format(train_data.count(), test_data.count()))
display(train_data)
```

To pass these features to the machine learning model, we need to do some feature engineering. We can standardize these operations into a single pipeline using MLlib, which allows us to pack several preprocessing operations into a single workflow. MLlib Pipelines is very similar to the pipeline implementation in scikit-learn and has five main components. These are as follows:

- - DataFrame: The actual DataFrame that holds our data.
- - Transformer: The algorithm that will transform the data in a features DataFrame into data for a predictions DataFrame, for example.
- - Estimator: This is the algorithm that fits on the data to produce a model.
- - Parameter: The multiple Transformer and Estimators parameters specified together.
- - Pipeline: The multiple Transformer and Estimators operations combined into a single workflow.

Here, we will have a pipeline composed of two steps, which are a StringIndexer to transform the labels from numerical into categorical features, and the DecisionTreeClassifier model, which will predict the label based on the train data in the feature's column. Let's get started:

3. First, let's make the necessary imports and create the pipeline:

```
from pyspark.ml.classification import
DecisionTreeClassifier, DecisionTreeClassificationModel
```

```
from pyspark.ml.feature import StringIndexer
```

```
from pyspark.ml import Pipeline
```

4. Now, we can instantiate StringIndexer and DecisionTreeClassifier:

```
indexer = StringIndexer(inputCol="label",
```

```
outputCol="indexLabel")
```

```
model = DecisionTreeClassifier(labelCol="indexLabel")
```

5. Finally, we can chain StringIndexer and DecisionTreeClassifier together into a single workflow:

```
pipeline = Pipeline(stages=[indexer, model])
```

6. So far, we have followed the standard way of creating a normal pipeline. What we will do now is include the CrossValidator MLflow class so that we can run the cross-validation process of the model. The evaluation metrics of each validated model will be tracked by MLflow and will allow us to investigate which hyperparameters yielded the best results.

In this example, we will specify two hyperparameters in the CrossValidator MLflow class to be examined, which are as follows:

a) maxDepth: This parameter determines the maximum depth that the tree can grow in the DecisionTreeClassifier. Deeper trees can yield better results but are more expensive to train and returns models that tend to overfit more.

b) maxBins: This parameter determines the number of bins that will be generated to discretize the continuous features into a finite set of numbers. It is a parameter that trains the model in a more efficient way when the training is done in a distributed computing environment. In this example, we will start by specifying a value of two, which will convert the grayscale features into either 1 or 0s. We will also test with a value of 4 so that we have greater granularity.

7. Now, we can define the evaluator that we will be using to measure the performance of our model. We will use PySpark MulticlassClassificationEvaluator and then use weightedPrecision as a metric:

```
from pyspark.ml.evaluation import
MulticlassClassificationEvaluator
model_evaluator =
MulticlassClassificationEvaluator(labelCol="indexLabel",
metricName="weightedPrecision")
```

8. Next, we will define the grid of parameters we want to examine:

```
from pyspark.ml.tuning import CrossValidator,
ParamGridBuilder
hyperparam_grid = ParamGridBuilder() \
.addGrid(model.maxDepth, [2, 6]) \
.addGrid(model.maxBins, [2, 4]) \
.build()
```

9. Now, we are ready to create CrossValidator using the previously defined pipeline, evaluator, and hyperparameter grid of values. As we mentioned previously, CrossValidator will keep track of the models we've created, as well as the hyperparameters we've used:

```
cross_validator = CrossValidator(
    estimator=pipeline,
    evaluator=model_evaluator,
    estimatorParamMaps=hyperparam_grid,
    numFolds=3)
```

10. Once we have defined the cross-validation process, we can start the training process. If an MLflow tracking server is available, it will start to log the data from each run that we do, along with all the other artifacts being used in the run under the current active run. If we don't have an active run, it will create a new one:

```
import mlflow
import mlflow.spark
with mlflow.start_run():
    cv_model = cross_validator.fit(train_data)
    test_metric = model_evaluator.evaluate(cv_model.transform(test_data))
    mlflow.log_metric(f'model_metric_{model_evaluator.getMetricName()}', test_metric)
    mlflow.spark.log_model(spark_model=cv_model.bestModel, artifact_path='best-model')
```

11. In the preceding code, we created a new run to track the model. Remember that we are using the with mlflow.start_run(): statement to avoid running into any naming conflicts if we run the cell multiple times. The preceding steps will guarantee that the cv.fit() function returns the best model that it was able to find, after which we evaluate its performance on the test data and then log the results of this evaluation in MLflow:

## chapter_10 (Python)

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

12. Once we have completed the training process, we will be able to see the results on the MLflow UI in the Azure Databricks Workspace, under the Models tab. We can also see the results of the training process by clicking on the experiment icon in the notebook where we are training our model. We can easily compare results from different runs by clicking on Experiment Runs, which will allow us to view all the notebook runs:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_328_0.png)

Figure 10.2 – The MLflow Model Registry UI

We can explicitly look for the specific results of these runs by passing a parameter in the search run box. For example, we can search the results we obtained when we used the value of 6 in the maxDepth hyperparameter by passing params.maxDepth = 6 in the search box.

We can also compare results by creating scatter plots of the different values of the hyperparameters that were used against a specific performance metric, which is useful when we’re trying to find and compare the best set of hyperparameters.

The next section will dive into a more automatic approach of finding the best set of hyperparameters that minimize a certain function. We will do this using the loss function. We will be using Hyperopt to define and explore a search space and find the hyperparameters that minimize the loss function.
