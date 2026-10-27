+   - **Amazon Simple Notification Service:** This messaging service can be used to send messages between applications or between applications and persons. You will use this service to send notifications whenever your training and deployment workflow goes through a status change.

Equipped with this overview, you can now head safely into the first step of your automation journey: assembling the model training and deployment workflow.

# Building the anomaly detection workflow

Let's now build the following part of our automation architecture:

![](bbox=[0.145, 0.319, 0.854, 0.645])

Figure 12.16 – Automation pipeline, focusing on the Step Functions

First, we need to create the Lambda function, which will be used to build a data schema to create a dataset in Amazon Lookout for Equipment. As an input, the step function will receive the location of the data and this location will be passed to the AWS Lambda function. This Lambda function will need the same access to the data located in S3 as Amazon Lookout for Equipment. Let's start by updating your access role.