
+   4. **AWS Secrets Manager:** You will configure a secret in AWS Secrets Manager to store the database credentials details that will be made accessible to your application Elastic Beanstalk environment using environmental variables. This is a much more secure approach than hardcoding the credentials in the application.
+   5. **IAM roles and policies:** As usual, you must also configure the IAM roles to enable the Elastic Beanstalk service to receive the secrets and provision the EC2 instances in your VPC. As part of the Elastic Beanstalk configuration, you will provision an autoscaling group and launch configuration – again, necessary IAM roles will be required.
+   6. **Elastic Beanstalk application:** With all the infrastructure components configured, you can build out your Elastic Beanstalk application and environment. Here, you will provide configuration details so that Elastic Beanstalk can provision your autoscaling group and autoscaling policies, provision the load balancer, and deploy your application.

## Technical Requirements

To complete the project tasks in this chapter, you need to do the following:

- You must be logged into your AWS development account as the IAM user “Alice.” Ensure that you are in the London region (eu-west-2).
- You must download the application source code and save it on your desktop for easy access. The source code is available at https://packt.link/pdy6D.

## Project Task 10.1 – Creating a Custom VPC

In this project task, you will build a custom VPC to host your application and configure Elastic Beanstalk to deploy it. While it is possible to get Elastic Beanstalk to build out a VPC for you, remember you want to deploy your RDS database in the VPC first and so require the VPC build-out as a precursor to configuring your Elastic Beanstalk application.

+   1. Log in to the AWS development account as the IAM user “Alice.”
+   2. From the main console web page, search for and navigate to the VPC Management Console using the search bar at the top of the page.
+   3. Next, ensure that you are in the London region by selecting it from the Region drop-down list in the console’s top left-hand corner.
+   4. From the left-hand menu, click Your VPCs and then click the Create VPC button in the top-right corner of the console.
+   5. Select the VPC and more options in the Resources to create section under VPC settings.
+   6. Uncheck the checkbox next to **Auto-generate**.
+   7. For the **IPv4 CIDR block** range, provide the CIDR block of `10.5.0.0/16`.
+   8. Next, ensure that **No IPv6 CIDR block** is selected as per the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_415_0.png)

Figure 10.9: The VPC settings

+   9. Expand **Customize AZs** under the **Number of Availability Zones (AZs)** section.
+   10. Ensure that `eu-west-2a` and `eu-west-2b` are selected as the AZs you will be using for this VPC.
+   11. Ensure that 2 is selected for the **Number of public subnets** option and 4 is selected for the **Number of private subnets** option.
+   12. Expand the **Customize subnets CIDR blocks** subsection and provide the following IP CIDR blocks:

+   I. Public subnet CIDR block in `eu-west-2a` - `10.5.1.0/24`. This will be tagged as `PublicSubnet01`.
+   II. Public subnet CIDR block in `eu-west-2b` - `10.5.2.0/24`. This will be tagged as `PublicSubnet02`.
+   III. Private subnet CIDR block in `eu-west-2a` - `10.5.10.0/24`. This will be tagged as `AppSubnet01`.
+   IV. Private subnet CIDR block in `eu-west-2b` - `10.5.11.0/24`. This will be tagged as `AppSubnet02`.
+   V. Private subnet CIDR block in `eu-west-2a` - `10.5.20.0/24`. This will be tagged as `DataSubnet01`.
+   VI. Private subnet CIDR block in `eu-west-2b` - `10.5.21.0/24`. This will be tagged as `DataSubnet02`.
+   VII. Review the following screenshot to ensure you have correctly configured your subnet CIDR blocks:

# Customize subnets CIDR blocks

Public subnet CIDR block in eu-west-2a

10.5.1.0/24
256 IPs

Public subnet CIDR block in eu-west-2b

10.5.2.0/24
256 IPs

Private subnet CIDR block in eu-west-2a

10.5.10.0/24
256 IPs

Private subnet CIDR block in eu-west-2b

10.5.11.0/24
256 IPs

Private subnet CIDR block in eu-west-2a

10.5.20.0/24
256 IPs

Private subnet CIDR block in eu-west-2b

10.5.21.0/24
256 IPs

Figure 10.10: VPC subnet CIDR blocks

+   13. Scroll further down and ensure that you select 1 in 1 AZ for the NAT gateway ($) configuration. You will only be deploying a single NAT gateway for this project. Please note that NAT gateways are a chargeable service, and you should terminate these resources after completing and testing this project.
+   14. Ensure that None is selected under VPC endpoints.
+   15. Scroll to the top of the page and review the Preview section in the right-hand pane. To ensure you can easily recognize the various subnets, manually edit the individual textboxes to tag them as per the following screenshot.

![](img/34ff9b5b5a734dae3450ab701e223c73_417_0.png)

Figure 10.11: VPC architecture visual preview

The following are the specific names to use for your configuration:

+   I. Provide a name for your VPC in the textbox under the VPC section as depicted in the preceding screenshot. You can use a name such as Todo-List-EB-VPC.
+   II. You will note that the names used for the six subnets in the preceding screenshot are as follows:

+   i. eu-west-2a:

```
PublicSubnet01
AppSubnet01
DataSubnet01
```

+   ii. eu-west-2b:

```
PublicSubnet02
AppSubnet02
DataSubnet02
```

+   3. Ensure you name your route tables per the screenshot to identify them easily. So, in this example, the route tables have been named:
+   1. `PublicRouteTable`: Route table for public subnets
+   2. `AppSubnet01RT`: Route table for `AppSubnet01`
+   3. `AppSubnet02RT`: Route table for `AppSubnet02`
+   4. `DataSubnet01RT`: Route table for `DataSubnet01`
+   5. `DataSubnet02RT`: Route table for `DataSubnet02`
+   4. If you hover your mouse over the individual subnets, you can identify which route tables they connect to.
+   5. Provide a name for the internet gateway, such as `Todo-List-IGW`, and a name for the NAT gateway, such as `Todo-List-NATGW`.

+   16. With all the configuration in place, scroll down to the bottom of the page and click the **Create VPC** button.
+   17. The **Create VPC workflow** screen will appear and configure your VPC with the required components.
+   18. Once the VPC has been created, you can navigate back to the main VPC dashboard and then click on **Your VPCs** to review your newly created VPC.

In the next section, you will configure some security groups in the VPC to ensure traffic can be routed to your RDS database.

## Project Task 10.2 – Creating Security Groups

In this project task, you will create two security groups required for the EC2 instances hosting the application to send traffic to the backend databases, which you will deploy shortly. Amazon Elastic Beanstalk will also provision its own set of security groups: one to allow traffic from the internet to the Application Load Balancers, and another security group will be associated with the EC2 instances it deploys, enabling the Application Load Balancer to distribute inbound HTTP requests to those instances.

As depicted in the following diagram, the resulting configuration will mean that your EC2 instances will have two security groups attached to them, one to inbound traffic on port `80` (HTTP) and one to enable those instances to send database requests to your Amazon RDS database:

![](img/34ff9b5b5a734dae3450ab701e223c73_419_0.png)

Figure 10.12: Security group configuration

Whereas Elastic Beanstalk will create the security groups for the Application Load Balancer and the EC2 instances it deploys, you must also configure two additional security groups – one for the EC2 instances and the other for the RDS database you will deploy shortly.

+   1. Ensure that you are still logged into the AWS development account and are in the London region.
+   2. Navigate to the VPC dashboard if necessary.
+   3. From the left-hand menu, click on Security groups under the Security section.
+   4. In the right-hand pane, click the Create security group button.
+   5. Provide a name for the security group such as App-Security-Group.
+   6. Provide an appropriate description of your choice.
+   7. Ensure you select the correct VPC from the VPC drop-down list, which in this case would be Todo-List-EB-VPC, unless you chose a different name for your VPC.
+   8. Scroll to the bottom of the page and click Add new tag in the Tags – optional section. Create a new tag with Key set to Name and set Value – optional to App-Security-Group. This will ensure that the security group is tagged and easily identifiable.
+   9. Click the Create security group button. The security group will now be created. You will reference this group when configuring the Elastic Beanstalk service and when defining the configuration of your EC2 instances. Note that you did not create inbound rules for this security group. This is perfectly alright because when you configure Elastic Beanstalk, the service will provision another security group for your EC2 instances to accept inbound traffic on port 80 (HTTP) from the Application Load Balancer. Remember, an EC2 instance can be associated with multiple security groups.
+   10. You will now configure a security group for the RDS database. From the left-hand menu, click Security groups again. In the right-hand pane, click the Create security group button.
+   11. Provide a suitable name for your security group, such as Data-Security-Group, and an appropriate description.
