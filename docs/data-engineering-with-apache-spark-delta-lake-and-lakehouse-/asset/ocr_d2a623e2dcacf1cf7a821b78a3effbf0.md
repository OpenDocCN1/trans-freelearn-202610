6. We are now ready to create the Spark cluster. Using the menu on the left, click on Compute, and then click on Create Cluster:

+   - Cluster name: `curationcluster`.
+   - Cluster mode: `Single node`.
+   - Edit the following command as per your Azure storage account name and key: `fs.azure.account.key.<storage-account-name>.blob.core.windows.net <storage-account-access-key>`.
+   - Click on **Advanced options**:
+   - Paste the edited command from the previous step in the **Spark Config** field as the last line. Now, click the **Create Cluster** button:

![](bbox=[0.175, 0.37, 0.795, 0.79])

Figure 7.22 – Azure Databricks cluster creation option

This will spin up a small Apache Spark cluster that we will use for invoking Spark and Delta Lake commands.