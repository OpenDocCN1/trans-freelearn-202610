

2. Open the Cloud Shell and choose the mode you want, that is, either Bash or PowerShell.

3. Then, we can run the Terraform command line in the shell.

The following is a screenshot of the execution of Terraform in Azure Cloud Shell:

![](img/d3344b39c888aab0b832625465f837c9_59_0.png)

Figure 2.5 – Azure Cloud Shell

The advantage of using this solution is that we don’t need any software to install; we can simply upload your Terraform files to Cloud Shell and run them in Cloud Shell. Additionally, we are already connected to Azure, so no configuration is required (please refer to the *Configuring Terraform for Azure* section).

However, this solution is only to be used in development mode and not for the local or automatic use of Terraform. For this reason, in this chapter, we will discuss the configuration of Terraform for Azure.

Now that we have installed Terraform, we can begin using it locally to provision an Azure infrastructure. We will start with the first step, which is to configure Terraform for Azure.

## Configuring Terraform for Azure

Before writing the Terraform code in which to provision a cloud infrastructure such as Azure, we must configure Terraform to allow the manipulation of resources in an Azure subscription.

To do this, first, we will create a new Azure **Service Principal (SP)** in Azure Active **Directory (AD)**, which, in Azure, is an _application user_ who has permission to manage Azure resources.

> **Important Note**
> For more details about the Azure SP, please read the documentation at https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals.

For this Azure SP, we have to assign to it the contributing permissions on the subscription in which we will create resources.

## Creating the Azure SP

This operation can be done either via the Azure portal (all steps are detailed within the official documentation at https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal) or via a script by executing the following `az cli` command (which we can launch in Azure Cloud Shell).

The following is a template `az cli` script that you have to run to create an SP. Here, you have to enter your SP name, role, and scope:

```bash
az ad sp create-for-rbac --name="<ServicePrincipal name>"
--role="Contributor" --scopes="/subscriptions/<subscription Id>"
```

Take a look at the following example:

```bash
az ad sp create-for-rbac --name="SPForTerraform"
--role="Contributor" --scopes="/subscriptions/8921-1444-..."
```

This sample script creates a new SP, named `SPForTerraform`, and gives it the contributor permission on the subscription ID, that is, `8921....`

## Important Note

For more details about the Azure CLI command to create an Azure SP, please refer to the documentation at https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest.

The following screenshot shows the execution of the script that creates an Azure SP:

![](img/d3344b39c888aab0b832625465f837c9_61_0.png)

## Figure 2.6 – Creating an Azure SP

The creation of this Azure SP returns three pieces of identification information:

- The application ID, which is also called the client ID
- The client secret
- The tenant ID

The SP is created in Azure AD. The following screenshot shows the Azure AD SP:

![](img/d3344b39c888aab0b832625465f837c9_61_1.png)

## Figure 2.7 – The App registrations list in the Azure portal

Here, we have just discovered how to create an SP in Azure AD, and we have given it permissions to manipulate the resources of our Azure subscriptions.

Now, let's learn how to configure Terraform to use our Azure SP.

## Configuring the Terraform provider

Once the Azure SP has been created, we will configure our Terraform configuration to connect to Azure using this SP. To do this, follow these steps:

1. In a directory of your choice, create a new filename, `provider.tf` (the `extension .tf` file corresponds to Terraform files), which contains the following code:

```hcl
provider "azurerm" {
  features {}
  subscription_id = "<subscription ID>"
  client_id = "<Client ID>"
  client_secret = "<Client Secret>"
  tenant_id = "<Tenant Id>"
}
```

In the preceding code, we indicate that the provider we are using is `azurerm`. The authentication information to Azure is the SP that has been created, and we add new block **features** that provide the possibility that we can customize the behavior of the Azure provider resources.

However, for security reasons, it is not advisable to put identification information in plain text inside your configuration, especially if you know that this code might be accessible by other people.

2. Therefore, we will improve the preceding code by replacing it with this one:

```hcl
provider "azurerm" {
  features {}
}
```

3. So, we delete the credentials in the Terraform configuration, and we will pass the identification values to specific Terraform environment variables:

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`

> **Important Note**
> For more information regarding the azurerm provider, please refer to the documentation at https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs. We will learn how to set these environment variables later in this chapter, in the Running Terraform form deployment section.

As a result, the Terraform code no longer contains any identification information.

We have just learned how to configure Terraform for Azure authentication. Now, we will explain how to quickly configure Terraform to perform local development and testing.

## The Terraform configuration for local development and testing

When you work locally and want to test the Terraform code quickly—for example, in a sandbox environment—it might be more convenient and faster to use your own Azure account instead of using an SP.

To do this, it is possible to connect to Azure beforehand using the az login command. Then, enter your identification information in the window that opens.

The following is a screenshot of the Azure login window:

![](img/d3344b39c888aab0b832625465f837c9_63_0.png)

Figure 2.8 – The Azure Sign in page with Azure login

If several subscriptions are accessed, the desired one can be selected using the following command:

```bash
az account set --subscription="<Subscription ID>"
```

Then, we configure the Terraform provider as before using the `provider`, "azurerm" `{ }`. Of course, this authentication method should not be done in the case of an execution on a remote server.

> Important Note
> For more information regarding the provider configuration, please refer to the documentation at https://www.terraform.io/docs/providers/azurerm/index.html.

Therefore, the Terraform configuration for Azure is defined by the configuration of the provider that uses the information from an Azure SP.

Once this configuration is complete, we can start writing the Terraform configuration to manage and provision Azure resources.

## Writing a Terraform script to deploy an Azure infrastructure

To illustrate the use of Terraform to deploy resources in Azure, we will provide a simple Azure architecture with Terraform that is composed of the following components:

- There’s an Azure resource group.
- There’s also a network configuration that is composed of a virtual network and a subnet.
- In this subnet, we will create a virtual machine that has a public IP address in order to be publicly available.

To do this, in the same directory where we previously created the `provider.tf` file, we will create a `main.tf` file with the following code:

1. Let’s start with the code that provides the `resource` group:

```hcl
resource "azurerm_resource_group" "rg" {
  name = "bookRg"
  location = "West Europe"
}
```

