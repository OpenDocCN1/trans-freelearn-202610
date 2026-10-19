This is better represented as follows:

+   - Assume User 1 and User 2 perform a write operation to a delta table at the same time.
+   - Delta Lake records the version of the table before any change has been made to the delta table.
+   - The write from User 1 commits successfully, and a success message is sent back.
+   - The write from User 2 does not fail. Instead, Delta Lake tries to silently resolve conflicts from User 2's transaction against the previously committed data. If no conflicts are discovered, the transactions are committed, else an error message is sent back to User 2.

This type of concurrency control is also known as **optimistic concurrency control**. Now that we are done with most of the work here, we will go ahead and clean up the Azure resources that we used in the following section.

# Cleaning up Azure resources

For this training exercise, we created a `scratch` namespace in Azure Data Lake storage. This namespace is not required for the rest of the book, so you may delete i:

+   1. Invoke the following commands in the Cloud Shell to delete the `scratch` namespace:

```
SCRATCH_NAMESPACE="scratch"
```

```
STORAGEACCOUNTNAME="traininglakehouse"
```

```
az storage fs delete -n $SCRATCH_NAMESPACE --account-name $STORAGEACCOUNTNAME --yes
```

+   2. Invoke the following commands in the Cloud Shell to delete the Azure Databricks workspace:

```
RESOURCEGROUPNAME="training_rg"
```

```
WORKSPACE="trainingdatabricks"
```

```
az config set extension.use_dynamic_install=yes_without_prompt
```

```
az databricks workspace delete --resource-group $RESOURCEGROUPNAME --name $WORKSPACE
```

The resources have now been deleted and we should be good to go!