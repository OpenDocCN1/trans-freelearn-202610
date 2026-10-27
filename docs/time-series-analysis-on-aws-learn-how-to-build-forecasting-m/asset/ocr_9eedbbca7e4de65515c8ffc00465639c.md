+   - A feature importance evolution over time. The sensor contributions are only visible when an event is detected. Here, you can easily see that the patterns of the events that were detected before, during, and after the anomaly are very different.

As you can see, learning how to post-process the outputs from Amazon Lookout for Equipment can yield rich insights that can help facilitate proactive inspection or maintenance of your manufacturing process or industrial equipment.

# Summary

In this chapter, you learned how to train your first anomaly detection model with Amazon Lookout for Equipment. Using the dataset you created in the previous chapter, you were able to configure and train a model.

One of the key things you learned from this chapter is how Amazon Lookout for Equipment leverages provided optional labels. Although the service only uses unsupervised models under the hood, these label ranges are used to rank the ones that are best at finding abnormal behaviors located within these ranges.

Last but not least, we took a deep dive into how to read the evaluation dashboard of a trained model and how valuable it can be to go beyond the raw results that are provided by the service.

In the next chapter, you are going to learn how to use your trained model to run regularly scheduled inferences on fresh data.