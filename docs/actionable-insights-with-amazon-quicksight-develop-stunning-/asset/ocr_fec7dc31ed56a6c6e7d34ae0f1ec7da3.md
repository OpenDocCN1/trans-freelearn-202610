Note that we didn’t need to give access directly to the Reader user for this dashboard.
Instead, we added the Reader user into the **Marketing** group, and then we allowed all
users from that group to have access to our published dashboard. In the future, if we
had more users joining the **Marketing** group, they would automatically assume access
to resources shared with that particular group, eliminating the need to define specific
permissions at the individual user level.

Now that we have learned about user groups and how to use them to simplify our
permissions, in the next section, we will talk about custom permissions.

# Setting up custom permissions

In _Chapter 2, Introduction to Amazon QuickSight_, we learned the different user cohorts:
Admin, Author, and Reader. The user cohort determines the level of access to features in
the QuickSight console. Custom profiles allow you to override the default permissions with
custom security profiles and define permissions that fit your organization’s requirements.

> Note
> For custom permissions to work, you will need to be using IAM federated
> users.

To better understand custom permissions, we will use a simple hands-on example. By
default, an Author user is able to configure a new data source. For our example, let’s
assume that to protect our **Amazon SPICE** space, we want to prevent authors from
creating new data sources:

+   1. First, log into the QuickSight Console as a QuickSight Admin user.
+   2. Expand the QuickSight menu in the top-right corner of the screen and select
+   **Manage QuickSight**.
+   3. With the **Manage users** option selected, click on **Manage permissions**, as shown in
+   the following screenshot:

![](bbox=[0.1641, 0.7359, 0.8125, 0.8438])

Figure 9.3 – Accessing custom permissions