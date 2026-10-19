# Creating the Electroniz infrastructure CI/CD pipeline

Finally, we can start developing our first CI/CD pipeline. For obvious reasons, we have chosen to develop the infrastructure pipeline first. The reason for this is that the code pipeline directly depends on the Azure resources that are created by the infrastructure pipelines. Let's get started:

+   1. You may recall that the recommended pipeline design included an Azure Key Vault to store the secrets for the CI/CD pipelines. Run the following commands in Azure Cloud Shell to create one:

```
RESOURCEGROUPNAME="elz_prod"
LOCATION="eastus"
IAC_ENVIRONMENT=prod
KEYVAULTNAME="deploykvelz"
az group create --name $RESOURCEGROUPNAME --location $LOCATION
az keyvault purge -n $KEYVAULTNAME
az config set extension.use_dynamic_install=yes_without_prompt
az keyvault create --location $LOCATION --name $KEYVAULTNAME --resource-group $RESOURCEGROUPNAME --enabled-for-template-deployment true
```

+   2. We will now add a couple of secrets to the newly created Azure Key Vault. Using the Azure portal, navigate to Home > Resource groups > elz_prod > deploykvelz.

Click on Secrets. Then, create two new secrets using Generate/Import, as follows:

+   - Name: SQLUSER Value: salesadmin
+   - Name: SQLPASSWORD Value: <password for Azure SQL administrator>: