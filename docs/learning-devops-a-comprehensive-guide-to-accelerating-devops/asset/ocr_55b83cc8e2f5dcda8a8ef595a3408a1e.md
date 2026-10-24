Also, as with many of the tools already detailed in this book, InSpec has been integrated into the **Azure Cloud Shell** tool suite, as shown in the following screenshot:

![](bbox=[0.178, 0.146, 0.824, 0.409])

Figure 14.3 – InSpec in Azure Cloud Shell

We have just seen the different ways to install InSpec, and we’ll now see the configuration of Azure for InSpec.

# # Configuring Azure for InSpec

Before writing test cases to test the compliance of our Azure infrastructure, we need to create an Azure service principal that has read permission on the Azure resources that will be tested.

To create this Azure service principal, we’ll use the same procedure that we already detailed in the _Configuring Terraform for Azure_ section of _Chapter 2, Provisioning Cloud Infrastructure with Terraform_.

Using the **Azure CLI** tool, we execute the following `az cli` command:

```
```
az ad sp create-for-rbac -name="<SP name> -role="Reader" -scopes="/subscriptions/<subscription Id>"
```
```

This preceding command requires the following parameters:

+   • `-name` is the name of the Azure service principal to be created.