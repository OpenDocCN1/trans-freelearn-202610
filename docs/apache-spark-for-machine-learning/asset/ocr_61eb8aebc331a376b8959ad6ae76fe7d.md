The following code snippet imports the required libraries, creates a sample Spark DataFrame, and displays the output:

```
```python
from pyspark.sql import SparkSession
from pyspark.ml.feature import UnivariateFeatureSelector
from pyspark.ml.linalg import Vectors
data = [(1.0, Vectors.dense([1.0, 0.1, -1.0])),
        (0.0, Vectors.dense([2.0, 1.1, 1.0])),
        (0.0, Vectors.dense([3.0, 10.1, 3.0]))]
columns = ["label", "features"]
df = spark.createDataFrame(data, columns)
df.show()
```
```

| label | features |
|---|---|
| 1.0 | [1.0,0.1,-1.0] |
| 0.0 | [2.0,1.1,1.0] |
| 0.0 | [3.0,10.1,3.0] |

Figure 3.37 – Data in a Spark DataFrame

The following code snippet transforms data using UnivariateFeatureSelector to extract the features:

```
```python
selector = UnivariateFeatureSelector(
    featuresCol="features", outputCol="selected_features")
selector.setFeatureType("continuous") \
    .setLabelType("categorical") \
    .setSelectionThreshold(1)
selected_df = selector.fit(df).transform(df)
selected_df.select(
    "label", "selected_features").show(truncate=False)
```
```

| label | selected_features |
|---|---|
| 1.0 | [1.0] |
| 0.0 | [2.0] |
| 0.0 | [3.0] |

Figure 3.38 – Univariate features in a Spark DataFrame

The UnivariateFeatureSelector selects the single most important feature based on statistical tests. In this example, we selected the feature with the highest relevance to the target variable.