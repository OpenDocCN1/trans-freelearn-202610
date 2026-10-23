+   9. Next, you will need to update your route tables. From the left-hand menu, click on **Route tables**.
+   10. Next, in the right-hand pane, select the main route table, **todolist-mainrt**.
+   11. Next, click the **Routes** tab in the bottom half of the right-hand pane and click **Edit routes**.
+   12. Click **Add route**.
+   13. For **Destination**, type 0 . 0 . 0 . 0 /0, which designates external networks including the internet. For **Target**, select **NAT Gateway** from the drop-down menu and ensure you select the correct NAT gateway you just created from the options listed:

![](bbox=[0.24, 0.28, 0.78, 0.36])

Figure 6.29 – Selecting the correct NAT gateway

+   14. Click **Save changes**.
+   15. An important point to note is that when AWS Secrets Manager creates your Lambda function for the automatic rotation, it will assign it the same security group as the RDS database it is associated with. You had called this security group **Data-Security-Group**. When you created this security group in *Chapter 4, Building Private Networks in the Cloud with Amazon VPC*, it was allowed to only accept traffic on port 3306 from any device associated with the **App-Security-Group**. However, this restricted configuration will not allow the Lambda function to communicate with the RDS database. Thus, you will need to amend the inbound rules for the **Data-Security-Group**.
+   16. From the left-hand menu, click **Security groups**.
+   17. Next, select **Data-Security-Group** in the right-hand pane.
+   18. In the bottom section of the right-hand pane, click the **Inbound rules** tab, and then click **Edit inbound rules**.
+   19. Click the **Add rule** button.