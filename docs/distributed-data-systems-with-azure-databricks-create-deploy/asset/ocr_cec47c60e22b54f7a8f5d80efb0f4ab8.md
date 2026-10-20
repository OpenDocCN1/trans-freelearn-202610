4. After that, we are ready to use the data. We will load it as a Spark DataFrame. The code to do this is as follows:

```
file_path = "dbfs:/databricks-datasets/COVID/coronavirusdataset/PatientInfo.csv"
df = spark.read.format("csv").load(file_path,header = "true",inferSchema = "true")
display(df)
```

The result is visible in the following screenshot:

![](bbox=[0.13, 0.26, 0.87, 0.47])

Figure 2.13 – Coronavirus dataset

5. After that we can run some statistics on the data frame:

```
df.printSchema()
df.describe().show()
df.head(5)
```

You can see the result in the following screenshot: