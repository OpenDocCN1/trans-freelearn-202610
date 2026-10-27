In the case of AI/ML managed services, some data preparation, ingestion tasks, and model management activities are also performed under the hood, allowing you to focus primarily on the problem to solve. Amazon Lookout for Equipment is a scalable, managed service that automates the whole end-to-end anomaly detection pipeline from data ingestion to model deployment and serving inference results. Additionally, the service deals with data preparation under the hood. Amazon Lookout for Equipment can perform the following functions:

+   - It can fill *missing values* in your datasets for you.
+   - It can deal with *misaligned timestamps* (that is, time series that don't share a common date time index between them).
+   - It can *prepare the time series* so that it can be ingested by multiple algorithms. Amazon Lookout for Equipment can prepare the appropriate time series dataset for both deep learning or statistical models suitable for anomaly detection.
+   - It can *compute a smart threshold* based on your historical time series data to decide whether a given data point should be flagged as an anomaly or not. Many unsupervised approaches require you to provide a contamination level, that is, the ratio of outliers or anomalies present in the dataset. This contamination level is used to define a threshold for the decision function that will determine whether a given data point is normal or not. Unfortunately, this is a parameter that you have to provide, and that will likely depend on your datasets. Changing this value can yield significant changes in the result, making the approach difficult to scale beyond one use case. Amazon Lookout for Equipment abstracts away this challenge for you.
+   - It can perform *hyperparameter tuning* to find the best parameters for you.

Now that you have a good understanding of what Amazon Lookout for Equipment can do for you, let's dive deeper into the challenges arising with anomaly detection when you want to tackle it without using this type of service.

# What are the different approaches to tackle anomaly detection?

Before we dive into Amazon Lookout for Equipment, first, we are going to look at a few definitions. In this section, you will read about the different types of anomalies before getting a high-level overview of the different methods you can use to build your own anomaly detection models.