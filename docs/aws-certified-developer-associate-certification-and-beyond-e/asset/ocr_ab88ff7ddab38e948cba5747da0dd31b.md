18. Click the **Create role** button at the bottom of the page.

You should receive a confirmation that the role has been successfully created.

# Project Task 16.2 – Create a VPC Flow Log and Publish to CloudWatch Logs

In this project task, you will create a VPC flow log and configure it to publish it to the CloudWatch Logs group:

+   1. From the top search bar in the AWS Management Console, search for and navigate to the VPC dashboard.
+   2. From the left-hand menu, select **Your VPCs**.
+   3. In the right-hand pane, select the checkbox next to **todolist-vpc** (if you named your VPC with another name, ensure it is the same VPC that you used to deploy the Todo List application).
+   4. From the **Actions** drop-down list, select **Create flow log**.
+   5. Provide a name for your flow log, such as `todolist-vpcflowlog`.
+   6. Next, for **Filter**, select **All**.
+   7. Set **Maximum aggregation interval** to **10 minutes**.
+   8. Next, for **Destination**, select the **Send to CloudWatch Logs** option.
+   9. In **Destination log group**, type in a name for your log group, for example, `todolist-flowlog-group`. A new CloudWatch log group will be created for you.
+   10. For **IAM role**, select the role you created in *Project Task 16.1* from the drop-down list.
+   11. Select **AWS default format** for **Log record format**.
+   12. Select the **Include Amazon ECS metadata** option for **Additional metadata**.
+   13. Click the **Create flow log** button at the bottom of the web page.

You should receive a confirmation that the flow log for your VPC has been successfully created.

# Project Task 16.3 – Viewing Flow Log Data

+   1. From the search bar in the Management Console, search for and navigate to the CloudFront console.
+   2. Select **Distributions** from the left-hand menu.
+   3. In the right-hand pane, click the CloudFront distribution you created earlier.
+   4. From the **General** tab, copy the **Distribution** domain name and paste it into a new browser tab/window.