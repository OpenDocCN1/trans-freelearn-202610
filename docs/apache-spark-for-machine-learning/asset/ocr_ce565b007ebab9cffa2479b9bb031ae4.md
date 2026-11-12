| id | features |
|----|----------|
| 0 | [1.0,2.0,3.0,4.0,... |
| 1 | [2.0,3.0,4.0,5.0,... |
| 2 | [3.0,4.0,5.0,6.0,... |

Figure 3.33 – Data in a Spark DataFrame

The following code snippet transforms data using VectorSlicer to extract the features:

```
slicer = VectorSlicer(inputCol="features",
 outputCol="selected_features", indices=[1, 3, 4])
sliced_df = slicer.transform(df)
sliced_df.select("id", "selected_features").show(truncate=False)
```

| id | selected_features |
|----|-------------------|
| 0 | [2.0,4.0,5.0] |
| 1 | [3.0,5.0,6.0] |
| 2 | [4.0,6.0,7.0] |

Figure 3.34 – Vectorslicer features in a Spark DataFrame

The VectorSlicer selects specific features (indices 1, 3, and 4) from the original feature vector. The selected_features column contains the extracted features.

# RFormula

RFormula is a feature transformer in PySpark that allows you to express transformations required to fit a dataset against an R model formula. It supports a limited subset of R operators, including ~, ., :, +, -, *, and ^. The RFormula is useful for creating features from a combination of existing columns.

The following code snippet imports the required libraries, creates a sample Spark DataFrame, and displays the output:

```
from pyspark.sql import SparkSession
from pyspark.ml.feature import RFormula
spark = SparkSession.builder.appName("RFormulaExample").getOrCreate()
data = [(1.0, 1.0, "a"),
       (0.0, 2.0, "b"),
       (0.0, 0.0, "a")]
```