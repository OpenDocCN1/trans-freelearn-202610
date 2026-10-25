# ## 3. Click the alert (bell) icon to configure your alert. For this scenario, let’s assume we want to add the first alert when our KPI is below the 95% mark of the target. Type the following values:

+   - **Alert name**: [Amber] New York Taxi Number of Passengers
+   - **Alert value**: Percent
+   - **Condition**: Is below
+   - **Threshold**: 95
+   - **Notification preference**: As frequently as possible

Alerts for SPICE datasets are evaluated every time the dataset is refreshed. According to the AWS documentation, for direct query datasets, alert rules are evaluated at a random time between 6:00 PM and 8:00 AM in the **AWS Region** that holds the dataset: https://docs.aws.amazon.com/quicksight/latest/user/ threshold-alerts.html

![](bbox=[0.213, 0.431, 0.747, 0.879])

Figure 9.18 – Configuring alerts