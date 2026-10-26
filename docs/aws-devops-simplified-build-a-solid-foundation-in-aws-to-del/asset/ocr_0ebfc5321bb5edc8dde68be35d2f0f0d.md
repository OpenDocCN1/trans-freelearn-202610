# Note

Please note that all the hands-on exercises in this book have been tested in the AWS Frankfurt (eu-central-1) region, using a t3.medium instance type of the Cloud9 environment. It's strongly advised that you use the same region in your AWS console. Furthermore, all activities from this point onwards will incur costs in your AWS account. A 5hrs/day usage of the t3.medium compute instance costs roughly 7.30 USD per month, for example. You can reduce these costs further by choosing a low-cost instance type, such as t3.small. Furthermore, it is highly recommended that you set up billing alarms in your AWS account. For instructions, please refer to https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html.

You will use a CloudFormation template to perform a one-time installation of the IDE in your AWS account. If you have not used CloudFormation in the past, it is an AWS service that enables you to provision AWS infrastructure deployments predictably and repeatedly. A template is a YAML or JSON formatted file that defines certain AWS resources, as code, and deploys them as one group, known as a stack. We will dive into the details of the service in Chapter 4, Managing Infrastructure as Code with AWS CloudFormation. Let's proceed with installing the Cloud9 IDE:

+   1. First and foremost, download the template from this book's GitHub repository onto your local machine: https://github.com/PacktPublishing/AWS-DevOps-Simplified/blob/main/toolbox/cloud9-ide.yaml.
+   2. In the AWS console, log in with the IAM user we created in the previous section, switch to the eu-central-1 region, and search for the CloudFormation service in the search panel at the top.
+   3. Once you are on the service page, click the Create stack dropdown and select the With new resources (standard) option.
+   4. In the Prepare template section, select the Template is ready option.
+   5. In the subsequent section, Specify template, select Upload a template file and upload the file you downloaded in Step 1. Click Next.
+   6. Enter any stack name that you want. In the Parameters section, you can configure certain properties. However, it is recommended that you use the defaults unless you are sure about the changes you want to make.
+   7. Continue clicking Next until you arrive at the final screen, where you can click on the checkbox that asks for your acknowledgment to create IAM resources. Finally, click Submit – your Cloud9 IDE should be ready in 5-10 minutes.

Once the CloudFormation stack has been created, you can go to the Cloud9 service page to access your newly created environment.