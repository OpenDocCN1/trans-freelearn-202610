In addition, if you wish to capture custom metrics at the application level, you can send them to CloudWatch using either the CLI or the SDKs for the application, for example, using the `PutMetricData` API call. Customer metrics come in two different resolutions, as follows:

+   - **Standard resolution:** Data is ingested with one-minute granularity
+   - **High resolution:** Data is ingested with a granularity of one second

High-resolution metrics offer prompt insights into sub-minute activities that can trigger alarms rapidly, based on 10-second or 30-second intervals. You will learn about the alarms in the next section.

# Overview of CloudWatch Alarms

As you capture metrics, you can use these to trigger alarms when a particular metric crosses a threshold value you define for a specified period. This idea that alarms are triggered when breaching a threshold value for a defined period is important. You do not want to trigger alarms simply because of an odd spike.

There are two types of alarms, as follows:

+   - **Metric alarm:** Associated with a single CloudWatch metric or the result of a math expression based on CloudWatch metrics.
+   - **Composite alarm:** Uses a rule expression and comprises multiple alarms. Using composite alarms helps to reduce overall noise. This is because you can configure a composite alarm to go into the **ALARM** state only if all of the underlying metric alarms are also in an **ALARM** state.

An alarm can be in any of the following three states:

+   - **OK:** This is when the metric is within the threshold values
+   - **ALARM:** This is when the metric is outside of threshold values
+   - **INSUFFICIENT_DATA:** This is when there is not enough data, usually when you just configured an alarm and need to wait for metric data points to be collected

Once an alarm has entered the **ALARM** state, you can automate various actions in response to that alarm. You can send an alert to an Amazon SNS topic, which can then send email alerts to a developer. You can also trigger an auto-scaling group, which may launch or terminate instances based on the triggered alarm, enabling you to scale out or scale in automatically.