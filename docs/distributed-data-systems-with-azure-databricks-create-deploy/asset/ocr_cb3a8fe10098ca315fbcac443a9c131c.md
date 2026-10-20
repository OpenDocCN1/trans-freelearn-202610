In the next example, we will read a CSV file, specifying a known schema of the data with the parser mode set to drop malformed data if its type is not as specified. As discussed before, specifying the schema is always a good practice. If the schema of the data is already known, it is better to avoid schema inference. Here, we will read a file with columns for ID, name, and last name, the first as an integer and the rest as strings:

```
from pyspark.sql.types import StructType, StructField
from pyspark.sql.types import DoubleType, IntegerType,
StringType
data_schema = StructType([
    StructField("id", IntegerType()),
    StructField("name", StringType()),
    StructField("lastname", StringType())
])
(sqlContext
.read
.format("com.databricks.spark.csv")
.schema(data_schema)
.option("header", "true")
.option("mode", "DROPMALFORMED")
.load("some_input_file.csv"))
```

Now that we have gone through how to read tabular data from CSV files, we can use the data to extract features from it. These features will allow us to accurately model our data and have more effective machine learning and deep learning models. In the next section, we will learn how to perform feature engineering in Azure Databricks.

# Feature engineering

Machine learning models are trained using input data to later provide as an outcome a prediction on unseen data. This input data is regularly composed of features that usually come in the form of structured columns. The algorithms use this data in order to infer patterns that may be used to infer the result. Here, the need for feature engineering arises with two main goals, as follows:

+   - Refactoring the input data to make it compatible with the machine learning algorithm we have selected for the task. For example, we need to encode categorical values if these are not supported by the algorithm we choose for our model.