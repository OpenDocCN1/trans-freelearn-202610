10. Next, in the Firewall (security groups) subsection, ensure that Select existing security group is selected, and then from the drop-down list under Security groups, select the app security group you created in Chapter 4, Building Private Networks in the Cloud with Amazon VPC, as per the following screenshot. Recall that the app security group is the one that you will associate with your EC2 instances as it will allow inbound traffic from the load balancers to the application running on the instances.

![](bbox=[0.14, 0.23, 0.87, 0.55])

Figure 7.22 – Configuring network settings for your launch template

11. Next, scroll down and expand the Advanced detail section.

12. Select the drop-down arrow for IAM instance profile and select the IAM role you created in Chapter 5, Understanding AWS Compute Services with Amazon EC2 and Storage Options. Recall this role was named todo-ec2-iam-master-role and is associated with three policies, which are as follows:

+   I. AmazonSSMManagedInstanceCore - This is an AWS-managed policy
+   II. todo-list-s3-sourcecode-access-policy - This is a customer-managed policy
+   III. todo-list-secret-access-policy - This is a customer-managed policy