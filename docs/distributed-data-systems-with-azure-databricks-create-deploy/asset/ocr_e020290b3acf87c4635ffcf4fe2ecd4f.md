3. As usual, we can use the describe method to show the underlying structure of the DataFrame, which in this case has been inferred by Apache Spark:

```
df.describe().show()
```

The output of this command will show us the statistical summaries of the data that's been loaded:

![](bbox=[0.11, 0.22, 0.85, 0.38])

Figure 4.3 – DataFrame information schema

4. Once the file has been loaded, we can write it to a Parquet file, which we will use to query later:

```
df.write.format("parquet").mode("overwrite").partitionBy("province").save("/tmp/covid_parquet")
```

We're doing this so that we can write the table in a format that has been optimized to work in a data lake, such as Parquet.

5. Once the file has been written, we will read it again and use it to create a table. In order to read the DataFrame, we must run the following command:

```
covid_parquet = spark.read.format("parquet").load("/tmp/covid_parquet")
```

Here, the table is being loaded from the /tmp/covid_parquet location.

6. Once we have loaded the Parquet file into Apache Spark, we can run a query on our data. We will run two queries. The will be based on a count and aggregation based on a date. To do this, we will use pyspark sql functions:

```
from pyspark.sql.functions import count
```

```
display(covid_parquet.groupBy("date").agg(count("*").alias("TotalCount")).orderBy("TotalCount", ascending=False).limit(20))
```