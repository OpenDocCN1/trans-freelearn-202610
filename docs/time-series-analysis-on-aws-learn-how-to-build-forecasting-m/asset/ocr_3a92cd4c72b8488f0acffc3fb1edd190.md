# # Technical requirements

No hands-on experience in a language such as **Python** or **R** is necessary to follow along with the content from this chapter. However, we highly recommend that you read this chapter while connected to your own AWS account and open the Amazon Lookout for Equipment console to run the different actions on your end.

To create an AWS account and log in to the Amazon Lookout for Equipment console, you can refer to the technical requirements of *Chapter 2, An Overview of Amazon Forecast*.

# # Using a trained model

The following diagram illustrates how Amazon Lookout for Equipment works at inference time:

![](bbox=[0.142, 0.37, 0.865, 0.537])

Figure 11.1 – Inference scheduler overview

Let's dive into the different steps of this pipeline:

+   1. New time series are generated: depending on your use case, you might collect new sensor data directly from your piece of equipment or directly access a piece of software such as a **historian**.
+   2. As you did at training time, you will need to push this fresh data to a location on Amazon S3 (you will see in the following *Configuring a scheduler* section how this location is configured).