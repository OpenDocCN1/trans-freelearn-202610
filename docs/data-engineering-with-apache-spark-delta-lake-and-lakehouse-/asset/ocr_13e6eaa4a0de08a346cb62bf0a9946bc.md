```
RESOURCEGROUPNAME="training_rg"
```

```
LOCATION="eastus"
```

```
KEY=`az storage account keys list -g $RESOURCEGROUPNAME -n $STORAGEACCOUNTNAME | jq .[0].value`
```

```
az storage blob upload --container-name $STORAGECONTAINER --account-name $STORAGEACCOUNTNAME --name inventory/1/inventory.csv --file ~/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/prep/inventory/1/inventory.csv --account-key $KEY
```

```
az storage blob upload --container-name $STORAGECONTAINER --account-name $STORAGEACCOUNTNAME --name inventory/2/inventory.csv --file ~/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/prep/inventory/2/inventory.csv --account-key $KEY
```

```
az storage blob upload --container-name $STORAGECONTAINER --account-name $STORAGEACCOUNTNAME --name inventory/3/inventory.csv --file ~/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/prep/inventory/3/inventory.csv --account-key $KEY
```

We just uploaded three inventory files to Azure Data Lake. Before we move on, let's understand what each file represents:

+   - File: `inventory/1/inventory.csv`:
+     - Received on Day 1: 2021-04-18
+     - Schema: `inventory_date,product, inventory`:

![](bbox=[0.29, 0.58, 0.695, 0.82])

Figure 10.2 – Data for the inventory file sent on 2021-04-18