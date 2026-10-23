+   14. Under the **Permissions** tab, click the **Add permissions** drop-down arrow and select **Attach policies**.
+   
+   15. You will be presented with **Attach policy to todo-ec2-iam-master-role**. (Note, if you had named your IAM role with a different name, then you will see that name instead.)
+   
+   16. From the search box under **Other permissions policies**, search for and check the box next to the two policies you created – one for accessing the secret in AWS Secrets Manager and the other for the Amazon S3 access policy you created in *Step 1* of this project task, as per the following screenshot:

![](bbox=[0.131, 0.261, 0.852, 0.474])

+   17. Click the **Add permissions** button. You should get a notification stating that the policies have been successfully attached to the role.

This configured your IAM role with the necessary policies to manage your application remotely using Session Manager, enabling your EC2 instances to download the application source code and allowing the application to fetch the credentials for your database from AWS Secrets Manager. Later, you will attach this IAM role to all EC2 instances deployed with the application in upcoming project tasks.

# Project Task 7.3 – Create Key Pairs for Your EC2 Instances

In this project task, you will create *key pairs* for your EC2 instances. While we will be using AWS Session Manager to administer the EC2 instances remotely, having a set of key pairs for remote access is always good practice. For example, in a corporate environment protected with a VPN, if you need to connect to an EC2 instance via a third-party tool such as Ansible, then having these key pairs would be handy:

+   1. From the **AWS Management Console** page, search for `key pairs` in the search box located at the top of the page. Then select the **Key pairs** feature.
+   
+   2. From the right-hand pane, click on **Create key pair**.
+   
+   3. Provide a name for the key pair, for example, `todo-list-ec2-keys`.