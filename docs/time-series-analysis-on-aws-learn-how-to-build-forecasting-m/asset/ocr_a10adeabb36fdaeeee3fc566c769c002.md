3. In the **Details** section of the **Create subscription** screen, enter **Email** for **Protocol** and your email address for **Endpoint**.

![](bbox=[0.117, 0.151, 0.851, 0.483])

Figure 12.37 – Creating a subscription to an existing SNS topic

4. Scroll down and click on the **Create subscription** button. You will receive an email after a few seconds with the subject titled *AWS Notification – Subscription Confirmation*. Click on the **Confirm subscription** link that you will find in this message: your email address is now subscribed to this SNS topic. This means that any message sent to this topic will be delivered to your email address.

Now that your notification delivery is configured, you will go back into Amazon EventBridge to connect your workflow status change to this topic. To do this, follow these steps:

+   1. Use the **Services** drop-down menu at the top left of the screen to bring up the list of AWS services. In the **Application Integration** section, look for the **Amazon EventBridge** service and click on its name to go to its console. From the service home page, click on the **Create rule** button.
+   
+   2. Give a **Name** to your rule: `LookoutEquipmentPipelineStatusChange`.