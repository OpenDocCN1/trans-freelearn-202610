8. Next, create the data source for the e-commerce website tracking logs and upload a sample website tracking file. Copy each of the following commands, line by line, and paste them inside the Cloud Shell window. Then, press Enter:

```
LOGSTORAGEACCOUNTNAME="ecommercetracking"
LOGSCONTAINER="ecommercelogs"
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
az storage account create --name $LOGSTORAGEACCOUNTNAME --resource-group $RESOURCEGROUPNAME --kind StorageV2 --location $LOCATION --sku Standard_LRS --tags owner=data engineering project=lakehouse environment=development
KEY=`az storage account keys list -g $RESOURCEGROUPNAME -n $LOGSTORAGEACCOUNTNAME | jq .[0].value`
az storage container create -n $LOGSCONTAINER --account-name $LOGSTORAGEACCOUNTNAME --account-key $KEY
az storage blob upload --container-name $LOGSCONTAINER --account-name $LOGSTORAGEACCOUNTNAME --name electroniz_access_log_1.json --file ~/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/prep/ecommerce_logs/electroniz_access_log_1.log --account-key $KEY
```

> Note
> Storage account names in Azure need to be globally unique. If you get a The storage account named ecommercetracking is already taken error, you will need to change the account name in the instructions in the next section.

We have now successfully created data sources for the stores database and e-commerce transactions. Now we will create the data destinations using the steps outlined in the next section.