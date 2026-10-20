9. Finally, we can write our data in delta file format to create our delta table. We can do this by specifying a partition by province, which is a column that we believe will be used frequently to filter data, as shown here:

```
covid_parquet.write.format("delta").mode("overwrite").partitionBy("province").save("/delta/covid_delta/")
```

![](bbox=[0.115, 0.215, 0.85, 0.275])

Figure 4.7 – Writing the DataFrame in delta file format

10. The next step is to read the files in delta format to create our delta table in the metastore. We can read and display the data using the following command:

```
covid_delta = spark.read.format("delta").load("/delta/covid_delta/")
display(covid_delta)
```

This way, we can see the results that have been loaded, as shown here:

![](bbox=[0.115, 0.455, 0.85, 0.735])

Figure 4.8 – Reading the delta file into a Spark DataFrame

Now that our DataFrame is associated with the delta file, we can use it to create a delta table and take advantage of the optimization features of Delta Lake.