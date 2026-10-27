+   3. In the **Define pattern** section, select the **Event pattern** radio button and then **Pre-defined pattern by service**. Select **AWS** for **Service provider**, **Step Functions** for **Service name**, and **Step Functions Execution Status Change** for **Event type**. Then, select **Specific state machine ARN(s)** and enter the ARN of the workflow you created earlier. This ARN will have the following format: `arn:aws:states:eu-west-1:<<ACCOUNT_ID>>:stateMachine:LookoutEquipmentAutomationPipeline`, where `<<ACCOUNT_ID>>` is your AWS account ID.

+   4. Scroll down to the **Select targets** section of the screen. Select **SNS Topic** for **Target** and choose `LookoutEquipmentWorkflowStatusUpdate` in the **Topic** dropdown. Leave all the other parameters untouched (including the new role creation).

![](bbox=[0.144, 0.361, 0.865, 0.661])

*Figure 12.38 – Event target definition*

+   5. Click on the **Create** button at the bottom of the page to create your event.

Your notifications are now ready to be triggered. Let's test your pipeline!

# Testing the pipeline

Your automation pipeline is ready – let's test it! Open your favorite text editor and write the following in it:

```
```json
{
    "train_prefix": "train-data/",
```
```