+   5. Now that you have selected the home Region, feel free to review the console quickly. On the left-hand menu, you will note various tools and services offered by AWS Systems Manager. You will also note the **Session Manager** tool listed under **Node management**. Session Manager is a powerful tool, but for this project, you will focus on using it to access backend EC2 instances for remote administration.
+   
+   6. You must create an IAM role, enabling Session Manager to manage your EC2 instances. From the top search box, search for IAM and open its console in another browser tab.
+   
+   7. In your IAM console, from the left-hand menu, select **Roles**.
+   
+   8. In the right-hand pane, click the **Create role** button and perform the following steps:
+   
+      I. Under **Select trusted entity**, ensure that you select the **AWS service** option.

![](bbox=[0.131, 0.335, 0.855, 0.643])

Figure 5.10 – Setting up an IAM role for Session Manager

+   II. Select **EC2** from the drop-down list under **Service or use case**.
+   
+   III. Under **Choose a use case for the specified service**, select **EC2 Role for AWS Systems Manager** and click **Next**.
+   
+   IV. You will note that the `AmazonSSMManagedInstanceCore` IAM policy has already been added to this role. Click **Next**.
+   
+   V. Provide a role name, such as `todo-ec2-iam-master-role`.