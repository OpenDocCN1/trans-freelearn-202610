
## Using user groups

As the number of users increases and their access patterns become more and more sophisticated, the complexity of configuring their permissions to resources also rises exponentially. In these circumstances, we can use **user groups**. User groups allow you to group users together and then apply permissions at the group level, rather than the individual level, which saves you time and effort. When a user joins a group, they automatically inherit the group permissions. Likewise, when a user leaves the group, then they lose access to the group’s resources.

To better understand how to manage groups in Amazon QuickSight, we will use a simple hands-on example:

1. First, let’s create a group for the marketing department. Using a terminal with AWS CLI configured and with elevated access, type the following command, replacing the `<account-id>` value with the value from your environment:

   ```
   $aws quicksight create-group --group-name "Marketing" 
   --description "Group for the Marketing Department" 
   --aws-account-id <account-id> --namespace default 
   --region us-east-1
   ```

2. Next, let’s add our Reader user to this group. For this purpose, we will use the create-group-membership CLI command. Type the following, replacing the highlighted values with those from your environment:

   ```
   $aws quicksight create-group-membership --member-name reader --group-name Marketing --aws-account-id <account-id> --namespace default --region us-east-1
   ```

3. Next, we will use the QuickSight Console to share a new dashboard with the marketing group, rather than an individual user. Log into QuickSight as the Author user. Open the New York Taxi analysis and publish it as a brand-new dashboard. Give any name to your newly created dashboard.

4. On the next screen, when sharing the dashboard, select the **Marketing** group and share your dashboard, as shown in the following screenshot:

![](img/33bad1d211a2d93fd743a1978d403f9a_196_0.png)

Figure 9.1 – Sharing a dashboard with a group

5. To verify that your dashboard has been shared successfully, log out and log back in as the Reader user. Navigate to **Dashboards**, and notice the newly created dashboard:

![](img/33bad1d211a2d93fd743a1978d403f9a_196_1.png)

Figure 9.2 – Confirming dashboard group access

Note that we didn’t need to give access directly to the Reader user for this dashboard. Instead, we added the Reader user into the **Marketing** group, and then we allowed all users from that group to have access to our published dashboard. In the future, if we had more users joining the **Marketing** group, they would automatically assume access to resources shared with that particular group, eliminating the need to define specific permissions at the individual user level.

Now that we have learned about user groups and how to use them to simplify our permissions, in the next section, we will talk about custom permissions.

## Setting up custom permissions

In _Chapter 2, Introduction to Amazon QuickSight_, we learned the different user cohorts: Admin, Author, and Reader. The user cohort determines the level of access to features in the QuickSight console. Custom profiles allow you to override the default permissions with custom security profiles and define permissions that fit your organization’s requirements.

> Note
> For custom permissions to work, you will need to be using IAM federated users.

To better understand custom permissions, we will use a simple hands-on example. By default, an Author user is able to configure a new data source. For our example, let’s assume that to protect our **Amazon SPICE** space, we want to prevent authors from creating new data sources:

1. First, log into the QuickSight Console as a QuickSight Admin user.
2. Expand the QuickSight menu in the top-right corner of the screen and select **Manage QuickSight**.
3. With the **Manage users** option selected, click on **Manage permissions**, as shown in the following screenshot:

![](img/33bad1d211a2d93fd743a1978d403f9a_197_0.png)

Figure 9.3 – Accessing custom permissions

4. On the next screen, tick the **Creating or updating all data sources** checkbox, as shown in the following screenshot:

# Edit custom permissions

**Name**

custom-author

**Restrict access to**

**Dashboard & analyses**

☐ Adding or running anomaly detection

☐ Creating or updating themes

☐ Exporting to CSV

☐ Exporting to Excel

☐ Sharing analyses

☐ Sharing dashboards

☐ Sharing datasets

**Datasets**

☐ Creating or updating all datasets

☐ Creating or updating only SPICE datasets

**Data sources**

☑ Creating or updating all data sources

**Folders**

☐ Creating shared folders

☐ Renaming shared folders

**Reports**

☐ Creating or updating email reports

☐ Subscribing to email reports

**Threshold Alerts**

☐ Creating or updating threshold alerts

Figure 9.4 – Editing custom permissions

**Note**

When defining custom permissions using the QuickSight Console, you choose which actions you want to *restrict* access to. The rest of the actions will be permitted based on which cohort the user belongs to.

5. Before we assign permissions, it is worth confirming that our Author user will be able to create a new dataset. Log into the QuickSight Console as the Author user and confirm that you can create a new dataset by selecting **New dataset**.

6. Next, we will need to add a new Author user. Make sure you use the IAM credentials for this user by ticking the IAM checkbox when registering the user. For this example, we can call the user `author-iam`.

7. After you register the new author-iam user, we will assign the new custom permissions profile to them. Using the AWS CLI and as the QuickSight Admin user, type the following command, replacing the highlighted values with those from your environment:

```
$aws quicksight update-user --user-name author-iam
--role AUTHOR --custom-permissions-name custom-author
--email <your-email> --aws-account-id <account-id>
--namespace default --region us-east-1
```

8. Now, we have attached our custom permissions to our newly created Author user. We can verify that the console access is as expected. Log out and log back into the QuickSight Console as the author-iam user.

9. Next, select **Datasets** from the left-hand side menu, and then select **Create a Dataset**. Your screen should look like the following figure, giving you no options for creating a new dataset:

![](img/33bad1d211a2d93fd743a1978d403f9a_199_0.png)

Figure 9.5 – Applying custom permissions

By applying custom permissions, we were able to override the default Author behavior and prevent our newly registered Author user from creating new data sources. You can configure multiple custom permissions profiles in your QuickSight account and allocate them to your users. A user cannot have more than one custom permissions profile.

10. As a final step, we will deregister the author-iam user from QuickSight, since we no longer need them. To deregister the user, you can either use the QuickSight Admin Console or the AWS CLI by typing the following command and replacing the highlighted value with the values from your environment:

```
aws quicksight delete-user --user-name author-iam
--aws-account-id <account-id> --namespace default
--region us-east-1
```

Now that we have learned how to configure custom permissions, in the next section, we will learn how to integrate QuickSight with Amazon Lake Formation.

## Integrating with Amazon Lake Formation

In this section, we will learn about the QuickSight integration with Amazon Lake Formation. **Amazon Lake Formation** provides an additional permission layer above the **AWS Glue Data Catalog**, allowing you to set up fine-grained permissions on top of your data lake on AWS. The integration with Lake Formation is useful for **Amazon Athena** datasets. Combined with QuickSight, Lake Formation will allow you to manage your data permissions from a single place, enforcing the permissions at the data lake layer by enhancing the existing QuickSight fine-grained permissions. To understand the value added by Lake Formation, we will first need to understand how to apply permissions for Athena datasets without Lake Formation.

## Configuring Amazon Athena datasets permissions without Amazon Lake Formation

To better understand the value of Lake Formation, it is important to understand how to configure permissions for Athena datasets. We will use the demo data we configured in _Chapter 1, Introducing the AWS Analytics Ecosystem_. Specifically, we have configured the following AWS Glue database:

- Database: my-data-lake-db
- Table name: yellowtrips
- S3 location: `s3://data-lake-xxxxxxxx/yellowtrips/`

In order to be able to create the Athena dataset, you will need QuickSight to do the following:

- Configure access to Amazon Athena.
- Configure access to the underlying S3 buckets.

In _Chapter 2, Introduction to Amazon QuickSight_, we learned how to give QuickSight access to **Amazon S3** buckets. You can follow a similar process to give access to the Athena service.

To enable Athena access, we can use the following steps:

1. First, log into the AWS Console as the QuickSight Admin user and select **Manage QuickSight**.
