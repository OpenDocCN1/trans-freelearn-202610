9. In the **Subnets** drop-down list, ensure you select the subnets that you had created for your database. You can cross-reference the subnet IDs with the VPC management console because this console page does not display the names you had tagged your subnets with. However, if you followed the steps correctly, your database subnets should be configured with `10.5.20.0/24` and `10.5.21.0/24` CIDR blocks as per the following screenshot, thus they are easily identifiable.

![](bbox=[0.147, 0.21, 0.872, 0.538])

Figure 10.15: Selecting the database subnets for subnet group creation

+   10. Click the **Create** button at the bottom of the page.
+   11. Your Todo-RDS subnet group should now have been created successfully. Next, you will proceed to deploy your MySQL RDS database.
+   12. From the left-hand menu in the Amazon RDS console, click on **Databases**.
+   13. In the right-hand pane, click the **Create database** button.
+   14. You will be presented with the **Create database** configuration web page. Ensure that you select **Standard create** from the **Choose a database creation method** section.
+   15. For the **Engine** options, select **MySQL**.
+   16. In the **Templates** section, select **Free tier**.
+   17. Scroll further down and in the **Settings** section, set **DB instance identifier** with a name such as `tododb`.
+   18. In the **Credentials Settings** section, ensure that **Master username** is set to `admin`.