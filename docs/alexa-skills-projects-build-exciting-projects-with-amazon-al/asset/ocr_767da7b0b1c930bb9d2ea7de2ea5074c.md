We can also add persistence to any Alexa Skill for saving the current state or the user's progress. Saving state can be used as a delighter or it can also be a necessity, especially in **Game/Entertainment**-oriented skills where it is critical to save the user's progress.

In this section, we shall demonstrate adding persistence to Factly; we shall be informing the user of the number of times they have played Factly up until now, and for this we will be storing the launch count using **DynamoDB**, which is a NoSQL database service provided by AWS.

![](bbox=[0.12, 0.28, 0.2, 0.35])

To read more about DynamoDB, please navigate to https://aws.amazon.com/dynamodb/.

We will be accessing DynamoDB via our Lambda code to persist and retrieve the launch counter. However, our Lambda, in its current state, cannot connect to DynamoDB. You may now ask: why so? The reason is that our Lambda, in its current state, is executing in **lambda_basic_execution** mode, which we chose while creating it, and this mode only allows the Lambda to execute some basic code and log some data. To connect to DynamoDB, we will need to elevate **lambda_basic_execution** mode so that it can access DynamoDB and we will do this via **IAM**, which is an Amazon Web Service for **Identity and Access Management**.

![](bbox=[0.12, 0.52, 0.2, 0.59])

To know more about IAM, please visit https://aws.amazon.com/iam/.

Let's get started:

+   1. Navigate to https://console.aws.amazon.com. From the list of provided services, select **IAM** and, if you cannot find it, then just search for it in the search box provided, as shown in the following screenshot: