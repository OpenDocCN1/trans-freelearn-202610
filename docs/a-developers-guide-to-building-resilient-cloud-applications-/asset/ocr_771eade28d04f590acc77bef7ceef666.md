In the following figure, we will execute the previous command line to create a new resource group:

```
hamida@Azure:~$ az group create --name healthcareRG --location eastus
{
  "id": "/subscriptions/
  b/resourceGroups/healthcareRG",
  "location": "eastus",
  "managedBy": null,
  "name": "healthcareRG",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

Figure 10.4 – The result of adding a new resource group

+   2. Create a managed identity:

```
$ az identity create -g healthcareRG -n user1
```

The output is shown in the following screenshot:

```
hamida@Azure:~$ az identity create -g healthcareRG -n user1
{
  "clientId": "40268d7a-7038-4037-97d3-5865ad988122",
  "id": "/subscriptions/",
  "location": "eastus",
  "name": "user1",
  "principalId": "64bd7ba9-7afe-424c-b08b-fc36178d74a4",
  "resourceGroup": "healthcareRG",
  "tags": {},
  "tenantId": "1800b09f-2b66-425c-b16c-3d7dde051282",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Figure 10.5 – The result of creating a managed identity

+   3. Create a new storage area with Data Lake Storage Gen2, but before that, add an extension to the Azure CLI so that it can use the different features for Data Lake Storage Gen2:

```
$ az extension add --name storage-preview
$ az storage account create --name myhealthcarestorage1 \
  --resource-group healthcareRG \
  --location eastus --sku Standard_LRS \
  --kind StorageV2 --hns true
```