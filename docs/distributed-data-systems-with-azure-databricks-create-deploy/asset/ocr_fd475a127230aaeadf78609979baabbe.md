+   - Estimator: This is the algorithm that fits on the data to produce a model.
+   - Parameter: The multiple Transformer and Estimators parameters specified together.
+   - Pipeline: The multiple Transformer and Estimators operations combined into a single workflow.

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

4. Now, we can instantiate StringIndexer and DecissionTreeClassifier:

```
indexer = StringIndexer(inputCol="label",
```

```
outputCol="indexLabel")
```

```
model = DecisionTreeClassifier(labelCol="indexLabel")
```

5. Finally, we can chain StringIndexer and DecissionTreeClassifier together into a single workflow:

```
pipeline = Pipeline(stages=[indexer, model])
```

6. So far, we have followed the standard way of creating a normal pipeline. What we will do now is include the CrossValidator MLflow class so that we can run the cross-validation process of the model. The evaluation metrics of each validated model will be tracked by MLflow and will allow us to investigate which hyperparameters yielded the best results.

In this example, we will specify two hyperparameters in the CrossValidator MLflow class to be examined, which are as follows:

a) maxDepth: This parameter determines the maximum depth that the tree can grow in the DecissionTreeClassifier. Deeper trees can yield better results but are more expensive to train and returns models that tend to overfit more.