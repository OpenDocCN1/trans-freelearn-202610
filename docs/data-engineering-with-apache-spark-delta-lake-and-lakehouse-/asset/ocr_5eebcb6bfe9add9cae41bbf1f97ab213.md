20. Now, we are all set to **trigger** a full snapshot, which will copy the shared data from the **provider** account to the **consumer** storage account.

Before we trigger a **full snapshot**, we need to grant a role to the storage account.

Using a new browser tab, navigate to the Azure portal.

Click on **Home > All Resources > <Consumers Storage Account>**. 

Then, using the menu on the left, click on **Access Control (IAM)**.

From here, click on **Add role assignment**.

Now, choose **Owner** and click on **Next**.

Choose **User, group, or service principal**, and then click on **Select members**.

Choose the Azure **principal** where the name starts with the same account name that the user logs into the Azure portal with. Click **Select** and then **Next**:

# Add role assignment

Role
Members
Review + assign

**Selected role**
Owner

**Assign access to**
● User, group, or service principal
○ Managed identity

**Members**
+ Select members

| Name | Object ID | Type |
|------|-----------|------|
| [redacted] | 39ea52c6-0ad8-4736-b35a-da3222e579... | User |

Figure 10.45 – Adding a role assignment to the consumer's subscription

Finally, click **Review + assign**.