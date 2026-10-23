As depicted in the following diagram, your application code will run on EC2 instances that will be deployed in the App Subnets. The application will need details of the database it needs to connect to such as the database endpoint, database name, username, and password. These details can easily be stored in a secret in AWS Secrets Manager, which the application can be designed to dynamically retrieve.

![](bbox=[0.13, 0.2, 0.87, 0.52])

Figure 6.13 – AWS Secrets Manager for the Todo List application

You can configure AWS Secrets Manager at the same time as you launch the RDS database. In our project exercise, however, you will be deploying the RDS database with a standard username and password and then configuring AWS Secrets Manager as a second step. This approach is just so that you can learn how to incorporate AWS Secrets Manager with existing RDS databases that were launched without it.

As seen in the preceding diagram, the key stages are as follows:

+   1. Configure AWS Secrets Manager to manage the RDS database credentials, specifying the Todo List application database as the one to manage. At the same time, set the rotation schedule to 7 days so that AWS Secrets Manager will automatically rotate the credential information of the database every seven days.
+   2. Update/deploy your application code that now uses the AWS Secrets Manager SDKs to dynamically access the database credentials by making the necessary API calls to AWS Secrets Manager.
+   3. Using the credentials supplied by AWS Secrets Manager, the Todo List application can access the backend Amazon RDS database and perform all CRUD operations.