Let’s take a look, in detail, at the execution of these steps:

+   1. To create an Azure storage account and a blob container, we can use either the
+   Azure portal (https://docs.microsoft.com/en-gb/azure/storage/
+   common/storage-quickstart-create-account?tabs=azure-portal)
+   or an az cli script:
+   ```
+   # 1-Create resource group
+   az group create --name MyRgRemoteBackend --location
+   westeurope
+   # 2-Create storage account
+   az storage account create --resource-group
+   MyRgRemoteBackend --name storageremotetf --sku Standard_
+   LRS --encryption-services blob
+   # 3-Get storage account key
+   ACCOUNT_KEY=$(az storage account keys list --resource-
+   group MyRgRemoteBackend --account-name storageremotetf
+   --query [0].value -o tsv)
+   # 4-Create blob container
+   az storage container create --name tfbackends --account-
+   name storageremotetf --account-key $ACCOUNT_KEY
+   ```
+   This script creates a MyRgRemoteBackend resource group and a storage account,
+   called storageremotetf.
+   Then, the script retrieves the key account from the storage account and creates a
+   blob container, tfbackends, inside this storage account.
+   This script can be run in Azure Cloud Shell, and the advantage of using a script rather
+   than using the Azure portal is that this script can be integrated into a CI/CD process.

+   2. Then, to configure Terraform to use the previously created remote backend, we need
+   to add the configuration section within the Terraform.tf file:
+   ```
+   terraform {
+    backend "azurerm" {
+    storage_account_name = "storageremotetfdemo"
+    container_name = "tfbackends"
+    key = "myappli.tfstate"
+    snapshot = true
+    }
+   }
+   ```