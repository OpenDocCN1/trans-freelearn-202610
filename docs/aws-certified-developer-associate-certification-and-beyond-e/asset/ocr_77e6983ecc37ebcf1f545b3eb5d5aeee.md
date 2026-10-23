# ## Project Task – Deploy a Highly Available and Scalable Backend Application Component for the Todo List Application

So far, you have built a substantial portion of your core infrastructure, as depicted in the following diagram, which will be used to deploy and host the Todo List application.

![](bbox=[0.15, 0.25, 0.85, 0.58])

Specifically, you have deployed the following core infrastructure services:

+   1. The setup and configuration of IAM with specific users with access to the account, defining some IAM roles with permissions to connect with AWS Session Manager to enable remote administration of the application server that we will deploy. This role will be further modified to include additional policies as we progress through the project tasks.

+   2. The setup and configuration of Amazon Cognito user pools, which will be used to create and manage fictitious users of our application.

+   3. The Amazon S3 static website currently hosts the application’s frontend **user interface (UI)**. Additional tasks will be performed on Amazon S3, for example, to host the web page that will be visible to logged-in users and host the application source code as a repository for deployment.