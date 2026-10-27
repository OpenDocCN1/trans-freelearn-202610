If a time series is completely empty (100% missing values) in the selected training range, the model will fail to train. When this happens, you have the following possibilities:

+   - If this is a data generation issue, regenerate your data, upload it in S3, and reattempt ingestion.
+   - If you have good knowledge of the process that generates this data and know how to fill these values, you can update the time series for this sensor, upload them to S3, and reattempt ingestion.
+   - If you want to remove this signal altogether, you will have to delete your dataset, build a new schema that doesn't include this sensor data, create a new dataset with the new schema, and reattempt ingestion.

Now that you understand the key issues that may happen with your dataset, it is time to conclude this chapter and move to the fun part – training and evaluating a model!

# Summary

In this chapter, you learned how you should organize your dataset before you can train an anomaly detection model with Amazon Lookout for Equipment. You also discovered the dataset we are going to use in the next few chapters and you performed the first tasks necessary to train a model, namely, creating a dataset and ingesting your time series data in it.

With the help of this chapter, you should now have an understanding of what can go wrong at this stage with a few pointers to the main errors that can happen at ingestion time.

In the next chapter, we are going to train and evaluate our first anomaly detection model.