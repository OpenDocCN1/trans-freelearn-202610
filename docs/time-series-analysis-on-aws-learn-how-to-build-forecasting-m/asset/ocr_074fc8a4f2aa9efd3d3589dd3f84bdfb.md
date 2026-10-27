# # Scheduling Regular
# Inferences

In the previous chapter, you trained a model and visualized the events it was able to detect over an evaluation period. Once **Amazon Lookout for Equipment** has trained a model, you can configure and start an inference scheduler that will run your data against it. This scheduler will wake up regularly, look for CSV files in a location on Amazon S3, open the right ones, and run them with your trained model to predict whether anomalous events are present in your new data. This process is called **inference**.

In this chapter, you will learn how to manage such schedulers and how to use the predictions obtained. In other words, you will learn how to use a deployed version of your model and use it in production.

In this chapter, we're going to cover the following main topics:

+   - Using a trained model
+   - Configuring a scheduler
+   - Preparing a dataset for inference
+   - Extracting the inference results