
Notice how the template was read and ARM successfully identified the correct number of parameters, variables, and resources created by this template. Click on Save.

6. On the Custom deployment screen, input the following values:

- **Subscription:** Free Tier or Paid Subscription.
- **Resource group:** Click Create new.
- **Name:** `elz_prod`.

Click OK.

Now input the following values:

- **Region:** East US 2
- **Environment:** prod
- **Project:** dlake

In many cases, Azure resource names are globally unique. Try to make the project name as unique as possible. Since a lot of readers will be using the same instructions, having uniqueness will eliminate unnecessary errors related to unique resource names:

- **Storage Account Name:** elz
- **Bronze Layer:** bronze
- **Silver Layer:** silver
- **Gold Layer:** gold
- **Location:** [resourceGroup().location]

Click on Review + create.

7. Check for the Validation Passed message.

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_396_0.png)

Figure 11.10 – Validating the custom ARM template

Click on Create.

8. ARM will now start the deployment. You should now see the Deployment is in progress screen as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_396_1.png)

Figure 11.11 – Deploying the custom ARM template

9. Once the deployment is finished, you should see the **Your deployment is complete** screen as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_397_0.png)

Figure 11.12 – Deployment completed confirmation message

Click on **Go to resource group**.

10. Check that a new storage account has been created. Notice the name of the storage account embeds the environment and project name.

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_397_1.png)

Figure 11.13 – Azure resources in the newly created resource group

Click on the **storage account name (proddlakeelz)**.

11. Now click on **Containers**. Notice the three newly created containers as sub-resources in the ARM template.

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_398_0.png)

Figure 11.14 – Azure resources in the newly created resource group

12. Navigate back to **Home | Resource group | elz_prod.**

Using the menu on the left, click on **Deployments**. Notice that ARM maintains a log of all deployments that were performed.

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_398_1.png)

Figure 11.15 – Checking the deployments in the newly created resource group

## Deploying ARM templates using the Azure CLI

Once the template has been developed and tested using the Azure portal, it is essential to run it using the Azure client. This will provide us with the assurance that the template is ready for use in the future using deployment scripts.

> **Important Note**
> Even though deploying an ARM template using the Azure portal is a relatively simple process, a lot of administrators prefer automated deployments using the **Azure CLI.**

This is how the Lakehouse_Storage_Account template can be deployed using Bash. Invoke the following commands using Azure Cloud Shell:

```
RESOURCEGROUPNAME="elz_prod"
LOCATION="eastus"
IAC_ENVIRONMENT=prod
FILE_PATH=~/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/infra
TEMPLATE_FILE=$FILE_PATH/storage_accounts/storage_accounts.json
PARAMETERS_FILE=$FILE_PATH/parameters/$IAC_ENVIRONMENT/storage_accounts_parameters.json
az group create --name $RESOURCEGROUPNAME --location $LOCATION
az deployment group create --name Lakehouse_Storage_Account --resource-group $RESOURCEGROUPNAME --template-file $TEMPLATE_FILE --parameters $PARAMETERS_FILE
```

As we can see, it is pretty straightforward and easy to do.

## Deploying ARM templates containing secrets

In the previous section, we deployed a relatively simple ARM template. The complexity of the template increases if the Azure resources require administrator credentials to be set while creating the resource itself. Since we are creating the infrastructure using code, it is unsafe and against security practices to leave exposed reference passwords in configuration files. In such cases, the security aspect of the template needs to be properly addressed. This security issue is frequently encountered when spinning up resources such as Azure SQL and Azure Synapse. Fortunately, Azure ARM can easily accommodate this requirement. It can retrieve secrets from Azure Key Vault at runtime during the deployment process. The process starts by creating the infrastructure secrets in Azure Key Vault. This is how it is done:

- 1. Run the following commands in Azure Cloud Shell:

```
RESOURCEGROUPNAME="elz_prod"
LOCATION="eastus"
KEYVAULTNAME="deploykvelz"
az group create --name $RESOURCEGROUPNAME --location $LOCATION
az keyvault purge -n $KEYVAULTNAME
az config set extension.use_dynamic_install=yes_without_prompt
```

```
az keyvault create --location $LOCATION --name $KEYVAULTNAME --resource-group $RESOURCEGROUPNAME --enabled-for-template-deployment true
```

If run correctly, you should see the following output:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_400_0.png)

Figure 11.16 – Output of the new Azure Key Vault creation

2. We will now add secrets to the newly created Azure Key Vault. Using the Azure portal, navigate to Home | Resource group | elz_prod | deploykvelz.

Click on Secrets. Then create two new secrets using Generate/Import:

- Name: SQLUSER   Value: salesadmin
- Name: SQLPASSWORD   Value: <password for Azure SQL administrator>

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_400_1.png)

Figure 11.17 – Checking newly created secrets in Azure Key Vault

### Important Note

Even though the creation of secrets in Azure Key Vault can be automated, it is highly recommended to perform this step manually. You can request the customer’s IT team to create the secrets for you. Using this method ensures confidentiality at their end.

3. We are now ready to deploy the Stores_Azure_SQL_Database template. Run the following commands using Azure Cloud SQL. These commands will deploy an Azure SQL Database template while dynamically fetching database credentials using secrets from Azure Key Vault created previously:

```
SERVERNAME="elzsql"

DATABASESNAME="salesdb"

VAULTNAME="deploykvelz"

PASSWORDSECRETNAME="SQLPASSWORD"

USERSECRETNAME="SQLUSER"

RESOURCEGROUPNAME="elz_prod"

LOCATION="eastus"

TEMPLATE_FILE="~/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/infra/azure_sql/azure_sql.json"

az deployment group create --name Stores_Azure_SQL_Database --resource-group $RESOURCEGROUPNAME --template-file $TEMPLATE_FILE --parameters vaultName=$VAULTNAME databaseName=$DATABASESNAME adminPasswordsecretName=$PASSWORDSECRETNAME sqlServerName=$SERVERNAME adminLoginUsersecretName=$USERSECRETNAME vaultResourceGroupName=$RESOURCEGROUPNAME
```

4. Navigate to Home | Resource group | elz_prod. If the preceding commands are successful, you should be able to see a new SQL server and database created as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_402_0.png)

Figure 11.18 – Checking the creation of the Azure SQL database

5. It would be interesting to find out how the dynamic fetch of secrets was achieved within ARM.

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_402_1.png)

Figure 11.19 – Process of ARM fetching secrets from Azure Key Vault

ARM allows for the use of nested templates. Using the parameters passed from the command line (vaultName, adminLoginUsersecretName, adminPasswordsecretName), the parent template generates the Azure Key Vault resource ID for the secrets.

```
"adminLoginUser": {
  "reference": {
    "keyVault": {
      "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters",
    },
    "secretName": "[parameters('adminLoginUserSecretName')]"
  }
},
"adminPassword": {
  "reference": {
    "keyVault": {
      "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters",
    },
    "secretName": "[parameters('adminPasswordSecretName')]"
  }
}
```

Figure 11.20 – Parent ARM templates generating the Azure vault resource ID

These resource IDs are used by the nested template to retrieve the actual value of the secret from Azure Key Vault.

> **Important Note**
> On highly secure projects, it is not becoming a standard practice to hide production secrets from data engineers. In such cases, the secrets are created by the customer's security teams. Once created, these secrets are dynamically retrieved during runtime without exposing them to unauthorized entities.

## Deploying multiple environments using IaC

In the previous section, we learned that once the IaC assets have been created by the DevOps team, they can be deployed repeatedly to create different environments, each time achieving consistent results.

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_403_0.png)

Figure 11.21 – Process of deploying multiple environments using IaC
