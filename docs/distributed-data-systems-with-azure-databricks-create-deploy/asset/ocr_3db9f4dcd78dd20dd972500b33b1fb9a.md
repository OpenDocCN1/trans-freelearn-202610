+   - **MLlib**: A library with several cluster-computing optimized machine learning algorithms for classification, regression, clustering, data preparation, utilities, and more.
+   - **RDDs**: This stands for **Resilient Distributed Datasets**. This is a layer of abstraction that sits on top of a distributed set of data and is the building block for any processes running in Azure Databricks.

Because our focus will be placed on working with data, it is very important for us to understand PySpark DataFrames; therefore, we will review the basics of operating with them in the next section.

# Operating with PySpark DataFrames

In this section, we will introduce common PySpark DataFrame functions using Python that can be executed in any Azure Databricks notebook. The steps are as follows:

+   1. Our first step is to create a PySpark DataFrame. We can do so by using the `createDataFrame` method to create a DataFrame named `df` with columns named `id` and `value` in the following way:

```
data = [(1, 'c1', 'a1'), (2, 'c2', 'a2')]
columns = ['id', 'val1', 'val2']
df = spark.createDataFrame(
    data,
    columns)
```

When using the `createDataFrame` method, we must take into account the following:

+   a) If the schema is defined by a list of column names, the type of each column will be inferred from data.
+   b) If the schema is defined using `pyspark.sql.types.DataType` or a datatype string, it must match the real data.

+   2. For example, to create a blank DataFrame, we can first define the schema of data using `StructField` and store the information in a variable named `schema`:

```
columns = [
    StructField("id", IntegerType(), True),
    StructField("value", StringType(), True),
]
schema = StructType(columns)
```