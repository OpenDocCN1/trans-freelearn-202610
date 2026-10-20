The Koalas API is very helpful for modeling our data and makes it easy to perform the necessary transformations, even when working with the time-series data. In the next section, we will learn how we can deal with possible missing values in our data frames when working in Azure Databricks.

# Handling missing values

Real-life data is far from perfect, and cases of having missing values are really common. The mechanisms in which the data has become unavailable are really important to come up with a good imputation strategy. We call imputation the process in which we deal with values that are missing in our data, which in most contexts are represented as NaN values. One of the most important aspects of this is to know which values are missing:

+   1. In the following code example, we will show how we can find out which columns have missing or null values by summing up all the Boolean output of the Spark isNull method by casting this Boolean output to integers:
+   2. Another alternative would be to use the output of the Spark data frame describe method to filter out the count of missing values in each column and, finally, subtracting the count of rows to get the actual number of missing values, as follows:
+   3. Once we have identified the null values, we can use remove all rows that contain null values by using the na.drop() method of Spark data frames, as follows:
+   4. We can also specify that the rows that have a number of null values greater than 2 be dropped, as follows:

```
from pyspark.sql.functions import col, sum
df.select(*(sum(col(c).isNull().cast("int")).alias(c) for c in df.columns)).show()
```

```
from pyspark.sql.functions import lit
rows = df.count()
summary = df.describe().filter(col("summary") == "count")
summary.select(*((lit(rows)-col(c)).alias(c) for c in df.columns)).show()
```

```
null_df.na.drop().show()
```

```
null_df.na.drop(thresh=2).show()
```