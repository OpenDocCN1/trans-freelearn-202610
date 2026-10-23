+   VI.	In the bottom pane, click **Inbound rules**.
+   VII.	Click **Edit inbound rules**.
+   VIII.	On the **Edit inbound rules** page, click **Add rule**.
+   IX.	Select **MySQL/Aurora** from the *Type* drop-down list.
+   X.	Ensure that **Source** is set to **Custom** and in the search box, select `Data-Security-Group` as per the following screenshot.

![](bbox=[0.14, 0.25, 0.82, 0.44])

+   XI.	Click **Save rules**.

Now that you have configured the required security groups, you can proceed with the next project task, which involves creating a database subnet group and deploying a MySQL RDS database instance.

# # Project Task 10.3 – Creating a Database Subnet Group and Launching an RDS Database

In this project task, you will deploy a MySQL RDS database instance to store your to-do list items. First, you must configure a database subnet group. This is a group that identifies the subnets that will be used for your RDS database instance.

+   1. Ensure you are still logged into the AWS development account as the IAM user “Alice” and that you are in the London region.
+   2. From the top search bar, search for and navigate to the Amazon RDS management console.
+   3. From the left-hand menu, click **Subnet groups**.
+   4. In the right-hand pane, click the **Create DB subnet group** button.
+   5. Provide a name for your subnet group, such as `Todo-RDS-Subnet-Group`.
+   6. Provide an appropriate description.
+   7. Select the VPC you configured earlier in *Project Task 10.1* from the **Choose a VPC** drop-down list.
+   8. In the **Add subnets** section, select the `eu-west-2a` and `eu-west-2b` AZs from the **Availability Zones** drop-down list.