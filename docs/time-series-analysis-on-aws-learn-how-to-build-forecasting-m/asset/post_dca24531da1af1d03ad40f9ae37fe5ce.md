
## Updating the access role

You will need to update the IAM role you created in *Chapter 9, Creating a Dataset and Ingesting Your Data*, and you will then need to create your Lambda function. To do all of this, follow these steps:

- 1. If you went through the *Technical requirements* prerequisites at the beginning of this chapter, you should already be logged in to the AWS console, otherwise, fire up your favorite browser and log in to the AWS console.
- 2. At the top left of your console, you will see a **Services** drop-down menu that will display all the available AWS services. In the **Security, Identity & Compliance** section, look for the **IAM** service and click on its name to go to the IAM console. On the left menu bar, click on the **Roles** menu and then search for your `LookoutEquipmentIndustrialPumpDataAccess` role in the list on the right.

![](img/e4ee48104c730845e796d8c469dfeecc_349_0.png)

Figure 12.17 – IAM role list

3. Click on the name of your role and then click on the Trust relationships tab. Then, click on the Edit trust relationship button.

![](img/e4ee48104c730845e796d8c469dfeecc_350_0.png)

Figure 12.18 – IAM role trust relationships list

4. Update the policy document from your role with the following code. Notice the additional service (lambda.amazonaws.com) that must be allowed to assume this role and access your data in Amazon S3:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "lookoutequipment.amazonaws.com",
                    "lambda.amazonaws.com"
```

2. Next, you will author a new function from scratch (called create-lookout-equipment-schema) and you will fill in the following fields on the function creation page:

- - **Function name**: Let's call this function create-lookout-equipment-schema.
- - **Runtime**: You will use **Python 3.9**.
- - Select **Use an existing role** for **Execution role**.
- - In the **Existing role** dropdown, select the LookoutEquipmentIndustrialPumpDataAccess role you updated earlier.

At the bottom right of this screen, you will find a **Create function** button; click on it to go to the next page.

3. On the next page, at the top right of the **Code source** section, there is an **Upload from** drop-down list. Click on it and select **Amazon S3 location.**

![](img/e4ee48104c730845e796d8c469dfeecc_352_0.png)

Figure 12.20 – Uploading a Lambda function from Amazon S3

4. In the window appearing there, enter the following URL pointing to the code this function will run and click on the **Save** button:

```
s3://packt-publishing-timeseries-on-aws-michaelhoarau/part2-amazon-lookout-for-equipment/create-lookout-equipment-schema.zip
```

> **Important Note**
> Make sure to be in the same region as your initial S3 bucket when creating your Lambda function with this zip package. If not, you will see a region-related error.

5. You are then brought back to the **Code** tab from your Lambda function, where your source code should be updated. Click on the **Configuration** tab and then on the **Edit** button at the top right.

![](img/e4ee48104c730845e796d8c469dfeecc_353_0.png)

Figure 12.21 – Lambda function configuration

![](img/e4ee48104c730845e796d8c469dfeecc_355_0.png)

Figure 12.23 – Lambda function overview

We have now configured a Lambda function and can now configure the actual workflow.

## Building the workflow

We will build the actual workflow, using the AWS Step Functions service. To do so, let's follow these steps:

- 1. Use the Services drop-down menu at the top left of the screen to bring up the list of AWS services. In the Application Integration section, look for the Step Functions service and click on its name to go to the AWS Step Functions console. Click on the hamburger menu (the three horizontal lines) at the top left of the window and then click on the State machines tab and finally, on the Create state machine button.

![](img/e4ee48104c730845e796d8c469dfeecc_355_1.png)

Figure 12.24 – State machines list

2. On the **Define state machine** screen, select **Write your workflow in code** at the top and select the **Standard** type.

![](img/e4ee48104c730845e796d8c469dfeecc_356_0.png)

Figure 12.25 – State machine definition

3. Download the following file:
https://packt-publishing-timeseries-on-aws-michaelhoarau.s3.eu-west-1.amazonaws.com/part2-amazon-lookout-for-equipment/lookout-equipment-automation-step-function.json

4. Open the preceding text file and search for the following string: <<LAMBDA ARN>>. Replace this string with the ARN of the Lambda function you copied previously (at the end of the Lambda function definition).

5. Copy the whole content of your text file (with the updated Lambda function ARN) into the **Definition** area of your state function. The workflow on the right is updated with the actual flow of the state machine.

![](img/e4ee48104c730845e796d8c469dfeecc_357_0.png)

Figure 12.26 – State machine definition

- 6. Click on the **Next** button located at the bottom right. This will bring up a second screen where you will be able to specify other details for your workflow.
- 7. On the next screen, you can specify a name for your state machine. I called mine `LookoutEquipmentAutomationPipeline`. Leave all the other fields as their default values and click on the **Create state machine** button located at the bottom of your screen.

8. This will bring you to a screen with the details of your newly created state machine. Click on the **Edit role in IAM** button at the top to update the role used by the state machine so that it can call the Amazon Lookout for Equipment API function.

![](img/e4ee48104c730845e796d8c469dfeecc_358_0.png)

Figure 12.27 – State machine details

9. The default role created by the AWS Step Functions service will have the ability to call the Lambda function but is not authorized to call the Amazon Lookout for Equipment API (as a reminder, for security reasons, AWS services only have permissions that you explicitly give them). On the Roles **Permissions** tab, click on **Attach policies**.

## Summary

Roles > StepFunctions-LookoutEquipmentAutomationPipeline-role-88662bce

Role ARN arn:aws:iam::...role/service-role/StepFunctions-LookoutEquipmentAutomationPipeline-role-88662bce

Role description Edit

Instance Profile ARNs

Path /service-role/

Creation time 2021-10-12 09:12 UTC+0200

Last activity Not accessed in the tracking period

Maximum session duration 1 hour Edit

Permissions Trust relationships Tags Access Advisor Revoke sessions

Permissions policies (2 policies applied)

Attach policies Add inline policy

Policy name Policy type

LambdaInvokeScopedAccessPolicy-8642dfa0-fb5b-4117-9ada-a544de57b53d Managed policy

XRayAccessPolicy-591f0c43-eb51-42cc-acb5-cc7e9fd32e4 Managed policy

Figure 12.28 - State machine default role definition

10. Search for the LookoutEquipment string in the Filter policies field at the top of the screen and check the box next to AmazonLookoutEquipmentFullAccess. Then, click on the Attach policy button located at the bottom of the screen:

Filter policies LookoutEquipment Showing 2 results

Policy name Type Used as

AmazonLookoutEquipmentFullAccess AWS managed Permissions policy (4)

AmazonLookoutEquipmentReadOnlyAccess AWS managed None

Cancel Attach policy

Figure 12.29 - Attaching a new policy to the state machine role

In this section, you performed the following tasks:

- 1. You updated the role you already used to access data in Amazon S3 to allow a Lambda function to assume this role.
- 2. Then, you created a Lambda function that is used to create a valid data schema ready to be used by Amazon Lookout for Equipment to create a new dataset.
- 3. You then proceeded to create a state machine (or workflow) in AWS Step Functions to orchestrate the different steps in your automation pipeline.
- 4. You finished the process by making sure your workflow has all the permissions it needs to call Lambda functions and Amazon Lookout for Equipment API methods.

You will now set up an automatic trigger to react to a new file uploaded to an S3 bucket: this trigger will launch the workflow you just defined.

## Configuring the automatic trigger

We will now build the following part of our automation pipeline:

![](img/e4ee48104c730845e796d8c469dfeecc_360_0.png)

Figure 12.30 – Automation pipeline, focusing on the trigger step
