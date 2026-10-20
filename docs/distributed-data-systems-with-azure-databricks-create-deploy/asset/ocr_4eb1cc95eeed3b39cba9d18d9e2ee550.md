2. We can now run featurization over our entire Spark DataFrame. Consider that this operation can take a considerable amount of time since it applies a large model to the full dataset:

```
features_df = images.repartition(16).select(col("path"), featurize_udf("content").alias("features"))
features_df.write.mode("overwrite").parquet("dbfs:/ml/tmp/flower_photos_features")
```

3. Here, we see the process running, which can take some time to dump the results as Parquet files:

![](bbox=[0.11, 0.32, 0.85, 0.49])

Fig.9.5 - Extracting and writing features

Finally, we have computed the extracted features using the pre-trained model and now we are able to use them to train a new model using the domain knowledge extracted from the pre-trained model. This last step is not shown in this example as it is considered a different process.

# Summary

In this chapter, we discussed how we can use TFRecords and Petastorm as libraries to make the process of loading a large amount of data easier to train our distributed deep learning models. This led to us learning how these records are structured, how we can handle expensive operations such as automated schema inference, how we can prepare records to be consumed, and how we can use them not only in the context of deep learning frameworks but also for pure Python applications.

We finished the chapter with an example of how we can leverage having pre-trained models to extract new features based on domain knowledge that later can be applied to extract features to train a new model.

In the next chapter, we will learn how we can fine-tune the parameters of our deep learning models to improve their performance in Azure Databricks.