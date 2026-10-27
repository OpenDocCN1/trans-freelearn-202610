
- *Automatically consider the human-in-the-loop feedback* that end users can provide when they flag an anomaly as correct or not. Human feedback is immediately reflected in the Amazon Lookout for Metrics model in real time. By automating this step and providing an easy way to incorporate feedback, Amazon Lookout for Metrics reduces the risk of alert fatigue or missed anomalies.
- *No manual retraining decisions:* Amazon Lookout for Metrics *automatically takes the decision to retrain* its model on your behalf. The retraining process is automated and a model will retrain on its own when it detects the need. This doesn’t require any human intervention, letting you focus more time on analyzing the results and providing timely feedback on Amazon Lookout for Metrics’ insights. This dynamic way of working allows Amazon Lookout for Metrics to adapt to changing cycles in your business or operation data, discover and match seasonal trend changes to reduce false alerts, and improve accuracy over time.
- *Build root cause analysis reports* to provide actionable results users can provide feedback on.

Now that you have a good understanding of what Amazon Lookout for Metrics can do for you, let’s dive deeper into how this service works.

## How does Amazon Lookout for Metrics work?

In this section, you will learn how Amazon Lookout for Metrics works by first looking at the different concepts manipulated by the service. You will then dive deeper into how these concepts are orchestrated together to build **detectors** (this is what Lookout for Metrics calls its anomaly detection models). This section will then end with an overview of the pricing model used by this service.

## Key concept definitions

To build models able to spot anomalies in your data, Amazon Lookout for Metrics uses the following concepts and resources:

- **Detector:** Amazon Lookout for Metrics trains ML models to detect outliers in your data. Such a model is called a *detector* in this service. A detector continuously learns from your data so that it gets better at understanding the normal behavior and any expected variability.
- **Datasource:** A datasource is a service that provides time series data that a detector can analyze. Each datasource must provide timestamped records with each record containing both metrics and (optionally) dimensions (the definition of these two concepts follows in this list). Amazon Lookout for Metrics can connect to multiple AWS services as well as third-party services able to integrate with **Amazon AppFlow** (Salesforce and Google Analytics being two examples).
- **Dataset:** Once a detector is connected to your datasource, it ingests a copy of your data and prepares it for analysis. The data ingested by Amazon Lookout for Metrics is stored in a *dataset*. A detector analyzes live continuous data to detect anomalies while learning your data's behavior from historical data. A dataset will contain both historical and continuous data.
- **Measure:** Each row in your dataset is called a **record** and a record can contain multiple key-value pairs (or labeled values) called **fields**. To be usable with Lookout for Metrics, your dataset must have at least *one timestamp field* and at least *one field to monitor*. The monitored field is called a **measure**.
- **Dimensions:** When choosing which field of your dataset will be a suitable measure to monitor, you can also select up to five additional fields that will be used to group measures based on the values of these additional fields. These fields are called *dimensions*. Let's say you are monitoring the sales of a given product across a country: `sales number` will be your measure. If you select another field that contains the city where the sales were made, `city` will be the dimension that will allow Lookout for Metrics to consider `sales number in Paris`, `sales number in New York`, and so on, as independent time series to monitor.
- **Metrics:** Once you have connected your detector to a datasource, you must select certain fields that will be your dataset's measures and additional ones that will be your dimensions. Each combination of measures and dimensions is called a *metric*. In the previous example, `sales number in Paris` and `sales number in New York` were metrics.
- **Severity score:** When an anomaly is detected by Amazon Lookout for Metrics, it ranks it using a *severity score*. This score is a number between 0 and 100, which is an indication of how far from the expected range a given metric value is.
- **Alert:** When a detector spots an anomaly, you can trigger an alert that will have a target such as a notification in an **Amazon Simple Notification Service (SNS)** topic, or more complex processing using an **AWS Lambda** function. This alert is sent when the severity score of an anomaly crosses a certain threshold (that you can configure): the alert will contain the record of the anomaly and you will be able to process it with the language of your choice (using AWS Lambda) or connecting it to any Amazon SNS subscriber that supports webhooks with this service (such as Slack).
- **Feedback:** When an anomaly is detected by a detector, it creates a report with the anomaly details. These details include all the metrics with unexpected values and packages them up in the Amazon Lookout for Metrics console. From there, you can provide your feedback on how relevant this anomaly is for each metric concerned. This feedback is immediately integrated into the model to improve its accuracy.

Let's now have a look at how all these concepts are used by Amazon Lookout for Metrics to train and run anomaly detection models.

## Amazon Lookout for Metrics workflow overview

You do not need to provide any past labels to Amazon Lookout for Metrics to be able to start detecting anomalies in your dataset: the service can start its detection immediately in an unsupervised fashion using only your new live data (the model is trained online and keeps learning as it observes more and more data from your datasource). If you have historical data, you can bootstrap the training by providing it to Amazon Lookout for Metrics. In addition, when the first anomalies are detected, the service switches to semi-supervised mode by asking for your feedback about the detected anomalies and feeds your answers back to the service to gradually improve the model.

The following diagram illustrates how Amazon Lookout for Metrics orchestrates all these tasks together:

![](img/e4ee48104c730845e796d8c469dfeecc_382_0.png)

Figure 13.1 – Amazon Lookout for Metrics overview

Let's dive into this illustration to explain the different steps you have to go through to use Amazon Lookout for Metrics:

- 1. Connecting your data to a service that Amazon Lookout for Metrics can use as a datasource for either historical data or live data:
  - A. Your *historical data* can be pushed to Amazon S3. If you want to use historical data to bootstrap Amazon Lookout for Metrics model training, you can only use Amazon S3 as a datasource to build a detector.
  - B. Your *live time series data* can be available in a database hosted on Amazon **Relation Database Service (RDS)** or from Amazon Redshift (the data warehouse service from AWS). You can also use a metric coming from the AWS log service (Amazon CloudWatch) or collect your data from Amazon AppFlow. The latter can be connected to one of your business applications able to support an Amazon AppFlow data flow.

> **Note**
> 
> At the time of writing this book, the compatible business applications that support an Amazon AppFlow data flow are the following: Amplitude, Dynatrace, Google Analytics, Infor Nexus, Marketo, Salesforce, ServiceNow, Singular, Trendmicro, Vevea, and Zendesk.

- 2. Once your data is available in your datasource of choice, you can use Amazon Lookout for Metrics to connect to it, configure your measures and dimensions (that is, define the metrics you want to monitor), and start your detector. Depending on the granularity of your measures, it may take up to a day for Amazon Lookout for Metrics to understand your data's behavior and start detecting anomalies.
- 3. Once a detector has seen enough data, it will start sending anomalies that you can review: you can give your feedback right from the service console so that Amazon Lookout for Metrics can improve its detector with a human-in-the-loop continuous improvement process. The service console will also allow you to further investigate anomalies by delivering root cause analysis, enabling you to quickly take better actions when an anomaly is triggered.
- 4. You can also configure alerts to receive notifications or trigger an application automation process by connecting Amazon SNS or AWS Lambda, respectively, as targets for Amazon Lookout for Metrics alerts.

You should now have a high-level understanding of how you can integrate this service into your own anomaly detection practice. The last piece of the puzzle is to understand the pricing model so that you can better optimize the way you use this service.
