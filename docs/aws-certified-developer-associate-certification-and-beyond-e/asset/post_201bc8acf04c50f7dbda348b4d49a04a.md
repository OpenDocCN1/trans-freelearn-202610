
In this section, you have explored the Amazon’s NoSQL database solution and DynamoDB. AWS DynamoDB is a highly scalable, reliable, and cost-effective NoSQL database designed for most modern web and mobile applications. AWS DynamoDB lends itself well to serverless design architectures in that you do not have to manage servers or plan and design which instance type or size to provision for your workloads. You will be revisiting the AWS DynamoDB later in this study guide when you will examine serverless applications in more detail.

AWS DynamoDB can be configured to provide the necessary capacity based on demand, or you can specify the necessary capacity based on the requirements of your application and DynamoDB will manage it for you.

Understanding the core components of DynamoDB, its architectural references, and its features is critical when designing your application architecture. This includes determining which features to use and how to configure them for maximum performance, reliability, and security.

In the next section, you will continue to work on the project exercises to build the infrastructure for the TodoPlus Limited client, which will be used to deploy and manage the Todo List application.

# Project Task – Deploying and Securing Access to an Amazon RDS Database for the Todo List Application

In the following project tasks, you will learn how to deploy a fully functioning Amazon RDS database, which will be used to host end customers’ to-do items for the Todo List application. The database will be deployed using the MySQL engine, and access to it will be permitted only from the EC2 instances running the backend application.

To ensure that your EC2 instances can access the database, you will deploy a secret in AWS Secrets Manager. This secret will securely store the database credentials and make them accessible only to authorized instances running your application.

# Technical Requirements

To complete the tasks in this project series, ensure you are logged into the AWS development account as the IAM User Alice.

# Project Task 6.1 – Deploying an RDS Database on AWS

In this project task, you will deploy an RDS database on AWS in the Development account. As discussed earlier in this chapter, RDS databases need to be deployed in a Virtual Private Cloud (VPC). You should have previously created a VPC called `todolist-vpc` from the exercises in Chapter 4, Building Private Networks in the Cloud with Amazon VPC. You will use this same VPC now to deploy your RDS database. Before beginning the following steps, ensure that you are in the North Virginia region.

## Configuring the RDS Subnet Group

To deploy an RDS database within your VPC, you should first configure a subnet group – this is a group of subnets that are allocated for your RDS database instances. You will need a minimum of two subnets to form the group. In your `todolist-vpc`, you have already defined two subnets that will be used for your database, as depicted in the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_246_0.png)

Figure 6.22 – VPC subnet groups (subnets defined in Chapter 4)

Do the following to launch your RDS database:

1. Log in to your AWS Development account and navigate to the RDS console by searching for RDS in the top search bar in the AWS Management Console.
2. From the left-hand menu, select **Subnet groups**.
3. From the right-hand pane, click on the orange **Create DB subnet group** button.
4. Provide a name for your subnet group such as `todo-list-db-subnet-group`.
5. Ensure you select `todolist-vpc` from the VPC drop-down list.
6. Under **Add subnets**, select **us-east-1a** and **us-east-1b** from the **Availability zones** drop-down menu.
7. Next, select the database subnets under each Availability Zone. As per the following screenshot, you need to select the correct subnet under each Availability Zone based on the IP CIDR block you had previously defined for the subnet, especially as the name of the subnet is not provided.

![](img/34ff9b5b5a734dae3450ab701e223c73_247_0.png)

Figure 6.23 – Selection of subnets for the database security group

8. Click the **Create** button. You should receive a message stating that the database subnet group was successfully created.

## Launch an RDS Free Tier Database

In this step, you will launch an RDS database instance with the MySQL engine using the Free Tier option. The Free Tier option does not allow you to configure a Multi-AZ deployment. However, you will not be needing this for the purposes of this project exercise. In a real-world production environment, you must consider deploying your RDS instances with Multi-AZ:

1. From the left-hand menu, click on **Databases**.
2. From the right-hand pane, click the orange **Create database** button.
3. Select **Standard create** under **Choose a database creation method**.
4. Next, under the **Engine** option, select the **MySQL** engine radio button.
5. Keep the engine version at the default version and from **Templates**, select the **Free tier** radio button.
6. Under **Settings**, provide the following details:
   - I. Set the name for **DB instance identifier** to `tododb`.
   - II. For **Credentials settings**, set the **Master username** to **admin**.
   - III. Under **Credentials management**, select the **Self-managed** option for now. You will configure Secrets Manager in another project task.
   - IV. Provide a *password* as the **Master password**, which you must make a note of. Re-enter your chosen password in the **Confirm master password** text box.
7. Under **Instance configuration**, ensure that you have the `db.t3.micro` instance class selected.
8. Next, under **Storage**:
   - I. Ensure that **General Purpose SSD (gp2)** is selected.
   - II. Set the **Allocated storage** to 20 GiB.
   - III. Expand the **Storage autoscaling** subsection.
9. Untick the Enable storage autoscaling checkbox as you will not need more than 20 GiB for our database.

**Storage**
Storage type Info
Provisioned IOPS SSD (io2) storage volumes are now available.

**General Purpose SSD (gp2)**
Baseline performance determined by volume size

**Allocated storage**
Info
20
GiB

The minimum value is 20 GiB and the maximum value is 6,144 GiB

> After you modify the storage for a DB instance, the status of the DB instance will be in storage-optimization. Your instance will remain available as the storage-optimization operation completes. Learn more

**Storage autoscaling**
Storage autoscaling Info
Provides dynamic scaling support for your database's storage based on your application's needs.

**Enable storage autoscaling**
Enabling this feature will allow the storage to increase after the specified threshold is exceeded.

Figure 6.24 – Storage configuration for your RDS database instance

10. Next, under **Connectivity**:
   - I. Ensure that **Don't connect to an EC2 compute resource** is selected.
   - II. Under **Virtual private cloud (VPC)**, select the `todolist-vpc` from the drop-down list.
   - III. Next, under **DB subnet group**, ensure that the same subnet group you created in step I is selected.
   - IV. Set **Public access** to **No**. This is because the RDS database will be deployed in a private subnet in accordance with best practices.
   - V. Next, for the **VPC security group** (firewall), select **Data-Security-Group** from the **Existing VPC security groups** drop-down list. You can also click the X symbol next to **default** to remove the default security group, which is not required.
   - VI. Under **Availability Zone**, you can choose an Availability Zone or leave it as **No preference**, as per the following screenshot:

**Existing VPC security groups**
Choose one or more options
Data-Security-Group ×

**Availability Zone**
Info
No preference

Figure 6.25 – Connectivity option for your RDS database

11. Under **Database authentication**, ensure that **Password authentication** is selected.
12. Next, expand the **Additional configuration** subsection and:
   - I. Set the Initial database name to `tododb`.
   - II. Leave the **DB parameter** group and **Option** group at their default values.
   - III. Next, under **Backup**, ensure that **Enable automatic backups** is selected and set the **Backup retention period** to 1 day.
   - IV. Leave the **Encryption** and **Maintenance** parameters at their default values.
13. Scroll to the bottom of the page and click the **Create database** button.
