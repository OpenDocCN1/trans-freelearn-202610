![](bbox=[0.12, 0.09, 0.86, 0.42])

Figure 10.3 – Upgrading to a storage account with Azure Data Lake Gen2 capabilities

After a successful migration, we will get the following message: This account has successfully been upgraded to an account with Azure Data Lake Gen2.

In the next section, we will create an Azure Data Lake instance using the Azure CLI.

# Creating an Azure Data Lake instance using the Azure CLI

We will run the Azure Cloud Shell browser tool to execute the following steps:

+   1. Create a new resource group to deploy our Azure Data Lake Storage into, and add the name and the location:

```
$ az group create --name healthcareRG --location eastus
```