
The steps to configure the target group are as mentioned here:

- 1. From the AWS Management Console, search for EC2 and navigate to the EC2 console. Ensure that you are in the **us-east-1 (N. Virginia)** region.
- 2. From the left-hand menu, select **Target Groups** from the **Load Balancing** section.
- 3. In the right-hand pane, click the **Create target group** button.
- 4. Select **Instances** from the **Choose a target type** section.
- 5. Next, provide a descriptive name in the textbox under *Target group name*, for example, `todo-list-ec2-target-group`.
- 6. Ensure that **Protocol** and **Port** are set to **HTTP** and **80**, respectively, as per the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_295_0.png)

Figure 7.20 – Configuring target groups for your load balancer

- 7. Set **IP address type** to **IPv4**.
- 8. Select **todolist-vpc**, which you previously created, from the VPC drop-down box.
- 9. Ensure that **HTTP1** is selected from the *Protocol* version.
- 10. Leave all remaining settings at their default values and click **Next** at the bottom of the page.
- 11. You will note that there are no available instances to add to this target group. That is fine because you will configure your Auto Scaling group to automatically associate newly launched instances with this target group later. For now, click on the **Create target group** button.
- 12. You should get a notification indicating that the target group has been created successfully.

Next, you will create the launch template for your Auto Scaling service. The **launch template** allows you to specify configuration details such as the AMI ID, the instance type and family, the key pairs you will use to connect to the instances, and any security groups that need to be associated with the instances. AWS highly recommends you use the **launch template** to define these configuration items, instead of the previous **launch configuration**. This is because you get additional features with the template and can also have multiple versions of a launch template. Hosting multiple versions lets you quickly provision your fleet for different use cases; for example, you might use one set of AMIs for development and another for production.

Let us proceed with setting up a launch template.

## Project Task 7.5 – Create a Launch Template

In this step, you will create a launch template. As part of the configuration, you will reference the **Amazon Machine Image (AMI)** that you created in *Chapter 5, Understanding AWS Compute Services with Amazon EC2 and Storage Options*. This AMI has certain configurations already embedded into the image, such as the installation of Node.js and PM2. While, for the purposes of this project task, the time taken to install these components is minimal, you can imagine how having preconfigured AMIs would significantly speed up the deployment process for your EC2 instances:

- 1. Ensure that you are in the EC2 management console and still in the **us-east-1 (N. Virginia)** region.
- 2. From the left-hand menu, select **Launch Templates** in the **Instances** section of the menu.
- 3. In the right-hand pane, click the **Create launch template** button.
- 4. In the Launch Template wizard that appears, provide a suitable name for your template, for example, `todo-list-launch-template`.
- 5. Next, provide a description for your launch template.
- 6. Under **Application and OS Images (Amazon Machine Image)**, select the **My AMIs** tab, and from the **Amazon Machine Image (AMI)** drop-down list, select the AMI you created in *Chapter 5, Understanding AWS Compute Services with Amazon EC2 and Storage Options*, as per the following screenshot:

### Application and OS Images (Amazon Machine Image) Info

An AMI is a template that contains the software configuration (operating system, application server, and applications) required to launch your instance. Search or Browse for AMIs if you don't see what you are looking for below

Search our full catalog including 1000s of application and OS images

- Recents
- My AMIs
- Quick Start

Don't include in launch template

Owned by me

Shared with me

Browse more AMIs
Including AMIs from AWS, Marketplace and the Community

Amazon Machine Image (AMI)

todolist-worker-ami
ami-01a82b0668b883e1
2024-03-21T06:35:32.000Z Virtualization: hvm ENA enabled: true Root device type: ebs

Figure 7.21 – Launch template configuration: selecting your corporate AMI

- 7. Next, under the Instance type section, select t2.micro, which is eligible for the Free Tier.
- 8. Select the key pair you created earlier in Step 3 from the Key pair name drop-down list.
- 9. Next, in the Network settings section, ensure that Don't include in launch template is selected in the Subnet drop-down list.

10. Next, in the Firewall (security groups) subsection, ensure that Select existing security group is selected, and then from the drop-down list under Security groups, select the app security group you created in Chapter 4, Building Private Networks in the Cloud with Amazon VPC, as per the following screenshot. Recall that the app security group is the one that you will associate with your EC2 instances as it will allow inbound traffic from the load balancers to the application running on the instances.

![](img/34ff9b5b5a734dae3450ab701e223c73_298_0.png)

Figure 7.22 – Configuring network settings for your launch template

11. Next, scroll down and expand the Advanced detail section.

12. Select the drop-down arrow for IAM instance profile and select the IAM role you created in Chapter 5, Understanding AWS Compute Services with Amazon EC2 and Storage Options. Recall this role was named todo-ec2-iam-master-role and is associated with three policies, which are as follows:

- I. AmazonSSMManagedInstanceCore - This is an AWS-managed policy
- II. todo-list-s3-sourcecode-access-policy - This is a customer-managed policy
- III. todo-list-secret-access-policy - This is a customer-managed policy

13. Next, scroll down to the User data – optional section. In the User data textbox, you must paste a Bash script (user-data.sh) provided to you in our GitHub repository. See the following screenshot for a sample:

```
1  #!/bin/bash
2
3  AWS_REGION=us-east-1
4  SECRET_NAME=[Secret Name]
5  DB_HOST=[Database Endpoint]
6  DB_PORT=3306
7
8  yum update -y
9  aws s3 cp s3://[S3 Bucket Name]/todo-plus-nodejs.zip /var
10 unzip /var/todo-plus-nodejs.zip -d /var
11 cd /var/todo-plus-nodejs
12 npm install
13 npm run build
14
15 echo "export AWS_REGION=$AWS_REGION" >> ~/.bashrc
16 echo "export SECRET_NAME=$SECRET_NAME" >> ~/.bashrc
17 echo "export DB_HOST=$DB_HOST" >> ~/.bashrc
18 echo "export DB_PORT=$DB_PORT" >> ~/.bashrc
19 source ~/.bashrc
20 pm2 start dist/main.js+
```

Figure 7.23 – Sample user data script

14. Ensure that you populate the secret name that you created in Chapter 6, Managing Multiple Datasets with AWS Relational and Non-Relational (NoSQL) Databases.

15. Populate the DB_HOST placeholder value with the endpoint address of your RDS database. You will find this in the RDS dashboard, under Databases. Click on the database you deployed in Chapter 6, Managing Multiple Datasets with AWS Relational and Non-Relational (NoSQL) Databases, to view its Connectivity and security settings. You will find the Endpoint address provided, which you can copy and paste into the code replacing the placeholder value.

16. Finally, replace the placeholder for the Amazon S3 bucket name that you created earlier in Project Task 7.1.

17. Next, click the Create launch template button.

18. You should receive a notification stating that the launch template has been successfully created.

Now that you have created your launch template, it’s time to set up the Auto Scaling group. The Auto Scaling group uses the details provided in the launch template and allows you to define your group’s minimum and maximum sizes. You can also specify the desired capacity that the Auto Scaling service aims to fulfill. The Auto Scaling service will also be configured to perform health checks using the target group configuration. In a fixed number of instances configuration, if an instance fails a health check, the Auto Scaling service will terminate the failed instance and replace it with a new one.

As discussed earlier in this chapter, you can also define scaling policies such as the target tracking scaling policy or step scaling. For the purposes of this project, however, we will configure the Auto Scaling service to maintain a fixed number of instances in the fleet.

## Project Task 7.6 – Create an Auto Scaling Group

In this task, you will proceed to configure your Auto Scaling group:

- 1. Ensure you are still in the EC2 management console and the **us-east-1 (N. Virginia)** region.
- 2. From the left-hand menu, select **Auto Scaling Groups** from the **Auto Scaling** section.
- 3. Next, in the right-hand pane, click the **Create Auto Scaling group** button.
- 4. Provide a descriptive name for your Auto Scaling group, for example, `todo-list-auto-scaling-group`.
- 5. Select the launch template you created in the previous step from the drop-down box, as per the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_300_0.png)

Figure 7.24— Configuring the Auto Scaling group and selecting the launch template for the group
