
```
runtime: lambda.Runtime.PYTHON_3_7,
code: lambda.Code.fromAsset('resources/lambda'),
environment: {
  'IMAGE_LABELS_TABLE': dynamodbTable.tableName
},
handler: 'image.lambda_handler'
}
```

The three objects that we initialize in the code will correspond to CloudFormation resources in the stack. We will see this in action soon after we synthesize our CDK stack.

Of course, there are a few other aspects of this solution that are still pending. As you might have noticed, we didn’t assign any permissions to our application orchestrator – the Lambda function. Furthermore, we need to add event notifications for the S3 bucket so that all subsequent image uploads are automatically sent to the Lambda image handler function:

```
// Add appropriate permissions for Lambda to work with S3, Rekognition and DynamoDB
const rekognitionPermissionsForLambda = new iam.PolicyStatement();
rekognitionPermissionsForLambda.addActions("rekognition:*");
rekognitionPermissionsForLambda.addResources("*");

const s3PermissionsForLambda = new iam.PolicyStatement();
s3PermissionsForLambda.addActions("s3:*");
s3PermissionsForLambda.addResources(bucket.bucketArn);
s3PermissionsForLambda.addResources(bucket.bucketArn + "/*");

const dynamodbPermissionsForLambda = new iam.PolicyStatement();
dynamodbPermissionsForLambda.addActions("dynamodb:*");
dynamodbPermissionsForLambda.addResources(dynamodbTable.tableArn);

const kmsPermissionsForLambda = new iam.PolicyStatement();
kmsPermissionsForLambda.addActions("kms:*");
kmsPermissionsForLambda.addResources("*");

const createLogsPermissionsForLambda = new iam.PolicyStatement();
createLogsPermissionsForLambda.addActions("logs:CreateLogGroup");
var logGroupResource = `arn:aws:logs:${process.env.CDK_DEFAULT_REGION}:${process.env.CDK_DEFAULT_ACCOUNT}:*`
createLogsPermissionsForLambda.addResources(logGroupResource);

const putLogEventsPermissionsForLambda = new iam.PolicyStatement();
putLogEventsPermissionsForLambda.addActions("logs:CreateLogStream");
putLogEventsPermissionsForLambda.addActions("logs:PutLogEvents");
```

```
var logGroupArnResource = `arn:aws:logs:${process.env.CDK_DEFAULT_REGION}:${process.env.CDK_DEFAULT_ACCOUNT}:log-group:/aws/lambda/*:*`
putLogEventsPermissionsForLambda.addResources(logGroupResource);
imageHandler.addToRolePolicy(rekognitionPermissionsForLambda);
imageHandler.addToRolePolicy(s3PermissionsForLambda);
imageHandler.addToRolePolicy(dynamodbPermissionsForLambda);
imageHandler.addToRolePolicy(kmsPermissionsForLambda);

// Trigger lambda function when S3 object is added
bucket.addEventNotification(s3.EventType.OBJECT_CREATED, new s3_notifications.LambdaDestination(imageHandler));
```

This completes our construct definitions. Before we move ahead with synthesizing our template, let's add the code for our Lambda function. The imageHandler construct definition has a mandatory argument called code, where we define the location of our Lambda function code. The lambda.Code.fromAsset('resources/lambda') value requires this function to be present in a subfolder called resources/lambda. So, let's go ahead and create it.

## Defining Lambda code for orchestrating the application workflow

> Note
> The steps in this section have already been completed for you, inside the chapter-6/ directory, so you may just skim through them for your understanding.

Let's create a new file called image.py that will host our Python code for the Lambda function:

```
aws-devops-simplified:~/environment/chapter-6 $ mkdir -p resources/lambda
aws-devops-simplified:~/environment/chapter-6 $ cd resources/lambda
aws-devops-simplified:~/environment/chapter-6 $ touch image.py
```

If you remember the workflow diagram that we discussed a while back, the two main responsibilities of the Lambda image handler are as follows:

- Extract the S3 bucket and key information from the event payload and pass it on to the Amazon Rekognition service
- Persist the results of the image labeling process in the DynamoDB table

## Synthesizing the template

The synthesizing process generates a valid CloudFormation YAML template. At this point, you could also deploy it directly within your AWS environment, or instruct CDK to do so. For now, let’s trigger template synthetization with the `cdk synth` command:

```
aws-devops-simplified:~/environment/chapter-6 $ cdk synth
Resources:
  imagelabelsD31B94E7:
    Type: AWS::DynamoDB::Table
    Properties:
      KeySchema:
        - AttributeName: object_key
          KeyType: HASH
      AttributeDefinitions:
        - AttributeName: object_key
          AttributeType: S
      ProvisionedThroughput:
        ReadCapacityUnits: 5
        WriteCapacityUnits: 5
    UpdateReplacePolicy: Retain
    DeletionPolicy: Retain
    Metadata:
      aws:cdk:path: Chapter6Stack/image_labels/Resource
  imagebucket0CAF3977:
    Type: AWS::S3::Bucket
    UpdateReplacePolicy: Delete
    DeletionPolicy: Delete
    Metadata:
      aws:cdk:path: Chapter6Stack/image_bucket/Resource
  imagebucketNotifications2F6EEDF9:
    Type: Custom::S3BucketNotifications
    Properties:
      ServiceToken:
        Fn::GetAtt:
          - BucketNotificationsHandler050a0587b7544547bf3
          - Arn
      BucketName:
        Ref: imagebucket0CAF3977
      NotificationConfiguration:
    ...
    ...
```

After clicking on the stack's name, you can check the resources, as shown in Figure 6.3:

![](img/ecd265dfca9bb64161408ba8a944a4d9_156_0.png)

Figure 6.3 – Stack resources deployed as part of the CDK deployment

Having completed the deployment, let's go ahead and test the end-to-end workflow.

## Testing the image analysis workflow

The test application we've deployed in the AWS account expects an image to be uploaded in the S3 bucket, which then triggers the Lambda function that interacts with the Amazon Rekognition service. The image I used for testing the workflow can be found at https://tinyurl.com/3z9ecjpb. You might want to upload it or use a different one.

Once the upload completes, the S3 event notification will fire off the Lambda function. The image label analysis results will be available in the DynamoDB table, as shown in Figure 6.4:

![](img/ecd265dfca9bb64161408ba8a944a4d9_157_0.png)

The response from Rekognition includes a JSON payload that defines the matched categories and respective confidence scores:

```
```json
[
  {
    "Name": "Cityscape",
    "Confidence": 99.99959564208984,
    "Instances": [
    ],
    "Parents": [
      {
        "Name": "Architecture"
      },
      {
        "Name": "Building"
      },
      {
        "Name": "Urban"
      }
    ]
  }
]
```
```

## Further reading

To learn more about the topics that were covered in this chapter, take a look at the following resources:

- AWS CDK examples in different programming languages: https://github.com/aws-samples/aws-cdk-examples
- The AWS jsii framework, which powers AWS CDK: https://github.com/aws/jsii
- AWS CDK patterns: https://cdkpatterns.com/patterns/

## Part 3

## Security and Observability of Containerized Workloads

In this part, you will learn about the three main pillars of modern software development: containers, observability, and DevSecOps. In addition to building a solid foundational understanding of Amazon ECS, an AWS-managed container platform, the chapters focus on integrations with external, mature open source tools and platforms. By the end, you will have multiple solution patterns deployed in your AWS account, which can be customized to meet your organization’s security and observability needs.

This part has the following chapters:

- Chapter 7, Running Containers in AWS
- Chapter 8, Enabling the Observability of Your Workloads
- Chapter 9, Implementing DevSecOps with AWS
