# SNS topic Info

Choose an Amazon SNS topic and a role that gives the detector permission to send notifications to it.

SNS topic

Choose a topic in the current Region or create one.

# Service role Info

Choose a service role with access to your topic, or create a role automatically.

Custom service role ARN

Figure 14.28 – Amazon SNS topic selection for the alert

# Note

As an alternative, you can also directly create a suitable role at this stage by selecting Create a role in the Service role dropdown. Amazon Lookout for Metrics will ask you to specify which Amazon SNS topic you would like to give some permissions for and a role will be created on your behalf.

+   7. Scroll down to the bottom of this screen and click on Add alert to create this alert.

Your alert is created and you can see it in the alerts list of your detector.

![](bbox=[0.115, 0.605, 0.85, 0.795])

Figure 14.29 – Amazon Lookout for Metrics alerts associated with a given detector

Congratulations! You have successfully trained a detector and configured an alert to warn you whenever it detects an anomaly. Time to close up this chapter!