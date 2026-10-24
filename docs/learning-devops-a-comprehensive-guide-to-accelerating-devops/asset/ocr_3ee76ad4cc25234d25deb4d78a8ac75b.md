If several subscriptions are accessed, the desired one can be selected using the following command:

```
az account set --subscription="<Subscription ID>"
```

Then, we configure the Terraform provider as before using the `provider`, "azurerm" `{ }`. Of course, this authentication method should not be done in the case of an execution on a remote server.

> Important Note
> For more information regarding the provider configuration, please refer to the documentation at https://www.terraform.io/docs/providers/azurerm/index.html.

Therefore, the Terraform configuration for Azure is defined by the configuration of the provider that uses the information from an Azure SP.

Once this configuration is complete, we can start writing the Terraform configuration to manage and provision Azure resources.

# # Writing a Terraform script to deploy an Azure infrastructure

To illustrate the use of Terraform to deploy resources in Azure, we will provide a simple Azure architecture with Terraform that is composed of the following components:

+   - There’s an Azure resource group.
+   - There’s also a network configuration that is composed of a virtual network and a subnet.
+   - In this subnet, we will create a virtual machine that has a public IP address in order to be publicly available.

To do this, in the same directory where we previously created the `provider.tf` file, we will create a `main.tf` file with the following code:

+   1. Let’s start with the code that provides the `resource` group:

```
resource "azurerm_resource_group" "rg" {
  name = "bookRg"
  location = "West Europe"
}
```