You will need to enter the following mandatory details to define your alerts:

+   - **Alert name:** Let's call our alert `ecommerce-revenue-alert`.
+   - **Severity threshold:** Leave the default value of 70%. The severity level is a measure of the criticality of a detected anomaly. In *Chapter 15, Viewing Anomalies and Providing Feedback*, you will have the opportunity to learn more about severity levels.
+   - **Channel:** You will choose Amazon SNS as the delivery channel.

3. In the **SNS topic** section, click on **Create a topic** to bring a new window into your browser. This will open the Amazon SNS service home page. Click on **Topics** in the left menu bar and then on the **Create topic** button in the top right. Select the **Standard** type and enter a name for your topic (for instance, `revenue-anomalies-topic`).

![](bbox=[0.12, 0.38, 0.87, 0.72])