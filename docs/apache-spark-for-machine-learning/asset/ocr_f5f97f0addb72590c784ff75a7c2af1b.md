| id | features |
|---|---|
| 0 | [1.0,0.1,-1.0] |
| 1 | [2.0,1.1,1.0] |
| 2 | [3.0,10.1,3.0] |

Figure 3.23 – Data in a Spark DataFrame

The following code snippet transforms the input column by applying the MinMaxScaler function:

```
scaler = MinMaxScaler(
    inputCol="features", outputCol="scaled_features")
scaler_model = scaler.fit(df)
scaled_df = scaler_model.transform(df)
scaled_df.select("id", "features",
    "scaled_features").show(truncate=False)
```

| id | features | scaled_features |
|---|---|---|
| 0 | [1.0,0.1,-1.0] | [(3,[],[()]] |
| 1 | [2.0,1.1,1.0] | [0.5,0.1,0.5 |
| 2 | [3.0,10.1,3.0] | [1.0,1.0,1.0] |

Figure 3.24 – MinMax-scaled features in a Spark DataFrame

MinMaxScaler scales the features column to the [0 , 1] range. The scaled_features column contains the scaled values.

# Normalizer

It normalizes the feature vectors by dividing each value by the Euclidean norm (L2 norm). Normalization is useful when you want to compare vectors based on their direction rather than magnitude.

The following code snippet creates a sample Spark DataFrame:

```
from pyspark.sql import SparkSession
from pyspark.ml.feature import Normalizer
from pyspark.ml.linalg import Vectors
data = [(0, Vectors.dense([1.0, 0.1, -1.0])), ,
    (1, Vectors.dense([2.0, 1.1, 1.0])), ,
    (2, Vectors.dense([3.0, 10.1, 3.0])), ]
columns = ["id", "features"]
df = spark.createDataFrame(data, columns)
df.show()
```