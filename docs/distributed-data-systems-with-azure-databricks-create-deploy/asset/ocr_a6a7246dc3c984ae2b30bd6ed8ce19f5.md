In the next section, we will briefly see how we can make SQL queries using the Koalas API.

# Using SQL in Koalas

One of the features of Spark DataFrames is that we can query the data in many ways, one of them being using SQL queries to fetch data from tables.

This feature also extends to Koalas DataFrames, where we can make SQL queries using the Koalas API, which supports standard SQL syntax using ks.sql. The query returns the result as a Koalas DataFrame:

```
koalas_df = ks.DataFrame({'year': [2010, 2011, 2012, 2013, 2014],
'c1': [15, 24, 689, 575, 1376],
'c2': [4, 27, 311, 720, 1650]})
ks.sql("SELECT * FROM { koalas_df } WHERE c1 > 100")
```

Using SQL statements to query the data is a feature that acts as a bridge between data scientists used to pandas DataFrames and data engineers or business intelligence analysts who commonly use SQL to query data.

The next section will show how we can transform Koalas DataFrames into Spark DataFrames seamlessly.

# Working with PySpark

You can apply several PySpark features such as the Spark UI, the history server, and so on in Koalas DataFrames, mostly because Koalas is built on top of PySpark. For example, we can easily convert Koalas DataFrames from and to PySpark DataFrames using the Koalas DataFrame to_spark method, which is like the to_pandas method. In turn, a PySpark DataFrame can be converted into a Koalas DataFrame using the PySpark DataFrame to_koalas method, which is an extension of the Spark DataFrame class:

```
koalas_df = ks.DataFrame({'c1': [1, 2, 3, 4, 5],
'c2': [10, 20, 30, 40, 50]})
spark_df = koalas_df.to_spark()
type(spark_df)
spark_df.show()
```