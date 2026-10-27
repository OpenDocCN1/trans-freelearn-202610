
You already have an Amazon S3 bucket that you have been using to store your training and inference data. We are going to create a trail in AWS CloudTrail that will log every event happening in this bucket. To do this, follow these steps:

- 1. Use the **Services** drop-down menu at the top left of the screen to bring up the list of AWS services. In the **Management & Governance** section, look for the **CloudTrail** service and click on its name to go to its console. From the service dashboard, click on the **Create trail** button.
- 2. For **Trail name**, enter `LookoutEquipmentAutomationS3Event`.
- 3. Disable the **Log file SSE-KMS encryption** checkbox if you do not want to encrypt the log files of the trail (if you do, you will have to provide an encryption key). Scroll down to click on the **Next** button.
- 4. On the second step of the trail creation, uncheck the **Management events** checkbox and check the **Data events** checkbox.

![](img/e4ee48104c730845e796d8c469dfeecc_361_0.png)

Figure 12.31 – Events type selection for new trail creation

- 2. Give a name to your rule: `LookoutEquipmentPipelineTrigger`.

- 3. In the **Define pattern** section, select the **Event pattern** radio button and then **Pre-defined pattern by service**. Select **AWS** for the **Service provider** field, **Simple Storage Service (S3)** for **Service name**, and **Object Level Operations** for **Event type**:

![](img/e4ee48104c730845e796d8c469dfeecc_363_0.png)

Figure 12.33 – EventBridge pattern definition

- 4. Select the **Specific operation(s)** radio button and select the **PutObject** operation from the drop-down list. Then, select **Specific bucket(s) by name** and fill in the name of your bucket (`timeseries-on-aws-lookout-equipment-michaelhoarau` in my case).

![](img/e4ee48104c730845e796d8c469dfeecc_364_0.png)

Figure 12.34 – EventBridge operations to be captured

- 5. Scroll down to the **Select targets** section of the screen. Select **Step Functions state machine** for **Target** and choose `LookoutEquipmentAutomationPipeline` in the **State machine** dropdown. Leave all the other parameters untouched (including the new role creation).

Select targets

Select target(s) to invoke when an event matches your event pattern or when schedule is triggered (limit of 5 targets per rule).

Target

Select target(s) to invoke when an event matches your event pattern or when schedule is triggered (limit of 5 targets per rule).

Step Functions state machine

State machine

LookoutEquipmentAutomationPipeline

Configure input

Create a new role for this specific resource

Amazon_EventBridge_Invoke_Step_Functions_1104444295

Use existing role

Learn more about EventBridge Identity-based policies.

Retry policy and dead-letter queue

Add target

Figure 12.35 – EventBridge target selection

- 6. Scroll to the bottom of the screen and click on the Create button.

Your trigger is now ready. The last step is to set up a notification once the step function is done.

## Configuring the notification

We will now build the last part of our architecture, the notification service configuration. To achieve this final task, you will once again use Amazon EventBridge and use it to send a message to an Amazon Simple Notification Service (SNS) queue each time your workflow status changes. To do this, you can take these actions:

- 1. Use the Services drop-down menu at the top left of the screen to bring up the list of AWS services. In the Application Integration section, look for Simple Notification Service and click on it to go to its console. From the service home page, set Topic name to LookoutEquipmentWorkflowStatusUpdate and click on the Next step button.

![](img/e4ee48104c730845e796d8c469dfeecc_366_0.png)

Figure 12.36 – Creating a new SNS topic

- 2. On the next screen, leave all the fields as their default values, scroll down to the bottom of the page, and click on the Create topic button. Your topic will be created and a summary dashboard will be displayed. In the Subscriptions section, click on Create subscription.

- 3. In the **Details** section of the **Create subscription** screen, enter **Email** for **Protocol** and your email address for **Endpoint**.

![](img/e4ee48104c730845e796d8c469dfeecc_367_0.png)

Figure 12.37 – Creating a subscription to an existing SNS topic

- 4. Scroll down and click on the **Create subscription** button. You will receive an email after a few seconds with the subject titled *AWS Notification – Subscription Confirmation*. Click on the **Confirm subscription** link that you will find in this message: your email address is now subscribed to this SNS topic. This means that any message sent to this topic will be delivered to your email address.

Now that your notification delivery is configured, you will go back into Amazon EventBridge to connect your workflow status change to this topic. To do this, follow these steps:

- 1. Use the **Services** drop-down menu at the top left of the screen to bring up the list of AWS services. In the **Application Integration** section, look for the **Amazon EventBridge** service and click on its name to go to its console. From the service home page, click on the **Create rule** button.
- 2. Give a **Name** to your rule: `LookoutEquipmentPipelineStatusChange`.

- 3. In the **Define pattern** section, select the **Event pattern** radio button and then **Pre-defined pattern by service**. Select **AWS** for **Service provider**, **Step Functions** for **Service name**, and **Step Functions Execution Status Change** for **Event type**. Then, select **Specific state machine ARN(s)** and enter the ARN of the workflow you created earlier. This ARN will have the following format: `arn:aws:states:eu-west-1:<<ACCOUNT_ID>>:stateMachine:LookoutEquipmentAutomationPipeline`, where `<<ACCOUNT_ID>>` is your AWS account ID.

- 4. Scroll down to the **Select targets** section of the screen. Select **SNS Topic** for **Target** and choose `LookoutEquipmentWorkflowStatusUpdate` in the **Topic** dropdown. Leave all the other parameters untouched (including the new role creation).

![](img/e4ee48104c730845e796d8c469dfeecc_368_0.png)

*Figure 12.38 – Event target definition*

- 5. Click on the **Create** button at the bottom of the page to create your event.

Your notifications are now ready to be triggered. Let's test your pipeline!

## Testing the pipeline

Your automation pipeline is ready – let's test it! Open your favorite text editor and write the following in it:

```json
{
    "train_prefix": "train-data/",
```

- 6. Check your email. You should have received two new messages, one when your workflow started running, and the other when the workflow was done.

> Note
> We did not customize the content of the notification email: they are rather nondescript for a non-developer as you receive the actual JSON payload as a raw notification. In an actual production system for business users, you might want to customize these emails to make them more user friendly. You would achieve this by expanding the Configure input section when you define the target in Amazon EventBridge. There, you can select Input transformer to transform this JSON raw data into an actual English sentence.

Congratulations for reaching the end of this section! You now have all the tools to automate a whole anomaly detection pipeline for your industrial time series!

## Summary

In this final chapter dedicated to Amazon Lookout for Equipment, you learned how to build dashboards that will be useful to get detailed visualizations of your model and inference results. You also built your first automation pipeline, to let the AWS orchestration services do all the heavy lifting for you while you focus on the model results.

This chapter was especially important to understand how to improve your anomaly detection models and how to automate most of the tasks to iterate as fast as possible.

In the next chapter, you will switch to another anomaly detection managed service, but dedicated to business metrics instead of industrial time series: Amazon Lookout for Metrics.
