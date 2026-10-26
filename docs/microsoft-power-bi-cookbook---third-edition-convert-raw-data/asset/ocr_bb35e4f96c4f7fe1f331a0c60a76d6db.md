5. Review the options in the Backup Database dialog and then click the OK button:

![](bbox=[0.19, 0.1, 0.91, 0.3])

Figure 13.22: Backup Database options in SSMS

This completes backing up the semantic model.

# How it works

Connecting workspaces to an Azure Data Lake Gen2 storage account and correctly configuring the XMLA endpoint for a workspace to Read Write. It may seem odd that backing up a semantic model requires the XMLA endpoint to be configured for both read and write access since backing something up generally only requires read access. However, consider that the backup file (.abf) is being backed up to the Azure Data Lake Gen2 storage account via the XMLA endpoint and thus, write access is required for the XMLA endpoint.

# There’s more...

In addition to backing up a semantic model via the SSMS user interface, the backup can also be performed via running a query. The below query could be run to back up a semantic model of a Power BI workspace connected via the XMLA endpoint:

```
{
  "backup": {
    "database": "Designing Mobile Layouts",
    "file": "Designing Mobile Layouts.abf",
    "allowOverwrite": true,
    "applyCompression": true
  }
}
```

Finally, you can confirm that the backup file was created via Azure Storage Explorer. Azure Storage Explorer can be downloaded here: https://azure.microsoft.com/en-us/products/storage/storage-explorer/.