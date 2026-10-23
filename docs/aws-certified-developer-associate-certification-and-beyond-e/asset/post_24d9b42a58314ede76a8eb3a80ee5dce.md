
## Improving Security with AWS Secrets Manager

The RDS database will be deployed with Multi-AZ configured. In addition, the database engine you will be using for this project will be the MySQL engine. Note that the master and standby copies of the database instances will be placed across the two Availability Zones in the private subnets allocated for RDS. This was defined earlier as the database subnet group. Data from the master copy will be synchronously replicated across to the standby copy.

Additionally, the RDS database instances will be associated with the Data security group as depicted in the diagram to allow traffic only from the application servers on port 3306 (the default port on which the MySQL engine listens). This security measure will ensure that no other service can attempt to access the database.

The application will also need a mechanism to authenticate against the database. As previously discussed, there are three different authentication options for Amazon RDS: **Password authentication**, **IAM authentication**, and **Kerberos authentication**.

For this project, you will be using password authentication. This comprises a username and password that is defined when you first create the database in the upcoming project task. The application needs to be configured to use these credentials so that it can access the database and perform all necessary CRUD operations on behalf of the customer.

The issue with this approach, however, is that you need to hardcode the credentials in the application code itself or place them somewhere in plain text so that the application can retrieve and use them to authenticate against the database. This approach comes with lots of security risks as well as an administrative burden, as follows:

-   - The security risk of leaked credentials data if the application code gets accessed.
-   - If your app servers get compromised, the credentials are in plain text and will be easily accessible.
-   - It is always recommended to rotate credential data on a regular basis. This would have to be carried out manually and the application code will need to be updated across all compute instances in the fleet, which adds to the administrative burden.

To prevent any security breaches, Amazon provides a service that can be used to enhance the overall security for accessing your database, which is known as AWS Secrets Manager. Let’s discuss the key features of this service next.

Managing credentials information, API keys, and other kinds of secret data has always been a challenge. AWS Secrets Manager is designed to help you securely manage, retrieve, and rotate your database and application credentials, API keys, and other secret data.

AWS Secrets Manager enables you to dynamically access secrets when needed by making API calls and only if you have been granted the right set of permissions and authorization levels. Secrets Manager can thus be used to improve your application security posture as you no longer need to hardcode credential data in your application code.

The following are the key features of AWS Secrets Manager:

-   - **Secret** – This consists of the secret information, including a value and metadata. Secret values can either be strings or binary. You can also use JSON text strings and key/value pairs as secrets. The secret metadata includes the secret’s name, which needs to be unique, and the **Amazon Resource Name (ARN)**, both of which can be referenced to call the secret. To access the secret, you need IAM permissions, which can be granted via an IAM role attached to an IAM policy.
-   - **Version** – AWS Secrets Manager will hold copies of your secrets if the value changes. A new version is created each time the value is changed or rotated.
-   - **Rotation** – AWS Secrets Manager enables you to automatically rotate your secret data, making it much more secure and reducing administrative burden. You can set up automation rotation at specified intervals or schedules to align with your corporate policies of password/secret rotations. Because AWS Secrets Manager integrates well with various AWS services such as Amazon RDS, automatic secret rotation of credentials is very easy and is fully managed by AWS. If you need to rotate other types of secret data, you can create the necessary Lambda functions to perform the rotations for you.

The following diagram illustrates how Amazon Secrets Manager is used to protect sensitive and secret data:

![](img/34ff9b5b5a734dae3450ab701e223c73_224_0.png)

Figure 6.12 – Using AWS Secrets Manager to manage database credentials data

The key steps involved in configuring AWS Secrets Manager to host and manage sensitive credentials data for your application are as follows:

-   1. The database administrator configures your database with a set of credentials that will be used by the application.
-   2. The database administrator stores the credential information as a secret in Secrets Manager. This data is also encrypted by the Secrets Manager.
-   3. When the application needs to access the database, the application code is designed to first query Secrets Manager via an API call to retrieve the credential data.
-   4. Secrets Manager will retrieve the secret and decrypt it, returning the plaintext version of the secret to the client application over HTTPS.
-   5. The client application will then use the credentials data and any other information, such as database endpoint names, to access the database and the information contained within it.

AWS Secrets Manager can store sensitive credentials in an encrypted format using AWS **Key Management Service (KMS)**. The process involves using KMS CMKs to generate and encrypt a symmetric data key, which is then used to encrypt the secret credentials data such as passwords. The process involves using the plain text data key to encrypt the secret outside of KMS, after which the key is discarded from memory. The encrypted version of the same data key is stored in the secret's metadata so that it is available to decrypt the secret value.

However, this encrypted version of the data key or the encrypted secret is never returned from any API calls made by the application. Instead, when the application needs the credential information, it uses Secrets Manager to make an API call to the AWS KMS decrypt operation and the encrypted data key is passed to KMS. AWS KMS then decrypts the data key and returns the plaintext version of that key to Secrets Manager. AWS Secrets Manager then uses this plaintext data key to decrypt the secret and pass it to the application.

With regards to the TodoPlus Limited and our Todo List application, it is highly recommended that you incorporate AWS Secrets Manager to manage the lifecycle of the database credential information automatically.

As depicted in the following diagram, your application code will run on EC2 instances that will be deployed in the App Subnets. The application will need details of the database it needs to connect to such as the database endpoint, database name, username, and password. These details can easily be stored in a secret in AWS Secrets Manager, which the application can be designed to dynamically retrieve.

![](img/34ff9b5b5a734dae3450ab701e223c73_226_0.png)

Figure 6.13 – AWS Secrets Manager for the Todo List application

You can configure AWS Secrets Manager at the same time as you launch the RDS database. In our project exercise, however, you will be deploying the RDS database with a standard username and password and then configuring AWS Secrets Manager as a second step. This approach is just so that you can learn how to incorporate AWS Secrets Manager with existing RDS databases that were launched without it.

As seen in the preceding diagram, the key stages are as follows:

-   1. Configure AWS Secrets Manager to manage the RDS database credentials, specifying the Todo List application database as the one to manage. At the same time, set the rotation schedule to 7 days so that AWS Secrets Manager will automatically rotate the credential information of the database every seven days.
-   2. Update/deploy your application code that now uses the AWS Secrets Manager SDKs to dynamically access the database credentials by making the necessary API calls to AWS Secrets Manager.
-   3. Using the credentials supplied by AWS Secrets Manager, the Todo List application can access the backend Amazon RDS database and perform all CRUD operations.
