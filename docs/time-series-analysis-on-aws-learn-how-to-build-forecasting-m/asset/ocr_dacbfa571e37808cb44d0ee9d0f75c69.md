As you can see, defining anomalies in a multivariate setting now becomes very challenging as it requires you to build a holistic view of every single signal while keeping in mind the relationship of each signal with every other signal. This is where the capability of deep learning algorithms to uncover complex non-linear relationships between multiple time series signals comes in handy!

Now you are ready to dive into how Amazon Lookout for Equipment can help you to tackle all of these challenges.

# # How does Amazon Lookout for Equipment work?

In this section, you will learn how Amazon Lookout for Equipment works by first looking at the different concepts manipulated by the service. Then, you will dive deeper into how these concepts are orchestrated together to build anomaly detection models. This section will then end with an overview of the pricing model used by this service.

# ## Defining the key concepts

To build anomaly detection models, Amazon Lookout for Equipment deals with the following concepts and resources:

+   - **Tags:** A tag is the name of a time series and can be linked to a sensor, an equipment-monitored condition, or a process variable. In this chapter, we will use the terms **tags**, **signals**, and **sensors** interchangeably.
+   - **Datasets:** A dataset is a container to host your time series data. The Amazon Lookout for Equipment algorithms use these datasets to train their models. Each dataset is defined by a schema where you can define the list of time series tags.
+   - **Components:** When defining the data schema of a dataset, you have the ability to group tags into components. This is an artificial construct that has no impact on the modeling process. However, it will give you the ability to easily train models on subsets of sensors linked to different components. Depending on what you are trying to model, you can use the component level to structure your sensors into different pieces of equipment on a single manufacturing line, regroup them in different facilities, or choose to organize them depending on their unit of measure.