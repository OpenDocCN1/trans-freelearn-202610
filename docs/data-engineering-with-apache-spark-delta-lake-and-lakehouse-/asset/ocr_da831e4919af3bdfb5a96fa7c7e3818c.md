Click Save:

Library > electroniz_infra_variables

Variable group   Save   Clone   Security   Pipeline permissions

# Properties

Variable group name

electroniz_infra_variables

Description

These variables are used by the infrastructure deployment files

Link secrets from an Azure key vault as variables

# Variables

| Name | Value |
|---|---|
| DATABRICKS_WORKSPACE_NAME | elzdbws |
| DATAFACTORY_NAME | elzdf |
| ENVIRONMENT | prod |
| EVENTHUB_CONTAINER | ecomm logs |

Figure 12.25 – The electroniz_infra_variables variable group in Azure Pipelines

6. We are now ready to create the infrastructure pipeline. Using the menu on the left, click on Pipelines:

![](bbox=[0.37, 0.57, 0.635, 0.7])

# Create your first Pipeline

Automate your build and release processes using our wizard, and go from code to cloud-hosted within minutes.

Create Pipeline

Figure 12.26 – Creating a new Azure pipeline