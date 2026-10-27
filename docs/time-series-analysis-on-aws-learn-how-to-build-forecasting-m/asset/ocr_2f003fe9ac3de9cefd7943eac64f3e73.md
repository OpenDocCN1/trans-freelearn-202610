# Using the off-time conditions

When training a model, Amazon Lookout for Equipment allows you to select a signal that will be used to identify when a process or a piece of equipment is off, as shown in the following screenshot:

![](bbox=[0.13, 0.19, 0.86, 0.62])

Figure 12.2 – Off-time detection feature

When you configure this parameter, Lookout for Equipment will remove the signals from these periods as they are not relevant to find any anomalies. If you leave this data while training, the periods where your asset or process is down will likely be marked as anomalies and you will end up with many false positives.