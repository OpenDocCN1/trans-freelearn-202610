

2. 与使用 Azure 存储密钥类似，你应该谨慎使用 SAS URI。不要在代码或部署文件中公开 URI，而应将其作为密钥存储在 Azure Key Vault secrets manager 中。你可能还记得我们之前创建了一个名为 trainingkv100 的 Azure Key Vault：

- 在 Azure 门户中，浏览至 Home > All Resources > trainingkv100。
- 使用左侧菜单，点击 Secrets。
- 点击 Generate/Import 并提供以下属性：
  - Name: ADLSSASKEY
  - Value: SAS token from the step above

3. 新密钥创建完成后，点击 ADLSSASKEY。之后，点击 current version 下方的条目并记下 Secret Identifier 属性。我们稍后会用到它。

4. 此时，我们可以开始构建金层的管道。连接到 Azure 门户并点击 Home > All Resources > trainingdatafactory100。
使用左侧面板，点击 Manage。我们将首先创建 Azure Synapse Serverless 数据库的链接服务。我们希望为聚合管道创建几个链接服务。

5. 要创建第一个 Azure Synapse 链接服务，点击 Create linked service。
在 New linked service 窗口中，选择 Azure SQL Database 的图标。点击 Continue。会打开一个名为 New linked service (Azure SQL Database) 的新窗口。提供以下属性：

- Name: Synapse Serverless.
- Account selection method: Enter manually
- Fully qualified domain name: trainingsynapse100-ondemand.sql.azuresynapse.net.
- Adjust the entry as per your SERVERLESS_SQL_ENDPOINT.
- Database name: gold.
- Authentication type: SQL authentication.
- User name: sqladminuser.
- Password: <SYNAPSE PASSWORD SET PREVIOUSLY>.

点击 **Test connection**。如果连接成功，点击 **Create:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_274_0.png)

Figure 8.13 – Data factory linked service options for an Azure Synapse SQL serverless pool

6. 很可能你已经有了第二个链接服务，名为 **traininglakehouse**，因为我们在启动 **electroniz_batch_ingestion_pipeline** 时创建了它。

如果没有，点击 **Create linked service.**

现在，点击 **New**。在 **New linked service** 窗口中，选择 Azure **Data Lake Storage Gen2** 的图标。点击 **Continue.**

会打开一个名为 **New linked service (Azure Data Lake Storage Gen2)** 的新窗口。提供以下属性：

- **Name:** `traininglakehouse`
- **Authentication method:** `From Azure subscription`
- **Azure subscription:** `Free Tier or Paid Subscription`
- **Storage account name:** `traininglakehouse`

点击 **Test connection**。如果连接成功，点击 **Create.**

7. 现在，我们准备创建管道。点击 Author & Monitor。

使用左侧面板，点击 Author 菜单。在 Azure Data Factory Resources 面板中，点击 Pipelines 右侧的三个点并选择 New Pipeline。

在 Properties 面板中输入以下内容：

- Name: electroniz_aggregation_pipeline
- Description: This pipeline will aggregate data from the sales database, currency conversion, geo-location, and ecommerce website tracking.

8. 现在，点击 Activities 部分中的空白区域。在 Parameters 下方的面板中，创建以下参数：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_275_0.png)

Figure 8.14 – Creating parameters for electroniz_aggregation_pipeline

9. 在 Activities 面板中搜索 Web。现在，将 Web activity 拖到右侧面板并提供以下属性：

- Name: Get Secret.
- Click on Settings.
- URL: Enter the Secret Identifier property of ADLSSASKEY from the Azure Key Vault secret we created previously. Make sure you add ?api-version=7.0 to the end of the URL; for example, https://trainingkv100.vault.azure.net/secrets/ADLSSASKEY/2d13f6f42ff14e53ae953e42c36c32c0?api-version=7.0.

Make sure you adjust the name of the Azure Key Vault as per your settings:

- **Authentication**: `Managed Identity`
- **Resource**: `https://vault.azure.net:`

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_276_0.png)

General   Settings   User properties

URL *
https://kvault1000.vault.azure.net/secrets/

Method *
GET

Headers
+ New

Datasets
+ Add dataset reference

Linked services
+ Add linked service reference

Integration runtime *
AutoResolveIntegrationRuntime

Advanced

Authentication
○ None   ○ Basic  ● Managed Identity   ○ Client Certificate

Figure 8.15 – The Get Secret activity in electroniz_aggregation_pipeline

10. 在 Activities 面板中搜索 Stored Procedure。现在，将 Stored procedure activity 拖到右侧面板并提供以下属性：

- **Name**: `Drop Gold Layer Tables.`
- Click on **Settings**.
- **Linked service**: `Synapse Serverless.`
- **Stored procedure name**: `[dbo].[drop_gold_tables]`.

- Under **Stored procedure parameters**: Add one parameter. Start by clicking New:

- **Name:** `goldns` **Type:** `String`
  **Value:** `@pipeline().parameters.GOLD_LAYER_NAMESPACE:`

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_277_0.png)

Figure 8.16 – Parameters of the stored procedure

11. 现在，将 **Get Secret** 链接到 **Drop Gold Layer Tables**。完成后，管道应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_277_1.png)

Figure 8.17 – Chaining activities – Get Secret to Drop Gold Layer Tables

12. 在 **Activities** 面板中搜索 `Stored Procedure`。现在，将 **Stored procedure** activity 拖到右侧面板并提供以下属性：

- **Name:** `Create Synapse Credentials.`

- Click on **Settings.**

- **Linked service:** `Synapse Serverless.`

- **Stored procedure name:** `[dbo].[create_synapse_creds]`.

- Under **Stored procedure parameters**: Add five parameters. Start by clicking New:

- **Name:** `format` **Type:** `String`
  **Value:** `@pipeline().parameters.EXT_TAB_FILE_FORMAT`

- **Name:** `extds` **Type:** `String`
  **Value:** `@pipeline().parameters.EXT_DATA_SOURCE`

- **Name:** creds  **Type:** String
  **Value:** @pipeline().parameters.EXT_CREDENTIALS
- **Name:** secret  **Type:** String
  **Value:** @activity('Get Secret').output.value
- **Name:** location  **Type:** String
  **Value:** @pipeline().parameters.EXT_TAB_LOCATION:

Linked service * Synapse Serverless Test connection Edit New
Stored procedure name * [dbo].[create_synapse_creds] Edit

Stored procedure parameters
Import New Delete

| Name | Type | Value |
|---|---|---|
| format | String | @pipeline().parameters.EXT_TAB_FILE... |
| extds | String | @pipeline().parameters.EXT_DATA_SO... |
| creds | String | @pipeline().parameters.EXT_CREDENTI... |
| secret | String | @activity('Get Secret').output.value |
| location | String | @pipeline().parameters.EXT_TAB_LOCA... |

Figure 8.18 – Parameters of the create_synapse_creds stored procedure

13. 现在，将 Drop Gold Layer Tables 链接到 Create Synapse Credentials。完成后，管道应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_278_0.png)

Figure 8.19 – Chaining activities – Drop Gold Layer Tables to Create Synapse Credentials

14. 在 Activities 面板中搜索 Stored Procedure。现在，将 Stored procedure activity 拖到右侧面板并提供以下属性：

- **Name:** Create Silver Layer Views.
- Click on **Settings**.
- **Linked service:** Synapse Serverless.
- **Stored procedure name:** [dbo].[create_silver_views].
- Under **Stored procedure parameters**: Add two parameters. Start by clicking New:
  - **Name:** silverns  **Type:** String
    **Value:** @pipeline().parameters.SILVER_LAYER_NAMESPACE
  - **Name:** location  **Type:** String
    **Value:** @pipeline().parameters.EXT_TAB_LOCATION:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_279_0.png)

Figure 8.20 – Parameters for the create_silver_views stored procedure

