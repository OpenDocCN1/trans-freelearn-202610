# # Setting up multitenancy

In this chapter, we will set up a simple multi-tenant environment using the namespace API. This exercise will help you understand how to set up multi-tenant environments as a QuickSight Admin user. It will also help you understand what your BI users’ experience will be in multi-tenant scenarios, and how QuickSight prevents users from sharing assets with other tenants.

# ## Creating a namespace

Creating namespaces is simple, and it can be done using the QuickSight API. At the time of writing, it is not possible to create namespaces through the QuickSight **graphical user interface (GUI)**:

+   1. First, you need to log into Amazon QuickSight as the QuickSight Admin user.
+   2. Next, open the **AWS Command Line Interface (CLI)**, and type the following command, replacing the highlighted values with the values from your environment. For this exercise, we will create a custom namespace called `companyA`. Note that we already have a default namespace for our existing Admin, Author, and Reader users used throughout this book:
+   ```
+   $aws quicksight create-namespace --aws-account-id <account-id> --namespace companyA --identity-store QUICKSIGHT
+   ```
+   This will result in an asynchronous creation of the new namespace. Note, in the CLI response, the status is `CREATING`:
+   ```
+   {
+    "Status": 202,
+    "Name": "companyA",
+    "CapacityRegion": "us-east-1",
+    "CreationStatus": "CREATING",
+    "IdentityStore": "QUICKSIGHT",
+    "RequestId": "xxxxxx-xxxxx-xxxxxxxxxxxxxx"
+   }
+   ```
+   3. To confirm that the new namespace has been created, we will use the `list-namespaces` CLI command. Type the following command, replacing the highlighted values with those from your environment:
+   ```
+   $aws quicksight list-namespaces --aws-account-id <aws-account-id>
+   ```