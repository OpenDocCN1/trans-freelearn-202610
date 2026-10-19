
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

- File: `inventory/1/inventory.csv`:
  - Received on Day 1: 2021-04-18
  - Schema: `inventory_date,product, inventory`:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_340_0.png)

Figure 10.2 – Data for the inventory file sent on 2021-04-18

- **File:** `inventory/2/inventory.csv`
- **Received on Day 2:** `2021-04-19`
- **Schema:** `inventory_date,product,inventory,warehouse`
- **The schema has evolved:** A fourth column (warehouse) was added to the source:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_341_0.png)

Figure 10.3 – Data for the inventory file sent on 2021-04-19

- **File:** `inventory/3/inventory.csv`
- **Received on Day 3:** `2021-04-20`
- **Schema:** `inventory_date,product,inventory,city`

- **Schema has evolved even further:** A new column (city) was added but a previous column was deleted (warehouse):

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_342_0.png)

Figure 10.4 – Schema for the inventory file sent on 2021-04-20

- 2. Let’s find out how to effectively deal with this evolving schema scenario. Once again, for this exercise, we will use Azure Databricks to read and write data from Azure Data Lake Storage. If you have dropped the Azure Databricks workspace we created previously, you can perform the following steps to recreate it.

- 3. Get the storage account keys for Azure Data Lake Storage. Invoke the following commands in Azure Cloud Shell:

```
STORAGEACCOUNTNAME="traininglakehouse"
az storage account keys list --account-name $STORAGEACCOUNTNAME
```

If the preceding commands worked as desired, you should see two storage keys. Take note of the value for key1. We will need it later in the Databricks workspace configuration:

```
training@Azure>STORAGEACCOUNTNAME="traininglakehouse"
training@Azure>az storage account keys list --account-name $STORAGEACCOUNTNAME
[
  {
    "keyName": "key1",
    "permissions": "FULL",
    "value": " [redacted] "
  },
  {
    "keyName": "key2",
    "permissions": "FULL",
    "value": " [redacted] "
  }
]
```

Figure 10.5 – traininglakehouse keys

4. Create the Azure Databricks workspace using the following code. Invoke the following commands on Azure Cloud Shell:

```
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
WORKSPACE="trainingdatabricks"
az config set extension.use_dynamic_install=yes_without_prompt
az databricks workspace create --resource-group $RESOURCEGROUPNAME --name $WORKSPACE --location $LOCATION --sku trial
```

If you do not have an Azure trial account, you need to use an SKU of standard:

```
training@Azure>RESOURCEGROUPNAME="training_rg"
training@Azure>LOCATION="eastus"
training@Azure>WORKSPACE="trainingdatabricks"
training@Azure>az config set extension.use_dynamic_install=yes_without_prompt
Command group 'config' is experimental and under development. Reference and support
training@Azure>az databricks workspace create --resource-group $RESOURCEGROUPNAME
{\| Finished ..
  "authorizations": [
    {
      "principalId": "9a74af6f-d153-4348-988a-e2672920bee9",
      "roleDefinitionId": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635"
    }
  ],
  "createdBy": {
    "applicationId": "c44b4083-3bb0-49c1-b47d-974e53cbd f3c",
    "oid": "09a48665-df3d-42cd-bd4d-5b314a4cc3d4",
    "puid": "000300003AA12A41"
  }
```

Figure 10.6 – Azure Databricks workspace creation

5. To validate the creation of the Azure Databricks workspace, navigate to the Azure portal.

To do so, click on **Home > All Resources > trainingdatabricks**, and then click on **Launch Workspace:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_344_0.png)

Figure 10.7 – Azure Databricks workspace on the Azure portal

Using the menu on the left, click on **Compute**. Then, click on **Create Cluster**. Make the following changes:

- **Cluster name:** `curationcluster`.
- **Cluster mode:** `Single node`.

- Edit the `fs.azure.account.key.<storage-account-name>.blob.core.windows.net <storage-account-access-key>` command as per your Azure storage account name and key.
- Click on **Advanced options.**

Paste the edited command into Spark Config as the last line. Now, press the **Create Cluster** button:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_345_0.png)

Figure 10.8 – Azure Databricks Create Cluster button

6. This will spin up a small Apache Spark cluster, which will be required for this exercise. Once the cluster has been created, you should see a window that looks like this:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_346_0.png)

Figure 10.9 – Azure Databricks cluster creation

7. Now, we need a notebook that will serve as our interactive development environment (IDE).

On the Databricks workspace, click on Workspace, and then Users.

Then, click on the arrow beside your username and click on Import:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_347_0.png)

Figure 10.10 – Importing a notebook into the Azure Databricks workspace

- Choose **URL**.
- Use the following URL: https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/blob/main/project/challenges/schema_evolution/inventory_notebook.ipynb.

Click on Import.

Now, click on the cluster dropdown and choose curationcluster:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_347_1.png)

Figure 10.11 – Choosing a Spark cluster for the notebook

8. For the next set of commands, you should be on the notebook interface. Before running the notebook cells, make sure you edit STORAGE_ACCOUNT and ADLS_KEY in the Cmd 2 notebook cell, as per your Azure account.

9. Run Cmd1, Cmd2, and Cmd3. These instructions will create a Spark DataFrame named df_read_inventory1 using the Day 1=2021-04-18 inventory file from the bronze layer:

```
STORAGE_ACCOUNT="traininglakehouse"
ADLS_KEY="pK4wSkKe/E5uniwhLzpGpV3fIXSWiQISTJvyJIuE2rR2ktxl f2Crgfa+IyceY8uqItfDExUPodpKL7R3ddPkg=="
BRONZE_LAYER_NAMESPACE="bronze"
SILVER_LAYER_NAMESPACE="silver"
INVENTORY_FOLDER="inventory"
spark.conf.set("fs.azure.account.key."+STORAGE_ACCOUNT+".blob.core.windows.net", ADLS_KEY)
```

```
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+INVENTORY_FOLDER+"/1"
df_read_inventory1 = spark.read.csv(BRONZE_TABLE_PATH, header="True", inferSchema="True")
display(df_read_inventory1)
```

| inventory_date | product | inventory |
|---|---|---|
| 1 | 2021-04-18 | Watch | 100 |
| 2 | 2021-04-18 | Vaccum | 500 |
| 3 | 2021-04-18 | Airconditioner | 45 |
| 4 | 2021-04-18 | Microwave | 34 |
| 5 | 2021-04-18 | DVD | 6789 |
| 6 | 2021-04-18 | TV | 43 |
| 7 | 2021-04-18 | Keyboard | 564 |
Showing all 11 rows.

Figure 10.12 – Reading the 2021-04-18 inventory file

Notice that df_read_inventory1 has three columns: inventory_date, product, and inventory.

- Now, run Cmd4, Cmd5, Cmd6, and Cmd7. These instructions will create a Delta table using the Day 1=2021-04-18 inventory file:
