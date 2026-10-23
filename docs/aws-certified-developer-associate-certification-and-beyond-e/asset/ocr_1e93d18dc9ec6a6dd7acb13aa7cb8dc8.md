You have now created a secret called `tododbsecret` in AWS Secrets Manager. It takes a few minutes for the Lambda rotation function to get created by AWS CloudFormation. Secrets Manager is now managing your database credentials, rotating them every seven days to ensure that a new set of credentials is created regularly for added security.

In the next section, you will create an IAM role with the necessary policies to enable the Elastic Beanstalk service to interact with your environment, such as EC2 instances, and query Secrets Manager to retrieve the credentials data dynamically and store it in its environmental variables.

# Project Task 10.5 – Configuring an IAM Role for Your Elastic Beanstalk Application

In this project task, you will need to create an IAM role with the following AWS-managed policies:

+   - `AWSElasticBeanstalkWebTier`
+   - `AWSElasticBeanstalkMulticontainerDocker`
+   - `AWSElasticBeanstalkWorkerTier`

In addition, you will also need an inline policy that grants permissions to access the secret in AWS Secrets Manager. The role is then configured to be assumed by the EC2 instances deployed by the Elastic Beanstalk service.

+   1. In the AWS Management Console, search for and navigate to the **AWS Identity and Access Management (IAM)** service.
+   2. From the left-hand menu, click **Roles**.
+   3. In the right-hand pane, click the **Create role** button.
+   4. For **Trusted entity type**, select **AWS service**.
+   5. Next, select **EC2** from the **Service or use case** drop-down list and then select EC2 for **Use case**. Click **Next**.
+   6. On the **Add Permissions** page, search for and select the three AWS Elastic Beanstalk policies mentioned previously and click **Next**.
+   7. Provide a name for the role, such as `Todo-EB-SM-IAM-Role`.
+   8. Scroll to the bottom of the page and click **Create role**.
+   9. Your role will be created. Next, search for the role in the search box from the list of roles available, as per the following screenshot in *Figure 10.21*: