# # Using user groups

As the number of users increases and their access patterns become more and more sophisticated, the complexity of configuring their permissions to resources also rises exponentially. In these circumstances, we can use **user groups**. User groups allow you to group users together and then apply permissions at the group level, rather than the individual level, which saves you time and effort. When a user joins a group, they automatically inherit the group permissions. Likewise, when a user leaves the group, then they lose access to the group’s resources.

To better understand how to manage groups in Amazon QuickSight, we will use a simple hands-on example:

+   1. First, let’s create a group for the marketing department. Using a terminal with AWS CLI configured and with elevated access, type the following command, replacing the `<account-id>` value with the value from your environment:
+   
+   ```
+   $aws quicksight create-group --group-name "Marketing" 
+   --description "Group for the Marketing Department" 
+   --aws-account-id <account-id> --namespace default 
+   --region us-east-1
+   ```

+   2. Next, let’s add our Reader user to this group. For this purpose, we will use the create-group-membership CLI command. Type the following, replacing the highlighted values with those from your environment:
+   
+   ```
+   $aws quicksight create-group-membership --member-name reader --group-name Marketing --aws-account-id <account-id> --namespace default --region us-east-1
+   ```

+   3. Next, we will use the QuickSight Console to share a new dashboard with the marketing group, rather than an individual user. Log into QuickSight as the Author user. Open the New York Taxi analysis and publish it as a brand-new dashboard. Give any name to your newly created dashboard.