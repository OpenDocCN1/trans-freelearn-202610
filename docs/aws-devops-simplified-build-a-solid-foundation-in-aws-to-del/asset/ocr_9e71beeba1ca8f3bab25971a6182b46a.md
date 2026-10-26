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