The following code snippet transforms the input columns into scaled features by applying the StandardScaler function:

```
scaler = StandardScaler(
    inputCol="features", outputCol="scaled_features",
    withStd=True, withMean=True)
scaler_model = scaler.fit(df)
scaled_df = scaler_model.transform(df)
scaled_df.select("id", "features",
    "scaled_features").show(truncate=False)
```

| id | features | scaled_features |
| :--- | :--- | :--- |
| 0 | [1.0,0.1,-1.0] | [-1.0,-0.6657502859356826,-1.0] |
| 1 | [2.0,1.1,1.0] | [0.0,-0.4841820261350419,0.0] |
| 2 | [3.0,10.1,3.0] | [1.0,1.1499323120707245,1.0] |

Figure 3.22 – Scaled features in a Spark DataFrame

The StandardScaler standardizes the features column by removing the mean and scaling to unit variance. The scaled_features column contains the standardized values.

# MinMaxScaler

This linearly transforms each feature to fit within the specified minimum and maximum values. MinMax scaling is useful for algorithms that are sensitive to feature scaling, such as gradient-based optimization methods.

The following code snippet creates a sample Spark DataFrame:

```
from pyspark.sql import SparkSession
from pyspark.ml.feature import MinMaxScaler
from pyspark.ml.linalg import Vectors
data = [(0, Vectors.dense([1.0, 0.1, -1.0]),),
    (1, Vectors.dense([2.0, 1.1, 1.0]),),
    (2, Vectors.dense([3.0, 10.1, 3.0]),)]
columns = ["id", "features"]
df = spark.createDataFrame(data, columns)
df.show()
```