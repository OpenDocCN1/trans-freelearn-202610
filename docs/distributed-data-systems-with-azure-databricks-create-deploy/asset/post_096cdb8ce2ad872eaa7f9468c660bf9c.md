
With workspace access control enabled, the following permissions exist:

- Only admins can create items in the **Workspace** folder, but users can manage existing items.
- Permissions applied to a folder will be applied to the items it contains.
- Users keep having **Manage** permission to their home directories.

Understanding these permissions helps us to know in advance how possible changes in these policies could affect how users interact with the organization's data.

## Notebook permissions

Notebooks have the same five permission levels as folders: **No Permissions**, **Read**, **Run**, **Edit**, and **Manage**.

## Configuring notebook and folder permissions

Users can configure notebook permissions by clicking on the **Permissions** button in the notebook context bar. Select the folder and then click on **Permissions** from the drop-down menu:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_52_0.png)

From there, you can grant permissions to users or groups as well as edit existing permissions:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_52_1.png)

Access control on notebooks can easily be applied in this way by selecting one of the options from the drop-down menu.

## MLflow Model permissions

You can assign six permission levels to **MLflow Models** registered in the **MLflow Model Registry**: **No Permissions**, **Read**, **Edit**, **Manage Staging Versions**, **Manage Production Versions**, and **Manage**.

## Default MLflow Model permissions

Besides the current workspace access control, these permissions are maintained:

- Models in the registry can be created by anyone.
- Administrators can manage any model in the registry.

When there is no workspace access control, users can manage any of the models in the registry.

With workspace access control enabled, the following permissions exist:

- Users can manage only the models they have created.
- Only administrators can manage models created by other users.

These options are applied to MLflow Models created in Azure Databricks.

## Configuring MLflow Model permissions

One thing to keep in mind is that only administrators belong to the admins with the **Manage** permissions group, while the rest of the users belong to the **all users** group.

MLflow Model permissions can be modified by clicking on the model's icon in the sidebar, selecting the model name, clicking on the drop-down icon to the right of the model name, and finally selecting **Permissions**. This will show us a dialog box from which we can select specific users or groups and add specific permissions:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_53_0.png)

Figure 1.42 – MLflow permissions

You can update the permissions of a user or group by selecting the new permission from the **Permission** drop-down menu:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_54_0.png)

Figure 1.43 – MLflow access management

By selecting one of these options, we can control how MLflow experiments interact with our data and which users can create models that work with it.

## Summary

In this chapter, we have tried to cover all the main aspects of how Azure Databricks works. Some of the things we have discovered include how notebooks can be created to execute code, how we can import data to use, how to create and manage clusters, and so on. This is important because when creating ETLs and ML experiments in Azure Databricks within an organization, aside from how to code the ETL in our notebooks, we will need to know how to manage the data and computational resources required, how to share assets, and how to manage the permissions of each one of them.

In the next chapter, we will apply this knowledge to explore in more detail how to create and manage the resources needed to work with data in Azure Databricks, and learn more about custom VNets and the different alternatives that we have in order to interact with them, either through the Azure Databricks UI or the CLI tool.

## 2
Creating an Azure Databricks Workspace

In this chapter, we will apply all the concepts we explored in *Chapter 1, Introduction to Azure Databricks*. We will create our first Azure Databricks workspace using the UI, and then explore the different possibilities of resource management through the Azure CLI, how to deploy these resources using the ARM template, and how we can integrate Azure Databricks within our virtual network using VNet injection.

In this chapter, we will discuss the following topics:

- Using the Azure portal UI
- Examining Azure Databricks authentication
- Working with VNets in Azure Databricks
- Azure Resource Manager templates
- Setting up the Azure Databricks CLI

We will first begin by creating our workspace from the Azure portal UI.

## Technical requirements

The most important prerequisite for this chapter is to already have an Azure subscription with funds and permissions. Remember that this is a pay-as-you-go service, but nevertheless, you can create a free trial subscription; check out more information about this option in the Azure portal (https://azure.microsoft.com/en-us/free/).

## Using the Azure portal UI

Let's start by setting up a new Databricks workspace through the Azure portal UI:

- 1. Log in to the Azure portal of your subscription and navigate to the Azure services ribbon.
- 2. Click on Azure Databricks:
- 3. This will lead you to the Azure Databricks default folder in which you will see all your resources listed. Click on Create new resource to create an Azure Databricks workspace environment:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_56_0.png)

Figure 2.1 – Creating an Azure Databricks service

![](img/7e45bd08916c2a17d4e75cbb898f41e8_56_1.png)

Figure 2.2 – Your Azure Databricks deployed resources

4. Once you click on Create azure databricks service, you will have to fill in a couple of details regarding the workspace you are creating. The settings will look something like this:

- Workspace name
- Subscription
- Resource group
- Location
- Pricing Tier
- Deploy Azure Databricks workspace in your Virtual Network (Preview)

The name of the workspace can be whatever you like, but it is always good to pick a name that is simple and references the use that it will have. For example, these environments can be acceptance or production, and each one would be its own type of subscription. The resource group can be created at that moment and must logically cover all the resources that have the same purpose.

The location to deploy your resources is also important because it must be close to the point of service or to the other deployed resources with which it must work together.

> **Note**
> Regarding the pricing tier: there are three options, which are Standard, Premium, and Trial. One of the biggest differences is that Premium allows you to apply access control. This makes it a common choice within corporate environments.

One of the options presented is that you can choose to deploy in your own dedicated virtual network. In this step, we will select No, but we will talk later in the chapter about VNet and how it can be a good security practice to have all your Azure resources under the same network security policy.

5. Finally, you can create the resource, and the workspace will be automatically configured and deployed:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_58_0.png)

Figure 2.3 – Creating an Azure Databricks workspace

Now that we have successfully created our workspace, we can access it through the Workspace UI of the Azure CLI.

## Accessing the Workspace UI

Once your Azure Databricks resource has been deployed successfully, you can access the workspace. You can directly access it by clicking on the Launch Workspace button at the center of your resource in the Azure portal, or you can access it by using the URL that is shown in the center-left corner of your resource. This URL will take you to the workspace that you will access using Azure AD sign-in:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_59_0.png)

Figure 2.4 – The Azure Databricks service main panel

Once you can access the workspace, you will see different options to access your assets. You can either use the left-hand ribbon of options, or the center options to open notebooks, create new clusters, new jobs, and so on. The entire list of options accessible in the main left-hand ribbon is as follows:

- Azure Databricks
- Home
- Workspace
- Recent assets
- Data
- Clusters
