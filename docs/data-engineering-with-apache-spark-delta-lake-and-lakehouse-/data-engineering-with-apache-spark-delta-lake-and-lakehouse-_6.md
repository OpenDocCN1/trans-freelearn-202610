### 8
数据聚合阶段 – 金层

做得好，坚持到这里！您的旅程即将接近最后阶段。我很高兴地报告，我们熟练的司机（数据工程师）已成功将车辆（数据管道）驾驶到旅程的最后一段。在旅程的最后一段，所有之前收集并已治理的数据将被用于描述性、诊断性、预测性以及规范性分析。

在上一章中，我们对数据治理进行了深入研究。通过各种数据治理示例，我们成功构建了 Electroniz 湖屋的治理银层。在本章中，我们将继续构建 Electroniz 湖屋的下一个也是最后一个层：金层。

在本章中，我们将涵盖以下主题：

- 聚合数据的需求
- 聚合数据的流程
- 开发数据聚合管道
- 运行数据聚合管道
- 理解数据消费
- 在金层中验证聚合数据
- 满足客户期望

#### 数据聚合的需求

在我们深入探讨如何聚合数据并构建 Electroniz 湖屋的金层之前，我们应该快速提醒自己为什么要首先构建湖屋。在发现阶段，Electroniz 提出了一些关键的业务需求，如下所示：

- 他们希望实现一个接近实时销售分析平台，能够更快更好地满足客户需求。
- 他们希望通过基于电子商务网站用户点击次数的智能分析来简化广告预算。
- 他们希望减少因库存短缺导致订单履行中断而造成的客户不满。

到目前为止，我们已经能够识别、收集和治理来自各种来源的数据，以帮助我们使用现代分析进行智能决策。此时，我们在银层中拥有代表已标准化、验证、去重并已安全的数据状态的数据。但它是否提供了 Electroniz 希望我们解决的所有业务需求的答案？简而言之，答案是否定的。那么，我们该如何寻找正确的答案呢？

> **重要**
> 在数据术语中，每個数据集的银层被称为“观察”。每个数据集都有多个行，被称为“变量”。

创建金层的目标是**聚合**或**汇总**数据。**聚合**是一种用于使用一组变量在观察集合上识别模式的技术。聚合的常见示例包括给定季度总销售额、每月访问您网站的总用户数以及各部门员工的平均工资。

很大程度上，银层中数据的结构（模式）与从中摄取的源非常相似。相反，聚合数据的结构要么是像表格那样的二维，要么是像立方体那样的多维：

#### 销售表

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_263_0.png)

#### 销售立方体

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_263_1.png)

#### 二维 vs
多维聚合

图 8.1 – 二维与多维数据

让我们更详细地看一下：

- **二维聚合**总结两个给定变量上的数据。例如，某个产品的每季度累计销售额是多少？
- **多维聚合**以层级形式总结多个变量上的数据。例如，您可以根据国家/地区查找某个产品的每季度累计销售额。

现在我们知道了数据为何有用以及聚合数据为何有帮助，让我们来看一下数据聚合流程。

#### 数据聚合的流程

尽管数据聚合的目标在很大程度上很直接，但您应该遵循一个明确的流程。数据聚合流程是客户和数据工程师共同承担的责任。每个实体都需要通过业务和技术知识的结合为目标做出贡献：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_263_2.png)

图 8.2 – 数据聚合流程

该流程可以解释如下：

- **定义 KPI**：数据聚合流程始于定义**关键绩效指标（KPI）**。数据工程师并不是做出此类业务导向决策的最佳资源。因此，寻求客户的帮助很重要，因为他们更接近业务，能够更准确地做出这些决策。

> **重要**
> 用于衡量公司成败的可量化指标称为 KPI。KPI 通常用于评估公司是否达到其目标。这些 KPI 最好由足够了解业务的人来定义，以便判断某个指标是否能准确定义成功或失败标准。

- **筛选观察对象**：一旦 KPI 得到明确定义，数据工程师（在客户协助下）应筛选出将参与聚合的观察对象（数据集）列表。重要的是对数据集进行详细分析，以充分了解其相互依赖关系、分组和过滤机制。

- **汇总数据**：这是数据工程师将开发逻辑以执行必要数据聚合的阶段。聚合逻辑可以用多种编程语言开发，尽管目前最常见的两种是 SQL 和 Spark。一旦逻辑开发完成，就需要将其集成到整体管道中，该管道按计划运行。

- **可视化数据**：作为人类，我们对视觉有很好的关联。将聚合数据以彩色图表、地图和仪表板的形式可视化是现代决策者常用的方法。在第 1 章《数据工程与分析的故事》中，我们讨论了数据讲故事的重要性。可视化聚合数据讲述了关于组织健康状况的精心编织的故事。

我以“每个数据字节都有一个故事要讲。真正的问题是故事是否被准确、安全、高效地讲述”开始了本书。此时，我们准备好讲述 Electroniz 的故事了。

#### 开发数据聚合管道

在开始开发聚合管道之前，我们需要部署以下 Azure 资源：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_265_0.png)

图 8.3 – Electroniz 聚合管道

在以下部分中，我们将创建前面图中突出显示的聚合管道。





#### 准备 Azure 资源

按照以下步骤开始 Azure 资源部署流程：

- 1. 我们将首先在 Azure Data Lake Storage 中为 gold 层创建一个新的命名空间。

我之前提到过 Azure 中的存储账户名称在全球范围内是唯一的。在本练习中，我们将使用 traininglakehouse 作为存储账户名称。你需要根据你创建的账户名称进行编辑：

```
STORAGEACCOUNTNAME="traininglakehouse"
GOLDLAYER="gold"
az storage fs create -n $GOLDLAYER --account-name $STORAGEACCOUNTNAME --only-show-errors
```

这会产生以下输出：

```
Azure> STORAGEACCOUNTNAME="traininglakehouse"
Azure> GOLDLAYER="gold"
Azure> az storage fs create -n $GOLDLAYER --account-name $STORAGE
{
  "client_request_id": "e8d0df66-d385-11eb-b0e1-0a580af4683e",
  "date": "2021-06-22T18:16:04+00:00",
  "error_code": null,
  "etag": "\"0x8D935A9CD09653E\"",
  "last_modified": "2021-06-22T18:16:05+00:00",
  "request_id": "03a5b5c3-001e-0022-2f92-67f980000000",
  "version": "2020-02-10"
}
```

图 8.4 – gold 容器创建命令的输出

- 2. 如果前面的命令成功，你应该能够通过浏览至 Home > All Resources > traininglakehouse > Containers 在 Azure 门户中看到新创建的容器：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_267_0.png)

图 8.5 – gold 容器创建的确认

3. 现在，让我们创建一个指向 gold 层的新的 Azure Synapse 工作区。将 SQLPASSWORD 替换为实际密码。记住它，因为我们稍后会用到它。另外，Azure Synapse 工作区名称在全球范围内是唯一的，因此请相应地编辑 SYSNAPSEWORKSPACENAME 变量：

```
SYSNAPSEWORKSPACENAME="trainingsynapse100"
RESOURCEGROUPNAME="training_rg"
LOCATION="westus"
SQLUSER="sqladminuser"
SQLPASSWORD="XXXXXXXXX"
STORAGEACCOUNTNAME="traininglakehouse"
GOLDLAYER="gold"
az synapse workspace create \
  --name $SYSNAPSEWORKSPACENAME \
  --resource-group $RESOURCEGROUPNAME \
  --storage-account $STORAGEACCOUNTNAME \
  --file-system $GOLDLAYER \
  --sql-admin-login-user $SQLUSER \
  --sql-admin-login-password $SQLPASSWORD \
  --location $LOCATION
```

这会产生以下输出：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_268_0.png)

图 8.6 – Azure Synapse 工作区创建命令的输出

4. 现在，我们必须根据原始 IP 地址授予新 Azure Synapse 工作区的访问权限。在真实的生产环境中，建议使用的 IP 地址列表非常严格：

```
az synapse workspace firewall-rule create --name allowAll --workspace-name $SYSNAPSEWORKSPACENAME \
--resource-group $RESOURCEGROUPNAME --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

这会产生以下输出：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_268_1.png)

图 8.7 – Azure Synapse 工作区防火墙规则命令的输出

5. 如果前面的命令成功，你应该能够通过浏览至 Home > All Resources > trainingsynapse100 在 Azure 门户中看到新创建的 Azure Synapse 工作区：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_269_0.png)

图 8.8 – Azure Synapse 工作区创建的确认

6. 我们现在准备创建一个新的 Azure Synapse serverless 数据库。请注意你 Azure Synapse 工作区中的 serverless SQL 端点。用户名和密码属性与我们之前在创建 Azure Synapse 工作区时使用的一样：

```
SERVERLESS_SQL_ENDPOINT="trainingsynapse100-ondemand.sql.azuresynapse.net"
SQLUSER="sqladminuser"
SQLPASSWORD="XXXXXXXXX"
sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/aggregation/sql/create_db.sql
```

7. 现在，创建将在 Azure Synapse serverless 数据库中创建对象的存储过程。确保你根据上一步获取的 serverless SQL 端点编辑 SQLPASSWORD 和 SERVERLESS_SQL_ENDPOINT：

```
SERVERLESS_SQL_ENDPOINT="trainingsynapse100-ondemand.sql.azuresynapse.net"
SQLUSER="sqladminuser"
SQLPASSWORD="XXXXXXXXX"
```

```
sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/aggregation/sql/create_synapse_creds.sql
```

```
sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/aggregation/sql/create_silver_views.sql
```

```
sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/aggregation/sql/drop_gold_tables.sql
```

```
sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/aggregation/sql/create_gold_tables.sql
```

这会产生以下输出：

```
Azure> SERVERLESS_SQL_ENDPOINT="trainingsynapse100-ondemand.sql.azuresynapse.net"
Azure> SQLUSER="sqladminuser"
Azure> SQLPASSWORD="      "
Azure> sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engine
create_synapse_creds.sql
Changed database context to 'gold'.
Azure> sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engine
create_silver_views.sql
Changed database context to 'gold'.
Azure> sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engine
drop_gold_tables.sql
Changed database context to 'gold'.
Azure> sqlcmd -S $SERVERLESS_SQL_ENDPOINT -U $SQLUSER -P $SQLPASSWORD -i Data-Engine
create_gold_tables.sql
Changed database context to 'gold'.
Azure>
```

图 8.9 – Azure Synapse 数据库中存储过程创建命令的输出

- 8. 我们还需要一个 Azure Data Factory 来完成本练习。你可能还记得我们在第 5 章《数据收集阶段 – Bronze 层》中创建了 traininglakehousedf。希望它仍然存在于你的 Azure 订阅中。如果你不小心删除了它，那么你需要回到“构建批处理摄取管道”部分并按照那里的步骤操作。

此时，你已准备好开始开发 Electroniz 聚合管道。

#### 为 gold 层创建管道

Azure 资源部署完成后，我们可以创建用于构建 gold 层的管道：

- 1. 让我们首先生成一个 Azure 存储账户 SAS 密钥，该密钥将由 Azure Synapse 用于访问数据文件。

使用 Azure 门户，导航至 Home > All Resources > traininglakehouse。

使用左侧菜单，点击 Shared access signature。输入以下选项：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_271_0.png)

图 8.10 – Azure 存储账户 SAS 令牌的创建选项

点击 **Generate SAS and connection string。**

请注意 **SAS token** 属性。你将在开始创建 gold 管道时需要此令牌：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_272_0.png)

图 8.11 – 用于身份验证的 SAS 令牌

在开始创建聚合管道之前，让我们了解 **共享访问签名 (SAS)** 用途。SAS 是一个 **URI**，它授予对 Azure Data Lake Storage 的受限访问。SAS 可以配置为在特定时间内授予具有非常有限权限的存储访问。

下图描述了 SAS URI 的结构。请注意每个选项如何映射到 URI 以限制使用它的用户或应用程序的访问：

##### SAS 令牌的结构

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_272_1.png)

https://datafencestorage.blob.core.windows.net/?sv=2019-12-12&ss=b&srt=sc&sp=rx&se=2021-02-12T04:00:00Z&st=2021-02-12T03:00:00Z&spr=https&sig=yJXqt8mIsvztZBbS%2FNDDyXbPK7gxIrebXWdWGF%2BBS0cmY%3D

图 8.12 – 用于身份验证的示例 SAS 令牌的结构

> **重要**
> SAS 密钥提供对 Azure 存储账户的委派和有限访问。SAS 密钥选项应在生产环境中实施更严格的控制和有限访问。





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





15. 现在，将 **Create Synapse Credentials** 链接到 **Create Silver Layer Views**。完成此操作后，管道应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_280_0.png)

图 8.21 – 链接活动 – 将 Create Synapse Credentials 链接到 Create Silver Layer Views

16. 在 **Activities** 面板中，搜索 Delete。现在，将 **Delete** 活动拖到右侧面板并提供以下属性：

- **Name:** `Cleanup Gold Data.`
- **Click on Source.** 现在，点击 **New**。
- **Choose Azure Data Lake Storage Gen 2** 并点击 **Continue**。
- **Name:** `gold_data.`
- **Linked service:** `traininglakehouse.`
- **File path:** `gold/external:`

## Set properties

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_280_1.png)

图 8.22 – Delete 活动属性

17. 点击 **OK**。点击 **Logging** 设置并取消选中 **Enable Logging** 复选框。

18. 现在，将 **Create Silver Layer Views** 链接到 **Cleanup Gold Data**。完成此操作后，管道应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_281_0.png)

图 8.23 – 链接活动 – 将 Create Silver Layer Views 链接到 Cleanup Gold Data

19. 在 **Activities** 面板中，搜索 **Stored Procedure**。现在，将 **Stored procedure** 活动拖到右侧面板并提供以下属性：

- **Name**: Create Gold Layer Tables.
- 点击 **Settings**。
- **Linked service**: Synapse Serverless.
- **Stored procedure name**: [dbo].[create_gold_tables].
- 在 **Stored procedure parameters** 下：添加三个参数。首先点击 **New**：
  - **Name**: goldns  **Type**: String
    **Value**: @pipeline().parameters.GOLD_LAYER_NAMESPACE
  - **Name**: format  **Type**: String
    **Value**: @pipeline().parameters.EXT_TAB_FILE_FORMAT
  - **Name:** extds  **Type:** String
    **Value:** @pipeline().parameters.EXT_DATA_SOURCE:

Linked service *  Synapse Serverless  Test connection  Edit  New

Stored procedure name * [dbo].[create_gold_tables]  Edit

## Stored procedure parameters

Import  New  Delete

| Name | Type | Value |
|---|---|---|
| goldns |  | @pipeline().parameters.GOLD_LAYER... |
| format |  | @pipeline().parameters.EXT_TAB_FILE... |
| extds |  | @pipeline().parameters.EXT_DATA_SO... |

图 8.24 – create_gold_tables 存储过程的参数

20. 现在，将 Cleanup Gold Data 链接到 Create Gold Layer Tables。此时，我们已完成 electroniz_aggregation_pipeline 的开发。点击 Publish all，然后点击 Publish。

electroniz_aggregation_pipeline 的最终版本应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_282_0.png)

图 8.25 – 最终的 electroniz_aggregation_pipeline

此时，我们已准备好对 electroniz_aggregation_pipeline 进行单元测试。

#### 运行聚合管道

在生产环境中，聚合管道将在 curation pipeline 成功完成后自动调用。由于我们处于单元测试阶段，目前我们将手动触发它。让我们开始：

- 1. 你必须在 Azure Cloud Shell 上执行以下命令。它们将根据我们传递的参数调用 **electroniz_aggregation_pipeline**。请确保根据提供的说明编辑 **EXT_TAB_LOCATION**。使用 Azure 门户，导航到你的 Azure Data Lake Storage 账户，路径为 **Home > All Resources > traininglakehouse > Endpoints.**
  
  注意 **Blob Service** 的 URL 并用此编辑 EXT_TAB_LOCATION：

```
RESOURCEGROUPNAME="training_rg"
DATAFACTORYNAME="traininglakehousedf"
PIPELINENAME="electroniz_batch_aggregation_pipeline"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME \
--parameters "{\"EXT_TAB_FILE_FORMAT\":\"parquetformat\", \"EXT_DATA_SOURCE\":\"trainingds\", \"EXT_CREDENTIALS\":\"trainingcreds\", \"EXT_TAB_LOCATION\":\"https://traininglakehouse.blob.core.windows.net\", \"SILVER_LAYER_NAMESPACE\":\"silver\", \"GOLD_LAYER_NAMESPACE\":\"gold\" }"
```

- 2. 要验证管道是否成功运行，你可以检查管道的状态：

  - 使用 Azure 门户，导航到 **Home > All Resources > trainingdatafactorydf.**
  - 点击 **Monitor > Pipeline Runs.**
  - 点击 **electroniz_batch_aggregation_pipeline**：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_284_0.png)

图 8.26 – 监控 electroniz_aggregation_pipeline 的运行

正如你所见，我们可以安全地推断 **electroniz_batch_aggregation_pipeline** 已成功完成。然而，这并不意味着它完成了预期的工作。为了确定，我们需要验证 Azure Synapse 中的聚合数据。

#### 了解数据消费

在开始验证聚合数据之前，我们应关注我们的终端用户将如何消费数据用于仪表板、ML 和 AI 目的。根据 **Electroniz** lakehouse 的架构，我们决定发布来自 gold 和 silver 层的数据。

发布 gold 层的数据是必要的；否则，用户如何访问聚合数据？但为什么我们需要发布 silver 层的数据？你猜对了 – 分析是一个持续的过程。未来，用户可能希望为公司改进创建新的仪表板和 ML 模型。

> **Important**
> 发布 gold 和 silver 层的数据是可以接受的，因为它们存储的数据处于干净且安全的状态。但对于 bronze 层的数据则不能这样说。发布原始/未清理的数据不仅会给终端用户带来大量关于标准化、验证和去重的工作，而且还会暴露本应受到保护的分类信息。

下图显示了 lakehouse 的结构：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_285_0.png)

图 8.27 – Electroniz 为终端用户的数据发布点

我们对 Electroniz 的承诺是，到项目结束时，终端用户不仅能够访问 gold 层中的聚合数据，还能访问 silver 层中的 curated 数据以便临时访问。此外，未来会有新的聚合和 ML 模型需求，这将需要访问 curated 数据。

你可能已经注意到，在创建 electroniz_batch_aggregation_pipeline 时，管道中定义了一系列活动。这些活动最终会在 Azure Synapse 中创建视图和外部表。了解 lakehouse 中的数据如何为终端用户发布非常重要。提供了以下数据：

- Access to Silver Layer Data: 为了访问 curated 数据，我们在 Azure Synapse serverless SQL pool 中提供了 EXTERNAL TABLES 和 VIEWS。如前所述，并非所有聚合都能在项目初期预测。访问 silver 层中的数据将使终端用户能够执行临时查询并在未来创建新的聚合。

- **Access to Gold Layer Data:** 我们通过在 **Azure Synapse serverless SQL pool** 中使用 **EXTERNAL TABLES** 为 gold 层的聚合表提供了访问权限。这些外部表是使用 **CREATE EXTERNAL TABLE AS SELECT (CETAS)** 创建的，并且查询结果被保存（持久化）在 **Azure Data Lake Storage Gen2** 中的 gold 层。由于聚合数据保存在 gold 层，这大大提高了查询处理时间：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_286_0.png)

上图显示了在 **electroniz_batch_aggregation_pipeline** 中外部视图和表是如何创建的。

#### 访问 silver 层数据

**Create Silver Layer Views** 调用 Azure Synapse 中名为 **create_silver_views** 的存储过程。此存储过程的代码可在以下位置获取：

```
https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/blob/main/project/aggregation/sql/create_silver_views.sql.
```





查看以下 SQL。你会注意到外部表和视图的组合是如何工作的：

```
EXEC sp_executesql N'IF EXISTS ( SELECT * FROM sys.external_tables WHERE object_id = OBJECT_ID(''store_customers'') )
SET @sqlcmd = 'CREATE EXTERNAL TABLE store_customers'([customer_id] INTEGER ,[customer_name] VARCHAR(255) ,[address] VARCHAR(255) ,
[city] VARCHAR(255) ,[postalcode] VARCHAR(10) ,[country] VARCHAR(100) ,
[phone] VARCHAR(100) ,[email] VARCHAR(255) ,[credit_card] VARCHAR(255) ,
[updated_at] DATETIME)
WITH (LOCATION = '' + @silverns + ''/sales/store_customers'', data_source = trainingds, FILE_FORMAT = DeltaLakeFormat)';
EXEC sp_executesql @sqlcmd

EXEC sp_executesql N'IF EXISTS ( SELECT * FROM sys.external_tables WHERE object_id = OBJECT_ID(''ecomm_customers'') )
SET @sqlcmd = 'CREATE EXTERNAL TABLE ecomm_customers'([customer_id] INTEGER ,[customer_name] VARCHAR(255) ,[address] VARCHAR(255) ,
[city] VARCHAR(255) ,[postalcode] VARCHAR(10) ,[country] VARCHAR(100) ,
[phone] VARCHAR(100) ,[email] VARCHAR(255) ,[updated_at] DATETIME )
WITH (LOCATION = '' + @silverns + ''/esalesns'', data_source = trainingds, FILE_FORMAT = DeltaLakeFormat)';
EXEC sp_executesql @sqlcmd

EXEC sp_executesql N'DROP VIEW IF EXISTS customers'
SET @sqlcmd = 'CREATE OR ALTER VIEW customers'
SELECT [customer_id],[customer_name] ,[address],[city],[postalcode],
[country],[phone],[email],NULL as credit_card,[updated_at]
FROM [dbo].[ecomm_customers]
UNION
SELECT [customer_id],[customer_name],[address],[city],[postalcode],
[country],[phone],[email],[credit_card],[updated_at]
FROM [dbo].[store_customers]'
EXEC sp_executesql @sqlcmd
```

图 8.29 – 湖仓银层中的外部表和视图

store_customers（商店数据库中的客户）是声明在 traininglakehouse Azure Data Lake Storage 账户银层的 /sales/store_customers 目录中的外部表。由于银层使用 Delta 格式，FILE_FORMAT 已设置为 DeltaLakeFormat。

ecomm_customers（在线购买产品的客户）是声明在 traininglakehouse Azure Data Lake Storage 账户银层的 /esalesns 目录中的外部表。由于银层使用 Delta 格式，FILE_FORMAT 已设置为 DeltaLakeFormat。

注意我们如何巧妙地将两种类型的客户组合在一个名为 customers 的公共视图中。这为我们的终端用户提供了灵活性，可以分别查询商店与电子商务的客户，或使用一个通用的聚合版本。

#### 访问金层数据

Create Gold Layer Tables 调用了 Azure Synapse 中名为 create_gold_tables 的存储过程。此存储过程的代码可在以下位置获得：

```
https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/blob/main/project/aggregation/sql/create_gold_tables.sql
```

查看以下 SQL。你会注意到外部表是如何使用 CTAS 创建的：

```
EXEC sp_executesql N'IF EXISTS ( SELECT * FROM sys.external_tables WHERE object_id = OBJECT_ID(''ext_aggregated_sales'') ) DROP EXTERNAL TABLE ext_aggregated_sales;
SET @sqlcmd = N'CREATE EXTERNAL TABLE ext_aggregated_sales WITH (
    LOCATION = '' + @goldns + ''/external/ext_aggregated_sales/'',
    DATA_SOURCE = '' + @extds + '',
    FILE_FORMAT = '' + @format + ''
) AS SELECT YEAR(order_date) AS year, DATEPART(QUARTER, order_date) as quarter, round(sum(sale_price_usd),2) as aggregated_sales_price FROM orders GROUP BY YEAR(order_date), DATEPART(QUARTER, order_date) '
EXEC sp_executesql @sqlcmd
```

图 8.30 – 湖仓金层中的外部表

注意 ext_aggregated_sales 是如何使用 CTAS 创建的。该查询从 orders 视图中按季度获取聚合数据，结果以 Parquet 格式持久化在金层中。

> **重要**
> 拥有一个可靠的数据消费策略对于数据湖项目的整体成功至关重要。毕竟，这是终端用户在其数据使用场景中进入的入口点。

现在我们对终端用户使用的数据消费模式有了充分的了解，我们可以验证实际数据。

#### 验证金层中的聚合数据

假设之前的 electroniz_batch_aggregation_pipeline 调用是成功的，你应该在 Azure Data Lake Storage 账户的 silver 容器中看到以下外部表和视图：

- 1. 使用 Azure 门户，导航到 Home > All Resources > trainingsynapse100。

现在，点击 Open in the Open Synapse Studio 部分：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_289_0.png)

图 8.31 – Open Synapse Studio 部分

2. 使用左侧菜单，点击 Data。现在，继续点击 Database 旁边的箭头，然后 gold (SQL) 和 External Tables。

你现在应该看到以下窗格：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_289_1.png)

图 8.32 – Synapse 无服务器 SQL 池中的银层外部表

3. 同样，以下外部表和视图代表了湖仓金层中的聚合数据：

图 8.33 – Synapse 无服务器 SQL 池中的金层外部表和视图

4. 让我们更进一步，从一个视图中获取数据。要查询视图，点击视图名称旁边的三点线，然后 New SQL script，然后 Select TOP 100 rows：

图 8.34 – 在 Synapse 无服务器 SQL 池中查询视图

现在，点击 Run：

```
SELECT TOP (100) [customer_id]
,[customer_name]
,[address]
,[city]
,[postalcode]
,[country]
,[phone]
,[email]
,[credit_card]
,[updated_at]
FROM [dbo].[customers]
```

| customer_id | customer_name | address | city | postalcode | country | phone | email | credit_card |
| (NULL) | Ignacia Price | d37d... | Lerwick | JO 3WK | UK | c6b6... | non@facilisivit... | (NULL) |
| (NULL) | Linda Pugh | 31271... | Gloucester | P1P 6BS | CAN | a6a87... | tincidunt@temp... | (NULL) |
| 7 | Tariq Singh | 6cce... | Silchar | 968978 | IND | fae9... | luctus.ut@euis... | b5159e... |
| 34 | Keelie Cardenas | 4ec6... | Leighton Buzzard | AU4L 7DX | UK | 00687... | duinec.tempus... | d5668... |
| 63 | Nithik Singh | 01035... | Shimla | 295873 | IND | e03d... | vivera.donec... | 65974... |
| 67 | Praval Jain | 09750... | Agartala | 713911 | IND | d1d5... | velit.eget@item... | 0645c... |
| 90 | Anjolie Mueller | 07d00... | Dieppe | JA4 855 | CAN | 5a57... | non.arcu.vivam... | 7566d... |
| 120 | Damian Reese | 82030... | Newark | B2M 3CX | UK | 902e... | iaculis@malesu... | 1aa6a... |
| 121 | Leila Tate | 4643e... | Barrow-in-Furn... | D1 6DG | UK | e57a... | elit.erat@apuru... | 85a7a... |

图 8.35 – Synapse 无服务器 SQL 池中 customers 视图的数据

你应该看到 customers 视图的数据显示。你可能会注意到 orders 视图从两个名为 store_customers 和 ecomm_customers 的外部表聚合数据。

- 5. 让我们再做一次，但这次我们将选择一个使用 CTAS 创建的外部表。使用左侧菜单，点击 External Tables。

要查询视图，点击视图名称旁边的三点线，然后 New SQL script，然后 Select TOP 100 rows：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_291_0.png)

图 8.36 – 在 Synapse 无服务器 SQL 池中查询外部表

现在，点击 Run：

```
SELECT TOP (100) [product_category]
,[product_name]
,[year]
,[quarter]
,[units_sold]
FROM [dbo].[ext_aggregated_products_by_quarter]
```

Results
Messages
View
Table
Chart
Export results
Search

| product_category | product_name | year | quarter | units_sold |
|---|---|---|---|---|
| HomeAppliance | Microwave | 2016 | 1 | 14 |
| HomeAppliance | Microwave | 2016 | 2 | 11 |
| HomeAppliance | Microwave | 2016 | 3 | 18 |
| HomeAppliance | Microwave | 2016 | 4 | 12 |
| HomeAppliance | Microwave | 2017 | 1 | 10 |
| HomeAppliance | Microwave | 2017 | 2 | 16 |

图 8.37 – ext_aggregated_products_by_quarter 的数据

- 6. 如你可能已注意到的，从 Azure Synapse 获取 ext_aggregated_products_by_quarter 的性能比获取 customers 视图快得多。原因在于 ext_aggregated_products_by_quarter 的数据已持久化在湖仓金层的以下目录中：



![](img/dae8a2243a3c3cbcd9c56ff3d2217672_293_0.png)

图 8.38 – 使用 CTAS 创建的聚合表的持久化数据

你可以自行查询更多外部表和视图以进一步增强信心。此时，我们可以推断 Azure Synapse 中的数据已准备好供终端用户查询。但仍有一个问题未得到解答：湖屋中的数据是否满足 Electroniz 终端用户的需求？我们是否满足了客户的期望？我们即将揭晓！

#### 满足客户期望

根据前一节所示的结果，我们可以安全地推断，我们可以成功地从 Electroniz 湖屋的银层和金层查询数据。这无疑是一个巨大的技术胜利；然而，真正的问题是从业务角度来看，我们是否满足了客户的期望。

在深入验证业务需求之前，我们需要确定是否已为终端用户，例如数据分析师、数据科学家和商业智能工程师提供了足够的访问权限。最终，终端用户将使用湖屋来满足其多样化的分析需求，因此应妥善解决他们的需求。

让我们重点验证项目开始时 **Electroniz** 提出的业务需求：

- 1. 首先，Electroniz 希望建立一个近实时销售分析平台，能够更快、更好地满足其客户需求。

你可能还记得 **Electroniz** 既在店内销售产品，也在线销售。在构建青铜层时，我们从两个来源摄取了销售数据。为了提供包含 **店内** 和 **在线** 销售数据的近实时分析，我们将销售数据聚合到一个名为 **orders** 的公共视图中。该视图是在 **create_silver_views** 存储过程中创建的，该过程通过 **electroniz_aggregation_pipeline** 中的 **Create Silver Layer Views** 活动调用：

```
EXEC sp_executesql N'DROP VIEW IF EXISTS store_orders'
SET @sqlcmd = N'CREATE OR ALTER VIEW store_orders AS
SELECT * FROM openrowset(
BULK '" + @silverns + '/sales/store_orders/"', data_source = '"trainingds"', FORMAT = '"delta"')
AS rows'
EXEC sp_executesql @sqlcmd

EXEC sp_executesql N'DROP VIEW IF EXISTS ecomm_orders'
SET @sqlcmd = N'CREATE OR ALTER VIEW ecomm_orders AS
SELECT order_number, ...
FROM openrowset(
BULK '" + @silverns + '/sales/store_orders/"', data_source = '"trainingds"', FORMAT = '"delta"')
WITH ( order_number INT, product_name NVARCHAR(MAX), order_date DATE, order_mode NVARCHAR(MAX), email NVARCHAR(MAX), sale_price FLOAT, sale_price_usd FLOAT, updated_at DATETIME )
) AS rows'
EXEC sp_executesql @sqlcmd

EXEC sp_executesql N'DROP VIEW IF EXISTS orders'
SET @sqlcmd = N'CREATE OR ALTER VIEW orders AS
SELECT order_number, product_name, order_date, units, sale_price, order_mode, sale_price_usd, dbo.store_orders.updated_at
FROM dbo.store_orders
JOIN dbo.customers ON dbo.customers.customer_id = dbo.store_orders.customer_id
JOIN dbo.products ON dbo.products.product_id = dbo.store_orders.product_id
UNION ALL
SELECT order_number, email, product_name, order_date, 1 AS units, sale_price, order_mode, sale_price_usd, updated_at
FROM dbo.ecomm_orders'
EXEC sp_executesql @sqlcmd
```

图 8.39 – 创建 orders 视图的 SQL

同一个视图还用于按季度聚合销售数据并发布为 **ext_aggregated_sales**。该表是在 **create_gold_tables** 存储过程中创建的，该过程通过 **electroniz_aggregation_pipeline** 中的 **Create Gold Layer Tables** 活动调用：

```
SET @sqlcmd = N'CREATE EXTERNAL TABLE ext_aggregated_sales
WITH (
LOCATION = '" + @goldns + '/external/ext_aggregated_sales/"',
DATA_SOURCE = '" + @extds + '"',
FILE_FORMAT = '" + @format + '"'
)
AS SELECT YEAR(order_date) AS year, DATEPART(QUARTER, order_date) as quarter,
round(sum(sale_price_usd),2) as aggregated_sales_price
FROM orders
GROUP BY YEAR(order_date), DATEPART(QUARTER, order_date) '
EXEC sp_executesql @sqlcmd
```

图 8.40 – ext_aggregated_sales 外部表的 SQL

以下来自 ext_aggregated_sales 的查询将显示按季度汇总的销售数据：

```
SELECT [year], [quarter], [aggregated_sales_price]
FROM [dbo].[ext_aggregated_sales]
ORDER BY 1 DESC, 2 ASC
```

这将产生以下输出：

```
1  SELECT [year], [quarter], [aggregated_sales_price]
2  FROM [dbo].[ext_aggregated_sales]
3  ORDER by 1 DESC, 2 ASC
```

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_295_0.png)

图 8.41 – 按季度汇总的销售额

- 2. 让我们开心一下，将其可视化为条形图。点击 **Chart** 并根据以下截图更改变量。正如你所见，**Electroniz** 的终端用户现在可以轻松按年/季度可视化汇总销售额，以近实时增强其决策能力：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_296_0.png)

图 8.42 – 按季度销售额图表

- 3. **Electroniz** 还希望通过基于其电子商务网站用户点击次数的智能分析来简化广告预算。他们希望 **广告** 和 **营销** 资金能够按国家/地区分配，并以其电子商务网站收到的用户点击次数为依据。

我们为此业务需求创建了几个外部表。第一个外部表名为 ext_total_website_hits，它按国家/地区汇总点击次数，其中至少收到 1,000 次点击。Electroniz 已决定，少于 1,000 次点击的数字不足以影响其决策：

> 重要
> 像这样来自客户的输入对于项目的整体成功至关重要。这就是为什么我之前提到聚合过程是客户和数据工程师共同的责任。

```
SET @sqlcmd = N'CREATE EXTERNAL TABLE ext_total_website_hits
WITH (
    LOCATION = ''" + @golds + '/external/ext_total_website_hits/''',
    DATA_SOURCE = ' + @extds + ',
    FILE_FORMAT = ' + @format + '
)
AS
SELECT SUM(hits) AS total_hits FROM
(SELECT country_name, count(*) AS hits
FROM logs
GROUP BY country_name
HAVING count(*) > 1000) as webhits'
EXEC sp_executesql @sqlcmd

SET @sqlcmd = N'CREATE EXTERNAL TABLE ext_advertising_budgets
WITH (
    LOCATION = ''" + @golds + '/external/ext_advertising_budgets/''',
    DATA_SOURCE = ' + @extds + ',
    FILE_FORMAT = ' + @format + '
)
AS SELECT country_name, hits, (hits*100/total_hits) AS advtg_budget
FROM
(SELECT country_name, count(*) as hits, (SELECT total_hits from ext_total_website_hits) AS total_hits
FROM logs
GROUP BY country_name
HAVING count(*) > 1000) AS agg_hits '
```

图 8.43 – 聚合广告预算的 SQL

第二个外部表名为 ext_advertising_budgets，它根据日志视图按国家/地区汇总网站点击次数。这些外部表是在 create_gold_tables 存储过程中创建的，该过程通过 electroniz_aggregation_pipeline 中的 Create Gold Layer Tables 活动调用：

```
SELECT TOP (100) [country_name]
, [hits]
, [advtg_budget]
FROM [dbo].[ext_advertising_budgets]
ORDER BY 3 DESC
```

未来，终端用户可以使用上述查询轻松分配基于用户点击次数支持的广告和营销预算百分比：

```
SELECT TOP (100) [country_name] ,[hits] ,[advtg_budget]
FROM [dbo].[ext_advertising_budgets]
ORDER BY 3 DESC
```

| country_name | hits | advtg_budget |
|---|---|---|
| United States of America | 24690 | 57 |
| China | 6936 | 16 |
| Japan | 2272 | 5 |
| Germany | 2323 | 5 |
| United Kingdom of Great Britain ... | 2029 | 4 |
| Korea (Republic of) | 1829 | 4 |
| France | 1585 | 3 |
| Italy | 1063 | 2 |

图 8.44 – 聚合网站点击次数和预算分配

点击 Chart 并根据以下截图更改提供的变量。通过这样做，Electroniz 的终端用户可以轻松可视化广告和营销资金的按国家/地区分配：



```
SELECT TOP (100) [country_name] ,[hits] ,[advtg_budget]
FROM [dbo].[ext_advertising_budgets]
```

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_299_0.png)

Figure 8.45 – Chart of advertising budget allocation per country

4. Electroniz 提出的最终核心需求是减少因库存短缺导致订单履行中断而产生的客户不满。他们希望能够近乎实时地查看所售产品，以便清楚了解库存下降情况并适当地补货。

为满足该需求，我们创建了一个名为 ext_aggregated_products_by_quarter 的聚合外部表。该表报告每季度售出的产品数量。这些信息为决策者提供了在任何给定时间库存耗尽程度的清晰了解，以便他们能够就未来的采购和补货采取适当行动。该表是在 create_gold_tables 存储过程中创建的，该存储过程通过 electroniz_aggregation_pipeline 中的 Create Gold Layer Tables 活动调用。

Electroniz 的终端用户可以调用以下查询来显示其库存水平的当前状态：

```
SELECT TOP (100) [product_category]
, [product_name]
, [year]
, [quarter]
, [units_sold]
FROM [dbo].[ext_aggregated_products_by_quarter]
WHERE year=2021
ORDER BY 3 DESC, 2 ASC
```

这将产生以下输出：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_300_0.png)

Figure 8.46 – Aggregated products sold in the recent quarter

点击 **Chart** 并根据以下屏幕截图更改提供的变量。**Electroniz** 的终端用户可以轻松可视化每种产品的所需库存：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_301_0.png)

Figure 8.47 – Chart of products sold in the recent quarter

我认为我们的工作做得非常出色；你可以庆祝了，数据工程师先生！前述的报告和可视化足以证明我们在满足客户期望方面做得很好。这些结果应以现场演示的形式与客户管理团队共享。我们当然可以庆祝这一胜利，但我们的工作尚未完成。

现在，我们需要确保摄取、治理和聚合管道作为一个整体单元工作。这意味着我们现在将关注基础设施和代码部署。

## Summary

在本章中，我们学习了如何构建湖屋的金层，该层存储聚合数据。我们还了解到数据聚合是客户与数据工程师之间的协作工作，双方各自提供对项目整体成功至关重要的一套专业技能。本章末尾，我们尝试验证我们是否成功解决了为 **Electroniz** 构建湖屋的业务需求。

在 *Chapter 9, Deploying and Monitoring Pipelines in Production* 中，我们将依次部署本章中创建的三个管道，即 **electroniz_ingestion_pipeline**、**electroniz_curation_pipeline** 和 **electroniz_aggregation_pipeline**。这将确保在每次调用时，它们都能作为一个整体单元工作，以实现整体目标。

## Section 3:
Data Engineering
Challenges and Effective
Deployment Strategies

本书的这一部分向您介绍数据工程专业人员面临的挑战。介绍了 Delta Lake 的概念。最后，将通过真实案例突出一系列数据工程挑战，并说明 Delta Lake 如何有效应对这些问题。

本节还向您介绍数据湖的有效部署策略。一旦构建了处理数据工程各个阶段的各个代码组件，这些策略在按可重复和持续的方式配置云资源并部署数据管道方面发挥着重要作用。

本节包含以下章节：

+   - Chapter 9, Deploying and Monitoring Pipelines in Production
+   - Chapter 10, Solving Data Engineering Challenges
+   - Chapter 11, Infrastructure Provisioning
+   - Chapter 12, Continuous Integration and Deployment (CI/CD) of Data Pipelines

### 9
Deploying and Monitoring Pipelines in Production

数据的旅程即将接近目的地。在上一章中，我们学习了数据聚合以及如何构建湖屋的金层。金层中的数据是汇总数据的表示，每次摄取新数据时都会发生变化。因此，这是一个循环过程。现在我们已经构建并测试了 Electroniz 湖屋所需的所有组件，我们需要开始考虑在生产环境中部署管道，以便以最及时和持久的方式满足终端用户的需求。

在本章中，我们将涵盖以下主题：

+   - The deployment strategy
+   - Developing the master pipeline
+   - Testing the master pipeline
+   - Scheduling the master pipeline
+   - Monitoring pipelines

#### The deployment strategy

我们付出了极大的努力，才达到了拥有三个功能管道的地步，如下所示：

+   - **The Electroniz ingestion pipeline:** `electroniz_batch_ingestion_pipeline`
+   - **The Electroniz curation pipeline:** `electroniz_curation_pipeline`
+   - **The Electroniz aggregation pipeline:** `electroniz_aggregation_pipeline`

作为回顾，在最近几章中，我们遵循了多个步骤来创建这些管道。创建后，我们手动调用每个管道以进行单元测试其功能。最后，我们验证了每个管道产生的数据，以确保其符合 **Electroniz** 用例的预期。这是一项大量工作，因此我们应该为取得如此大的进展而感到自豪。

假设我们对前述管道执行的单元测试结果感到满意，现在是时候开始思考在生产环境中部署这些管道的最佳方式了。根据最佳实践，三个管道应作为一个完整单元运行。每次管道运行，也称为作业，应通过调用摄取管道从各种源提取增量数据来工作。随后调用治理管道，对数据进行清洗并合并到 **Delta Lake** 表中。最后，聚合管道对数据进行汇总以供终端用户使用。

通常，管道以以下两种方式部署，如下所述。

#### The time-based method

在 **time-based method** 中，每个管道单独部署并根据时间驱动方式调用。例如，您可以在凌晨 12:00 调用摄取管道，在凌晨 12:20 调用治理管道，在凌晨 12:30 调用聚合管道。以下是该方法的一些优缺点列表：

+   - **The overall deployment is simple:** 您只需在指定时间安排每个管道。
+   - **It requires time precision:** 每个管道的调用时间需要仔细计算；否则，它们可能会相互重叠。您不希望第二个管道在第一个管道完成之前就开始。

+   - **There is less control:** 总体而言，对管道成功的控制较少，因为每个管道单独运行，彼此之间没有任何依赖关系：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_307_0.png)

Figure 9.1 – The time-based method of deploying pipelines

#### The event-based approach

在 **event-based method** 中，创建一个主管道，根据成功和失败事件运行所有其他管道。例如，首先调用摄取管道。然后，在成功后调用治理管道。然后，在成功后调用聚合管道。以下是该方法的一些优缺点列表：

+   - **It requires a master pipeline:** 在此方法中，需要一个主管道来编排所有其他管道的运行。

+   - **Time precision is less of a concern:** 在此方法中，只有主管道安排其他管道。一旦被调用，主管道以受控的方式控制所有其他管道的运行，该方式由事件驱动。





- **增强控制**：与基于时间的方法相比，此方法提供更多控制。后继管道要等到前置管道完成才开始：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_308_0.png)

图 9.2 – 基于事件的管道部署方法

基于事件的方法更可取，因为它能更好地控制管道最终的成功。出于前面所述的原因，让我们使用这种方法来部署 Electroniz 管道。在下一节中，我们将开始创建主管道，该管道将调用其余管道。

##### 开发主管道

要开始创建主管道，请确保 `electroniz_batch_ingestion_pipeline`、`electroniz_curation_pipeline` 和 `electroniz_aggregation_pipeline` 管道已安装并在 Azure 数据工厂中正常运行：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_309_0.png)

图 9.3 – Electroniz 摄取、整理和聚合管道

我们将通过执行以下步骤开始：

- 1. 使用 Azure 门户，导航到 Home > All Resources > trainingdatafactory100。单击 Open Azure Data Factory Studio 下方的 Open。
- 2. 使用左侧面板，从菜单中单击 Author。在 Factory Resources 面板中，单击 Pipelines 右侧的三个点并选择 New Pipeline。

在 Properties 面板中输入以下内容：

- - Name: electroniz_master_pipeline
- - Description: This pipeline runs the ingestion, curation, and aggregation pipeline.

使用底部面板，单击 Parameters 并添加以下参数：

- - Name: STORAGE_ACCOUNT Type: String
- - Name: BRONZE_LAYER_NAMESPACE Type: String
- - Name: STORE_SALES_FOLDER Type: String
- - Name: TABLE_LIST Type: String
- - Name: CURRENCY_LIST Type: String
- - Name: CURRENCY_FOLDER Type: String
- - Name: GEOLOCATION_FOLDER Type: String
- - Name: LOGS_FOLDER Type: String
- - Name: ECOMM_FOLDER Type: String
- - Name: EXT_TAB_FILE_FORMAT Type: String

- - Name: EXT_DATA_SOURCE  Type: String
- - Name: EXT_CREDENTIALS  Type: String
- - Name: EXT_TAB_LOCATION  Type: String
- - Name: SILVER_LAYER_NAMESPACE  Type: String
- - Name: GOLD_LAYER_NAMESPACE  Type: String:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_310_0.png)

图 9.4 – 主管道参数

3. 现在，单击 Activities 部分中的空白区域。在 Activities 面板中搜索 Execute。现在，将 Execute Pipeline 活动拖到右侧面板：

- - Name: Run electroniz_batch_ingestion_pipeline.
- - Click on Settings.
- - For Invoked Pipeline, choose electroniz_batch_ingestion_pipeline from the drop-down menu.

- - Wait on completion: Checked:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_311_0.png)

图 9.5 – 配置 Run electroniz_batch_ingestion_pipeline

4. 在 Activities 面板中搜索 Execute。现在，将 Execute Pipeline 活动拖到右侧面板：

- - Name: Run electroniz_curation_pipeline

- - Click on Settings.

- - For Invoked Pipeline, choose electroniz_curation_pipeline from the drop-down menu.

- - Wait on completion: Checked

在 Parameters 部分，添加以下参数：

- - Name: STORAGE_ACCOUNT Type: String
  Value: @pipeline().parameters.STORAGE_ACCOUNT

- - Name: BRONZE_LAYER_NAMESPACE Type: String
  Value: @pipeline().parameters.BRONZE_LAYER_NAMESPACE

- - Name: SILVER_LAYER_NAMESPACE Type: String
  Value: @pipeline().parameters.SILVER_LAYER_NAMESPACE

- - Name: STORE_SALES_FOLDER Type: String
  Value: @pipeline().parameters.STORE_SALES_FOLDER

- - Name: ADLS_FOLDER Type: String
  Value: @{formatDateTime(utcnow(), 'yyyy')}/@{formatDateTime(utcnow(), 'MM')}/@

```
{formatDateTime(utcnow(),'dd')}/@
{formatDateTime(utcnow(),'HH')}
```

- - Name: TABLE_LIST  Type: String
  Value: @pipeline().parameters.TABLE_LIST
- - Name: CURRENCY_LIST  Type: String
  Value: @pipeline().parameters.CURRENCY_LIST
- - Name: CURRENCY_FOLDER  Type: String
  Value: @pipeline().parameters.CURRENCY_FOLDER
- - Name: GEOLOCATION_FOLDER  Type: String
  Value: @pipeline().parameters.GEOLOCATION_FOLDER
- - Name: LOGS_FOLDER  Type: String
  Value: @pipeline().parameters.LOGS_FOLDER
- - Name: ECOMM_FOLDER  Type: String
  Value: @pipeline().parameters.ECOMM_FOLDER:

| Parameters |  |  |  |
|---|---|---|---|
| Name | Type | Value | Default value |
| STORAGE_ACCOUNT | string | @pipeline().parameters.STORAGE_ACC... |  |
| BRONZE_LAYER_NAMESPACE | string | @pipeline().parameters.BRONZE_LAYE... |  |
| SILVER_LAYER_NAMESPACE | string | @pipeline().parameters.SILVER_LAYER... |  |
| STORE_SALES_FOLDER | string | @pipeline().parameters.STORE_SALES_... |  |
| ADLS_FOLDER | string | @{formatDateTime(utcnow(),'yyyy')}/... |  |
| TABLE_LIST | string | @pipeline().parameters.TABLE_LIST |  |
| CURRENCY_LIST | string | @pipeline().parameters.CURRENCY_LIST |  |
| CURRENCY_FOLDER | string | @pipeline().parameters.CURRENCY_FO.. |  |
| GEOLOCATION_FOLDER | string | @pipeline().parameters.GEOLOCATIO... |  |
| LOGS_FOLDER | string | @pipeline().parameters.LOGS_FOLDER |  |
| ECOMM_FOLDER | string | @pipeline().parameters.ECOMM_FOLD.. |  |

图 9.6 – 运行 electroniz_curation_pipeline 参数

5. 现在，将 Run electroniz_batch_ingestion_pipeline 链接到 Run electroniz_curation_pipeline。管道应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_313_0.png)

图 9.7 – 链接 Electroniz 摄取和整理管道

6. 在 Activities 面板中搜索 Execute。现在，将 Execute Pipeline 活动拖到右侧面板：

- - Name: Run electroniz_aggregation_pipeline.
- - Click on Settings.
- - From Invoked Pipeline, choose electroniz_aggregation_pipeline from the drop-down menu.
- - Wait on completion: Checked.

在 Parameters 部分，添加以下参数：

- - Name: EXT_TAB_FILE_FORMAT Type: String
  Value: @pipeline().parameters.EXT_TAB_FILE_FORMAT
- - Name: EXT_DATA_SOURCE Type: String
  Value: @pipeline().parameters. @pipeline().parameters.EXT_DATA_SOURCE
- - Name: EXT_CREDENTIALS Type: String
  Value: @pipeline().parameters.EXT_CREDENTIALS
- - Name: EXT_TAB_LOCATION Type: String
  Value: @pipeline().parameters.EXT_TAB_LOCATION
- - Name: SILVER_LAYER_NAMESPACE Type: String
  Value: @pipeline().parameters.SILVER_LAYER_NAMESPACE
- - Name: GOLD_LAYER_NAMESPACE Type: String
  Value: @pipeline().parameters.GOLD_LAYER_NAMESPACE:

###### Parameters

| Name | Type | Value |
|---|---|---|
| EXT_TAB_FILE_FORMAT | string | @pipeline().parameters.EXT_TAB_FILE_... |
| EXT_DATA_SOURCE | string | @pipeline().parameters.EXT_DATA_SO... |
| EXT_CREDENTIALS | string | @pipeline().parameters.EXT_CREDENTI... |
| EXT_TAB_LOCATION | string | @pipeline().parameters.EXT_TAB_LOCA... |
| SILVER_LAYER_NAMESPACE | string | @pipeline().parameters.SILVER_LAYER_... |
| GOLD_LAYER_NAMESPACE | string | @pipeline().parameters.GOLD_LAYER_... |

图 9.8 – 运行 electroniz_aggregation_pipeline 参数

7. 现在，将 Run electroniz_curation_pipeline 链接到 Run electroniz_aggregation_pipeline。

单击 Publish all，然后单击 Publish。

最终管道将如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_314_0.png)

图 9.9 – 将 Electroniz 摄取、整理和聚合管道链接在一起

electroniz_master_pipeline 管道的开发现已完成。接下来，我们将进行管道的单元测试。





## 测试主管道

现在主管道的开发已经完成，我们应该验证管道是否能够成功运行。让我们手动调用它并检查运行结果：

- 1. 连接到 Azure Cloud Shell 并执行以下命令：
```
DATAFACTORYNAME="trainingdatafactory100"
RESOURCEGROUPNAME="training_rg"
PIPELINENAME="electroniz_master_pipeline"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME \
--parameters "{\"STORAGE_ACCOUNT\":\"traininglakehouse\",\"BRONZE_LAYER_NAMESPACE\":\"bronze\", \
\"SILVER_LAYER_NAMESPACE\":\"silver\",\"STORE_SALES_FOLDER\":\"sales\", \
\"TABLE_LIST\":\"products,store_customers,store_orders\",\"CURRENCY_LIST\":\"CAD,INR,EUR\",\"CURRENCY_FOLDER\":\"currency\", \
\"GEOLOCATION_FOLDER\":\"geolocation\",\"LOGS_FOLDER\":\"logs\",\"ECOMM_FOLDER\":\"esalesns\/esaleshub\/*\/ *\/ *\/ *\/ *\/ *\/ *\", \
\"EXT_TAB_FILE_FORMAT\":\"parquetformat\", \"EXT_DATA_SOURCE\":\"trainingds\", \"EXT_CREDENTIALS\":\"trainingcreds\", \
\"EXT_TAB_LOCATION\":\"https://traininglakehouse.blob.core.windows.net\", \"GOLD_LAYER_NAMESPACE\":\"gold\" }"
```
- 2. 使用 Azure 门户，导航至 Home > All Resources > trainingdatafactory100。点击 Open 下方的 Open Azure Data Factory Studio。

现在，点击 Monitor 菜单，然后点击 Pipeline runs。注意主管道如何成功运行了另外三个管道：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_316_0.png)

图 9.10 – 主管道手动运行的状态

在之前的管道运行期间，Electroniz lakehouse 的所有层，即 bronze、silver 和 gold 都已填充。在一次运行中，最新数据从源中摄取，合并到 silver 层（Delta Lake），最后在 gold 层中进行聚合。

让我提醒你，在项目的发现阶段，Electroniz 曾表达希望在 lakehouse 中实现接近实时的数据摄取，最大延迟为 1 小时。这意味着我们应该按每小时的计划运行主管道。让我们接下来弄清楚如何实现这一点。

### 调度主管道

要在给定时间自动执行管道，需要在 Azure 数据工厂中创建触发器。让我们了解如何操作：

- 1. 使用左侧面板，点击 Manage。然后，点击 Triggers。最后，点击 New：
  - Name: electroniz_master_trigger
  - Start date: 选择日期和时间
  - Time Zone: 选择你的时区
  - Recurrence: 每 1 小时
  - Activated: Yes.

最后，点击 **OK**：

Name *
electroniz_master_trigger

Description

Type *
Schedule

Start date *
07/07/2021 6:00 PM

Time zone *
Eastern Time (US & Canada) (UTC-5)
This time zone observes daylight savings. Trigger will auto-adjust for one hour difference.

Recurrence *
Every 1 Hour(s)
Specify an end date

Annotations
+ New

OK
Cancel

图 9.11 – Electroniz 主管道的触发器

2. 使用左侧窗格，点击 **Author**。点击 **Pipeline** 下方的 **electroniz_master_pipeline**：

- 点击 **Add Trigger**。然后，点击 **New/Edit**。
- 选择以下触发器：`electroniz_master_trigger`。
- 点击 **OK**。

在 Edit trigger 面板中，按如下方式添加 Trigger Run 参数：

- **Name:** STORAGE_ACCOUNT  **Type:** String
  **Value:** traininglakehouse
- **Name:** BRONZE_LAYER_NAMESPACE  **Type:** String
  **Value:** bronze
- **Name:** SILVER_LAYER_NAMESPACE  **Type:** String
  **Value:** silver
- **Name:** STORE_SALES_FOLDER  **Type:** String
  **Value:** sales
- **Name:** TABLE_LIST  **Type:** String
  **Value:** products,store_customers,store_orders
- **Name:** CURRENCY_LIST  **Type:** String
  **Value:** CAD,INR,EUR
- **Name:** CURRENCY_FOLDER  **Type:** String
  **Value:** currency
- **Name:** GEOLOCATION_FOLDER  **Type:** String
  **Value:** geolocation
- **Name:** LOGS_FOLDER  **Type:** String
  **Value:** logs
- **Name:** ECOMM_FOLDER  **Type:** String
  **Value:** esalesns/esaleshub/*/*/*/*/*/*/*
- **Name:** EXT_TAB_FILE_FORMAT  **Type:** String
  **Value:** parquetformat
- **Name:** EXT_DATA_SOURCE  **Type:** String
  **Value:** trainingds
- **Name:** EXT_CREDENTIALS  **Type:** String
  **Value:** trainingcreds
- **Name:** EXT_TAB_LOCATION  **Type:** String
  **Value:** https://traininglakehouse.blob.core.windows.net

请根据你的 Azure 数据湖存储账户编辑上述参数。最终参数如下：

- **Name:** GOLD_LAYER_NAMESPACE  **Type:** String
  **Value:** gold:

## Edit trigger

Trigger Run Parameters

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_319_0.png)

图 9.12 – 触发器参数

- 3. 现在，从顶部菜单点击 **Publish all**，然后点击 **Publish** 按钮。

- 4. 从此以后，主管道将每小时调用一次。

> **Important**
> 对于一名数据工程师来说，成功创建功能性管道是一项重大成就。然而工作尚未完全结束。我们需要以一种使其具有持久性、安全性和响应性的方式部署管道。

接下来，我们将学习如何正确监控管道。

### 监控管道

要使用左侧面板监控每小时的运行，点击 Monitor。然后，点击 Trigger runs：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_320_0.png)

图 9.13 – Electroniz 主管道的首次触发运行

注意主管道如何每小时成功运行以摄取增量数据并重新计算聚合。希望这能让 Electroniz 成为一个非常满意的客户。作为一名数据工程师，你应该为自己的成就感到自豪。走到这一步需要付出很多努力。

在理想世界中，部署后管道应自动且无缝运行。现实是故障会发生，因此你需要提前做好准备。毕竟，你的管道从不同的源摄取数据，这些源可能并不完全在你的直接控制之下。某一天，你的管道所摄取的数据库可能因维护而停机，或者你订阅的 REST API 可能离线。

> **Important**
> 一名聪明的工程师会为最佳情况开发管道，但也会为最坏情况做好充分准备。

在其他情况下，问题可能是暂时的。管道可能由于网络波动而无法连接到数据库或 API。自然而然，作为一名数据工程师，不可能预测此类故障。然而，有方法可以使你的管道做出反应并优雅地处理错误。让我们研究一下可以这样做的方式。

### 添加持久性功能

在创建 Electroniz 管道时，我们创建了多种类型的活动来执行特定操作。仔细查看每个活动的属性，你会注意到三个重要的属性可以增加管道的持久性：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_321_0.png)

图 9.14 - 活动中的持久性属性

以下列表解释了这些参数：

- **Timeout**: 此属性表示特定活动应运行的最长时间。在 Azure 数据工厂中，默认值设置为 7 天。超时到期后，无论是否成功，活动都会被终止。这听起来可能很糟糕，但事实并非如此。相反，强烈建议将超时值设置为安全值。在开发和测试阶段进行一些初始监控后，很容易预测特定活动通常需要多长时间。例如，你的管道中可能有一个笔记本活动，通常需要 5 分钟才能完成。如果加上缓冲，你可能希望将值设置为 15-30 分钟。





我相信你会好奇这样做到底实现了什么。简单来说，就是节约云资源成本。如果一个程序意外地挂起，你是希望累积 7 天的云资源费用，还是仅仅 15–30 分钟？在这种情况下，让活动终止并单独调查意外挂起的原因可能更好。

> **重要**
> 聪明的数据工程师的标志之一是能够在坏和更坏之间做出有效选择。在某些场景下，从长远来看，选择快速失败机制会更好，因为后者可能导致大量不必要的云资源成本。

- **重试**：顾名思义，添加重试意味着在放弃之前活动将被重试的次数。Azure 数据工厂中该值的默认设置为 0，换句话说，就是只尝试活动一次。设置更高的重试值可以抵御瞬时故障，这些故障通常与网络故障和不可用性有关。在大多数情况下，值为 3 可能是一个不错的起点。然而，重试的行为会直接受到重试间隔的影响。
- **重试间隔**：重试机制会在每次重试尝试之间添加延迟。Azure 数据工厂中该值的默认值为 30 秒。如前所述，此属性经常与重试次数配对使用。

这些参数确实可以增加管道的耐久性；然而，它们并不能覆盖所有故障场景。某一天，故障总会发生。

> **重要**
> 在管道中预见并优雅地处理故障条件应得到周密规划和演练。毕竟，在数据工程师完成工作后，你将把系统移交给运营团队进行未来的监控和支持。

### 处理故障条件

如前所述，一个设计良好的管道应该能够优雅地处理故障条件。但问题是如何？让我们用以下故障场景来说明。

作为 Electroniz 管道的一部分，我们添加了从商店数据库摄取增量数据的活动。某一天，存储数据库因维护而宕机。因此，按小时触发的管道因数据库不可用而未能运行。在此场景中，Azure 数据工厂报告错误如下：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_323_0.png)

图 9.15 – 因数据库不可用导致的管道失败

总体而言，这是预期的行为。但真正的问题是如何处理这种情况？好吧，我们有几个选择——绕过失败的活动，让其余管道照常进行，或终止整个管道并单独处理故障。让我们接下来更详细地探讨这两个选项。

### 绕过失败的活动

在某些情况下，即使某个活动出现问题，也建议让其余管道继续进行。这就是操作方法：

- 1. 在 Azure 数据工厂中的每个活动内，你会注意到右下角的 **Add output icon**。点击它将显示 **Conditions** 菜单，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_324_0.png)

图 9.16 – 添加活动条件

- 2. 点击 **Failure**。现在，将 **Find SalesDB Tables** 的 **Failure** 条件链接到 **Find Currencies**。这样，如果销售数据库宕机，失败条件将不会终止整个作业；相反，它将绕过 **For Each Table** 并继续执行 **Find Currencies:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_324_1.png)

图 9.17 – 绕过失败的活动

> **重要**
> 绕过失败活动的决定需要非常谨慎地规划。在某些场景中，后续摄取的数据可能直接依赖于失败的活动。在这种情况下，当任何活动失败时，终止整个管道会更好。

### 重新运行失败的活动

处理故障的另一种方法是，只要任何单个活动出现问题，就让整个管道失败。如果管道活动之间存在强数据依赖关系，此方法经常被优先于绕过活动的方法。在此方法中，仔细评估失败活动运行的错误消息和日志。一旦发现并修复错误，就按如下方式重新运行失败的管道：

electroniz_batch_ingestion_pipeline

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_325_0.png)

图 9.18 – 重新运行失败的活动

以下列表演示了活动：

- 1. **重新运行**：整个管道可以使用重新运行选项重新运行。由于某些活动可能运行多次，因此此方法存在缺点，因为它可能在青铜层中重复数据。
- 2. **从失败的活动重新运行**：或者，如果使用此选项，你将只从失败的活动开始运行。虽然此方法可以避免重复，但它倾向于摄取不一致的数据，因为部分数据可能在不同的时间间隔内被摄取。

> **重要**
> 重新运行整个管道与从失败活动运行之间的决定需要仔细规划并充分测试。数据工程师应就所选方法征求客户的批准。

### 添加警报功能

在前一节中，我们仔细规划并执行了有关管道故障和重启机制的方法。为了增加管道的耐久性，我们应配置一些警报机制。如果没有适当的警报机制，管理员用户可能无法及时做出反应并从故障中恢复。这可能是灾难性的，因为从管道故障中恢复的延迟已被证明会导致巨大的客户不满、实时决策延迟，甚至更糟——数据损坏：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_326_0.png)

图 9.19 – 为管道添加警报

以下是创建实时警报的步骤：

- 1. 咨询客户，确定应接收警报的用户。电子邮件和短信是两种常见选择。与其向个人电子邮件地址发送警报，强烈建议你将警报发送到电子邮件组，例如 administrators@site.com。
- 2. 在 Azure 数据工厂中，点击 Monitor。现在点击 Alerts and metrics。然后，点击 New alert rule:
  - Alert rule name: Error Alerts
  - Description: Alert for pipeline failures
  - Severity: Sev1

- 3. 点击 + Add criteria。从 Add criteria 屏幕中，选择 Failed pipeline runs metrics:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_327_0.png)

图 9.20 – 为管道失败添加新警报

- 4. 点击 Continue:
  - 导航到 Configure alert logic 屏幕。
  - 在 Dimension 下方，输入以下内容：
  - Name: 点击 Select all

- **FailureType:** 点击 **Select all:**

## Configure alert logic

Failed pipeline runs metrics

Show history

Over the last 6 hours

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_328_0.png)

选择维度值将帮助你筛选到正确的时间序列。

| Dimension | Values |
| Name | 5 selected |
| FailureType | 3 selected |

## Alert logic

Condition *

Greater than

Time aggregation *

Total

Threshold count *

0

Update criteria

Cancel

图 9.21 – 配置警报逻辑

- 5. 点击 **Update criteria**。
- 现在，点击 + **Configure notification**。选择 **Create new**:

## Configure notification

Notify your team via email and text messages or automate actions using webhooks, runbooks, functions logic apps or integrating with external ITSM solutions.

- Create new
- Use existing

Action group name *
Pipeline Administrators

Short name *
PADMIN

Notifications *

+ Add notification

图 9.22 – 配置通知

- 6. 点击 + **Add notification**。

- 7. 点击 **Email** 复选框并添加警报组的电子邮件 ID。





8. 点击 SMS 复选框并添加一个电话号码以接收警报：

## 添加通知

了解更多关于定价和隐私声明。

操作名称 *

NewAction

## 选择您想要接收的通知类型

Email

manoj_kukreja@yahoo.com

SMS

国家代码

电话号码 *

1

6474649563

可能收取运营商费用。

Azure 应用推送通知

输入您用于登录 Azure 账户的电子邮件。了解如何使用 Azure 应用连接您的 Azure 资源。

email@example.com

语音

国家代码

电话号码 *

1

1234567890

图 9.23 – 添加电子邮件和 SMS 以接收警报

9. 点击添加通知：

## 配置通知

通过电子邮件和短信通知您的团队，或使用 webhook、runbook、函数、逻辑应用或集成外部 ITSM 解决方案来自动化操作。

创建新
使用现有

操作组名称 *
Pipeline Administrators

简称 *
PADMIN

| 通知 | 操作类型 | 操作 |
|---|---|---|
| NewAction | Email/SMS/Push/Voice | ✏️ 🗑️ |

+ 添加通知

图 9.24 – 添加操作组确认屏幕

10. 点击 **添加操作组**：

## 新警报规则

警报规则名称 *

Error Alerts

描述

警报管道失败

严重性 *

Sev1

目标条件
当 Pipeline Failed Runs 指标 大于 0 时

+ 添加条件

配置的条件将收取月费。了解更多关于定价

通知
Pipeline Administrators
操作组类型
1 Email, 1 SMS

+ 配置通知

创建时启用规则
开

创建警报规则

取消

图 9.25 – 创建警报规则确认屏幕

11. 点击 **创建警报规则。**

规则创建后，您将通过电子邮件收到确认：

## 您已被添加到 Azure Monitor 操作组

您现在已加入操作组，将收到发送到该组的通知。

查看 Azure Monitor 操作组的详细信息 >

## 账户信息

订阅 ID：

资源组名称：

操作组名称：Pipeline Administrators

图 9.26 – 确认警报订阅的示例电子邮件

您还将在移动设备上收到警报，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_333_0.png)

图 9.27 – 确认警报订阅的示例 SMS

这证实警报规则现在 **已激活**。从现在开始，如果管道出现故障，您应该会收到类似以下的警报：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_334_0.png)

图 9.28 – 管道失败后发送的示例电子邮件和 SMS 警报

> **重要**
> 在实施警报机制时，数据工程师应仔细筛选值得发送警报的重要场景。警报噪音过大通常会被客户视为负面因素。

因此，我们学习了如何监控管道、添加耐久性，以及添加警报功能以在管道失败时发出警告。

## 摘要

在本章中，我们学习了各种部署策略，例如基于时间和基于事件的机制。此外，使用基于事件的机制，我们创建了 **Electroniz** 主管道。在手动测试主管道后，我们将其安排每小时运行一次。我们还学习了如何监控管道、添加耐久性功能以及处理故障条件。最后，我们研究了如何为管道添加警报，以便在出现故障时通知用户管理组。

下一章非常重要，因为它为数据工程师提供了一些关键技能。在 *第 10 章，解决数据工程挑战* 中，我们将学习现代数据工程师面临的一些常见挑战以及推荐的处理方法。

### 10
解决数据工程挑战

在过去几章中，我们学习了数据湖仓架构。在完成多个练习后，我们学习了数据工程师如何构建和部署湖仓的 bronze、silver 和 gold 层。湖仓中的数据会随时间增长和变化。随着新数据源的添加和旧数据源的修改，数据工程实践需要跟上这种增长。就像行业中的任何事物一样，数据工程师的角色也需要不断发展。除了构建和部署数据管道外，他们还需要涵盖之前未涉及的多个复杂的数据工程方面。他们必须学会应对这些新挑战。

在本章中，我们将涵盖以下主题：

+   - 模式演进
+   - 数据共享
+   - 数据治理

#### 模式演进

**模式演进** 可以描述为一种用于适应数据结构持续变化的技术。随着系统成熟并增加更多功能，模式演进是不可避免的。因此，适应模式演进是现代管道的极其重要的要求。

通常在项目开始时开发管道，使其具有表的基模式。经常在进入生产环境时，某个传入文件或表的模式已经发生变化的可能性非常高。但为什么这是个大问题？

> **重要**
> 数据工程师绝不能犯下假设传入数据的模式永远不会改变的错误。相反，应准备管道使其能够自动适应这种演进。

让我们讨论一个示例场景来说明这一点。假设您的管道已部署到生产环境，并且您一直在 bronze 层摄取一个库存文件，其中包含三列：**inventory_date**、**product** 和 **inventory**。在 silver 层，存在一个名为 `inventory` 的相同文件的 Delta 表。第二天，当库存文件被摄取时，发现它包含的不是三列而是四列。库存文件的模式已经演进。现在的问题是如何有效地将这个新列反映到湖仓中，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_338_0.png)

图 10.1 – 模式演进

真正的挑战在于，库存文件的先前数据列数少于今天收到的新文件。我们如何容纳新列，同时保持先前数据的完整性？过去，我们以两种方式处理这个问题：

+   - 我们会创建多个版本的库存表，每次模式更改时都会创建一个新版本的表。为了获得聚合结果，我们可以在所有版本之上创建一个 **VIEW**。这种方法有严重的缺点。不仅每次模式更改时都需要创建新版本的表，而且包含的视图也需要更改。
+   - 一个更简单的方法是使用值为 **NULL** 的虚拟列重写旧数据文件。之后，您可以简单地更改表的模式并向其中添加新列。这种方法比多版本方法稍好，但如果先前数据极大，则可能成为问题。

如果先前存在的列从新的传入文件中被删除，可能会出现类似情况。处理此删除场景的方法与我们刚刚讨论的非常相似，只是处理方式相反。在这种情况下，先前数据保持不变，但缺少该列的数据在未来数据中默认为 **NULL**。

> **重要**
> 在开发管道之前，请确保客户完全同意为处理模式演进而选择的方法。项目投入生产后更改此方法可能会非常昂贵。

幸运的是，我们可以在这里实施一个更简单的方法，这涉及使用 Delta Lake。**Delta Lake** 不仅可以通过强制执行 **模式验证** 来保护您的数据，还可以调整模式，而无需表版本控制或使用虚拟列重写先前数据。Delta Lake 会自动将传入数据的模式与先前数据进行比较。如果在 Delta Lake 中检测到差异，则默认会引发异常。您可以覆盖此默认行为并强制将模式更改合并到表中，而无需重写先前数据。让我们通过以下示例来了解 Delta 表中的模式验证和演进：

+   1. 将一些示例库存文件上传到湖仓的 `bronze` 层。逐行复制此处显示的每个命令，将其粘贴到 Azure Cloud Shell 窗口中，然后按 *Enter*：

```
STORAGEACCOUNTNAME="traininglakehouse"
STORAGECONTAINER="bronze"
```




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

我们刚刚将三个库存文件上传到 Azure Data Lake。在继续之前，让我们了解每个文件代表什么：

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

- 2. 让我们找出如何有效地处理这种不断演变的模式方案。同样，对于本练习，我们将使用 Azure Databricks 从 Azure Data Lake Storage 读取和写入数据。如果您之前创建的 Azure Databricks 工作区已删除，可以执行以下步骤重新创建它。

- 3. 获取 Azure Data Lake Storage 的存储帐户密钥。在 Azure Cloud Shell 中调用以下命令：

```
STORAGEACCOUNTNAME="traininglakehouse"
az storage account keys list --account-name $STORAGEACCOUNTNAME
```

如果上述命令按预期工作，您应该会看到两个存储密钥。请注意 key1 的值。我们稍后在 Databricks 工作区配置中需要它：

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

4. 使用以下代码创建 Azure Databricks 工作区。在 Azure Cloud Shell 上调用以下命令：

```
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
WORKSPACE="trainingdatabricks"
az config set extension.use_dynamic_install=yes_without_prompt
az databricks workspace create --resource-group $RESOURCEGROUPNAME --name $WORKSPACE --location $LOCATION --sku trial
```

如果您没有 Azure 试用帐户，则需要使用 standard 的 SKU：

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

5. 要验证 Azure Databricks 工作区的创建，请导航到 Azure 门户。

为此，单击 **Home > All Resources > trainingdatabricks**，然后单击 **Launch Workspace:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_344_0.png)

Figure 10.7 – Azure Databricks workspace on the Azure portal

使用左侧菜单单击 **Compute**。然后，单击 **Create Cluster**。进行以下更改：

- **Cluster name:** `curationcluster`.
- **Cluster mode:** `Single node`.

- 根据您的 Azure 存储帐户名称和密钥编辑 `fs.azure.account.key.<storage-account-name>.blob.core.windows.net <storage-account-access-key>` 命令。
- 单击 **Advanced options.**

将编辑后的命令粘贴到 Spark Config 中作为最后一行。现在，按 **Create Cluster** 按钮：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_345_0.png)

Figure 10.8 – Azure Databricks Create Cluster button

6. 这将启动一个小型 Apache Spark 集群，该集群将是本练习所必需的。集群创建完成后，您应该会看到如下窗口：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_346_0.png)

Figure 10.9 – Azure Databricks cluster creation

7. 现在，我们需要一个笔记本作为我们的交互式开发环境（IDE）。

在 Databricks 工作区中，单击 Workspace，然后单击 Users。

然后，单击您用户名旁边的箭头并单击 Import：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_347_0.png)

Figure 10.10 – Importing a notebook into the Azure Databricks workspace

- 选择 **URL**。
- 使用以下 URL：https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/blob/main/project/challenges/schema_evolution/inventory_notebook.ipynb。

单击 Import。

现在，单击集群下拉菜单并选择 curationcluster：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_347_1.png)

Figure 10.11 – Choosing a Spark cluster for the notebook

8. 对于下一组命令，您应该在笔记本界面上。在运行笔记本单元之前，请确保根据您的 Azure 帐户编辑 Cmd 2 笔记本单元中的 STORAGE_ACCOUNT 和 ADLS_KEY。

9. 运行 Cmd1、Cmd2 和 Cmd3。这些说明将使用来自 bronze 层的 Day 1=2021-04-18 库存文件创建一个名为 df_read_inventory1 的 Spark DataFrame：

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

请注意 df_read_inventory1 有三列：inventory_date、product 和 inventory。

- 现在，运行 Cmd4、Cmd5、Cmd6 和 Cmd7。这些说明将使用 Day 1=2021-04-18 库存文件创建一个 Delta 表：



```
%sql
DROP TABLE IF EXISTS `inventory`; 
```

OK
Command took 8.08 seconds -- by datauniversity.ca@gmail.com at 2021-07-12, 9:09:38 p.m. on curationcluster

```
DELTA_TABLE_WRITE_PATH="wasbs://"+SILVER_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+INVENTORY_FOLDER
df_read_inventory1.write.format("delta").option("path", DELTA_TABLE_WRITE_PATH).saveAsTable(TABLE)
```

(4) Spark Jobs
Command took 10.73 seconds -- by datauniversity.ca@gmail.com at 2021-07-12, 9:10:15 p.m. on curationcluster

```
%sql
SELECT * FROM `inventory`; 
```

(1) Spark Jobs

|  | inventory_date | product | inventory |
|---|---|---|---|
| 1 | 2021-04-18 | Watch | 100 |
| 2 | 2021-04-18 | Vacuum | 500 |
| 3 | 2021-04-18 | Airconditioner | 45 |
| 4 | 2021-04-18 | Microwave | 34 |
| 5 | 2021-04-18 | DVD | 6789 |
| 6 | 2021-04-18 | TV | 43 |
| 7 | 2021-04-18 | Keyboard | 564 |
Showing all 11 rows.

Figure 10.13 – Creating the inventory Delta table using the 2021-04-18 file

10. Notice that the inventory table schema in Delta Lake now has three columns; that is, inventory_date, product, and inventory:

```
%sql
DESCRIBE `inventory`; 
```

|  | col_name | data_type | comment |
|---|---|---|---|
| 1 | inventory_date | string |  |
| 2 | product | string |  |
| 3 | inventory | int |  |
| 4 |  |  |  |
| 5 | # Partitioning |  |  |
| 6 | Not partitioned |  |  |
Showing all 6 rows.

Figure 10.14 – Checking the inventory Delta table's schema

- Next, run Cmd8. These instructions will create a Spark DataFrame named df_read_inventory2 using the Day 2=2021-04-19 inventory file:

```
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+INVENTORY_FOLDER
df_read_inventory2 = spark.read.csv(BRONZE_TABLE_PATH, header='True', inferSchema='True')
display(df_read_inventory2)
```

(3) Spark Jobs

df_read_inventory2: pyspark.sql.dataframe.DataFrame = [inventory_date: string, product: string ... 2 more fields]

|  | inventory_date | product | inventory | warehouse |
|---|---|---|---|---|
| 1 | 2021-04-19 | Watch | 100 | Arizona |
| 2 | 2021-04-19 | Vacuum | 500 | New York |
| 3 | 2021-04-19 | Airconditioner | 45 | Texas |
| 4 | 2021-04-19 | Microwave | 34 | New York |
| 5 | 2021-04-19 | DVD | 6789 | Texas |
| 6 | 2021-04-19 | TV | 43 | New York |
| 7 | 2021-04-19 | Keyboard | 564 | New York |
Showing all 11 rows.

Figure 10.15 – Reading the 2021-04-19 inventory file

Notice that df_read_inventory2 has four columns: inventory_date, product, inventory, warehouse.

- Run Cmd9. This instruction will try to merge df_read_inventory2 into the existing inventory Delta table:

```
df_read_inventory2.write.format("delta").mode("append").save(DELTA_TABLE_WRITE_PATH)
```

AnalysisException: A schema mismatch detected when writing to the Delta table (Table ID:

AnalysisException
Traceback (most recent call last)
<command-2814988441641260> in <module>
----> 1 df_read_inventory2.write.format("delta").mode("append").save(DELTA_TABLE_WRITE_PATH)

Figure 10.16 – Schema mismatch exception

Here, the instruction ran into an exception. If you scroll down the error window, you will see the following output:

```
AnalysisException: A schema mismatch detected when writing to the Delta table (Table ID: 2990042c-3e69-4a50-b75c-70f8d2d9ef25).
To enable schema migration using DataFrameWriter or DataStreamWriter, please set:
'.option("mergeSchema", "true")'.
For other operations, set the session configuration
spark.databricks.delta.schema.autoMerge.enabled to "true". See the documentation
specific to the operation for details.

Table schema:
root
-- inventory_date: string (nullable = true)
-- product: string (nullable = true)
-- inventory: integer (nullable = true)

Data schema:
root
-- inventory_date: string (nullable = true)
-- product: string (nullable = true)
-- inventory: integer (nullable = true)
-- warehouse: string (nullable = true)
```

Figure 10.17 – Schema mismatch between the 2021-04-18 and 2021-04-19 inventory files

This is the expected behavior. The Delta table has correctly detected a schema mismatch between the incoming data and the pre-existing Delta table. By default, Delta Lake will throw an exception if such a mismatch is detected. The real question is how to deal with such a situation.

11. Now, run Cmd10 and Cmd11. Notice that a new option has been added in the merge command. mode=append overrides the default behavior and instructs Delta Lake to proceed with the data merge by adjusting the schema:

```
df_read_inventory2.write.format("delta").mode("append").option("mergeSchema", "true").save(DELTA_TABLE_WRITE_PATH)
```

```
%sql
DESCRIBE inventory;
```

|  | col_name | data_type | comment |
|---|---|---|---|
| 1 | inventory_date | string |  |
| 2 | product | string |  |
| 3 | inventory | int |  |
| 4 | warehouse | string |  |
| 5 |  |  |  |
| 6 | # Partitioning |  |  |
| 7 | Not partitioned |  |  |
Showing all 7 rows.

Figure 10.18 – Creating the inventory Delta table using the 2021-04-19 file using mode=append

Notice that this time, an exception was not raised. Instead, Delta Lake calmly adjusted the schema of the pre-existing table by adding a new column to it.

> **Important**
> The decision to either raise an exception or use mode=append in the Delta table's merge commands should be discussed and agreed upon with the customer.

12. Run Cmd12. Note how Delta Lake has adjusted the data after merging the data and schema. All data before Day 2=2021-04-19 has been marked as NULL because the new column warehouse did not exist before:

```
Cmd 12
1   %sql
2   SELECT * FROM inventory;
```

(2) Spark Jobs

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_352_0.png)

Figure 10.19 – Checking the data in the inventory Delta table

Now, let's see what happens when we try to merge the Day 3=2021-04-20 inventory file with the inventory table. You may recall that on this day, a new column (city) was added but a previous column was deleted (warehouse).

Run Cmd13. These instructions will create a Spark DataFrame named df_read_inventory3 using the Day 3=2021-04-20 inventory file:

```
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+INVENTORY_FOLDER
df_read_inventory3 = spark.read.csv(BRONZE_TABLE_PATH, header='True', inferSchema='True')
display(df_read_inventory3)
```

(3) Spark Jobs

df_read_inventory3: pyspark.sql.dataframe.DataFrame = [inventory_date: string, product: string ...]

| inventory_date | product | inventory | city |
|---|---|---|---|
| 1 | 2021-04-20 | Watch | 100 | Phoenix |
| 2 | 2021-04-20 | Vacuum | 500 | New York |
| 3 | 2021-04-20 | Airconditioner | 45 | Dallas |
| 4 | 2021-04-20 | Microwave | 34 | New York |
| 5 | 2021-04-20 | DVD | 6789 | Dallas |
| 6 | 2021-04-20 | TV | 43 | New York |
| 7 | 2021-04-20 | Keyboard | 564 | New York |
Showing all 11 rows.

Figure 10.20 – Reading the 2021-04-20 inventory file

13. Run Cmd14. This instruction will try to merge df_read_inventory3 in the existing inventory Delta table.

Run Cmd15. Notice how Delta Lake was able to adjust the schema once again. Even though the warehouse column was deleted in the incoming file, it remains in the Delta table:

```
df_read_inventory3.write.format("delta").mode("append").option("mergeSchema", "true").save(DELTA_TABLE_WRITE_PATH)
```

(4) Spark Jobs

Command took 3.66 seconds -- by datauniversity.ca@gmail.com at 2021-07-12, 9:43:51 p.m. on curationcluster

```
%sql
DESCRIBE inventory;
```

| col_name | data_type | comment |
|---|---|---|
| 1 | inventory_date | string |  |
| 2 | product | string |  |
| 3 | inventory | int |  |
| 4 | warehouse | string |  |
| 5 | city | string |  |
| 6 |  |  |  |
| 7 | # Partitioning |  |  |
Showing all 8 rows.

Figure 10.21 – Creating the inventory Delta table using the 2021-04-20 file using mode=append

14. Run Cmd16:

```
%sql
SELECT * FROM inventory;
```

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_354_0.png)

Figure 10.22 – Checking the data in the inventory Delta table

This time, after merging data from Day 3=2021-04-20, the warehouse column gets defaulted to NULL, and the new city column has been added to the inventory table.

With that, you've seen how Delta Lake can transparently adjust the data and schema each time a schema change is detected. Hopefully, this example was able to effectively highlight how schema evolution can be managed in modern data engineering pipelines. We will now look at sharing data. This is quickly becoming a necessity for organizations who are wanting to harness the power of data in monetary terms.

##### Sharing data

In Chapter 1, The Story of Data Engineering and Analytics, we discussed the power of data. This has enabled organizations to realize revenue diversification using data monetization. But this dream cannot be effectively realized without sharing data with external parties. In the past, organizations used several data-sharing mechanisms such as emails, SFTP, APIs, cloud storage, and hard drives:



![](img/dae8a2243a3c3cbcd9c56ff3d2217672_355_0.png)

Figure 10.23 – State of data sharing currently

不幸的是，这些数据共享方法存在若干问题：

- **Complex**: 这些数据共享机制设置和使用起来可能很复杂，因为它们可能需要交换密钥/密码并使用多种不同的工具。
- **Insecure**: 这些机制可能不安全，无法保护静态数据或传输中数据的安全。这意味着经典的 **man-in-the-middle** 攻击可能会以明文形式泄露数据。
- **Tracking**: 目前没有有效的方法来追踪谁与谁共享了数据以及使用的是哪种机制。
- **Control**: 你无法控制共享数据的使用者。
- **Updates**: 数据共享是一个持续的过程，而前面提到的机制并不能提供简单的方式来共享更新。

幸运的是，有一种有效的替代方案。Azure 允许组织使用名为 **Azure Data Share** 的简单且安全的服务在内部和外部共享数据。使用 Azure Data Share，共享数据的用户（**providers**）可以轻松邀请他们希望共享数据的用户（**consumers**）。在数据与消费者共享之前，他们需要接受数据使用条款。数据更新也可以按照提供方配置的设定频率进行共享。数据共享可以以两种方式进行。我们将在以下小节中分别介绍这两种方式。

##### In-place sharing

在这种情况下，数据始终保留在提供方的 Azure 订阅中，并向消费者的 Azure 订阅发送一个符号链接。消费者可以使用此符号链接读取数据，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_356_0.png)

Figure 10.24 – In-place sharing using Azure Data Share

##### Snapshot sharing

在这种情况下，数据从数据提供方的 Azure 订阅移动并落地到数据消费方的 Azure 订阅中：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_356_1.png)

Figure 10.25 - Snapshot sharing using Azure Data Share

让我们通过一个简单的练习来了解更多关于共享数据的信息。对于此练习，你需要一位共享伙伴来充当消费者。你的伙伴应拥有一个可操作的 Azure 账户。如果没有，他们可以创建一个新的。

##### Preparing the Azure resources

我们将开始准备共享数据所需的 Azure 资源：

- 1. 在 Azure 门户中，搜索 Data Shares 并导航到它。
- 2. 在 Data Shares 页面，点击 Create。
- 3. 在 Create Data Share 页面，输入以下详细信息：
  - Subscription: Free Tier or Paid Subscription
  - Resource group: training_rg
  - Location: East US 2
  - Name: trainingshare
点击 Review + create：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_357_0.png)

Figure 10.26 – Creating a new data share

- 4. 注意顶部出现的 Validation Passed 消息。按下 Create。
- 5. 新创建的数据共享现在将被部署。部署完成后，你应收到一条 Your deployment is complete 的消息。

6. 点击 Go to resource。这应会带你进入新创建的数据共享页面：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_358_0.png)

Figure 10.27 – Home screen of the new data share

> **Important**
> 决定在内部或外部共享数据的决定极其敏感。作为数据工程师，你在进行数据共享之前必须强制寻求客户的书面批准。

##### Creating a data share

现在我们已经创建了一个新的数据共享账户，可以继续共享数据：

- 1. 点击 Start sharing your data。现在，点击 Create 并提供以下值：
  - Share name: golddata
  - Share type: Snapshot

点击 Continue。这应会带你进入 Datasets 选项卡。从这里，点击 Add datasets 并选择 Azure Data Lake Gen2：

Figure 10.28 – Choosing a dataset type for the data share

2. 点击 Next 并提供以下值：
  - **Subscriptions:** Free Tier or Paid Subscription
  - **Resource groups:** training_rg
  - **Storage accounts:** traininglakehouse:

Figure 10.29 – Choosing an Azure Data Lake Storage account for the data share

3. 点击 Next。

在 Select datasets 页面，选择 gold 并点击 Next：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_360_0.png)

Figure 10.30 – Choosing the gold dataset for the data share

4. 在 Rename datasets 页面，保持名称不变或按需更改：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_360_1.png)

Figure 10.31 – Renaming the dataset for the data share

5. 点击 Add datasets 然后 Continue。

现在，我们将添加将接收共享的消费者（伙伴）的电子邮件地址值。

6. 点击 **Add recipient**。输入收件人的电子邮件地址并点击 **Add**，然后 **Send invitation**：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_361_0.png)

Figure 10.32 – Adding the email address of the consumer

7. Azure 现在将向消费者发送电子邮件，消费者需要接受邀请并执行一系列步骤以从新创建的数据共享中下载数据。

请注意，接下来的步骤系列是从消费者的角度出发的。此后显示的一些屏幕截图将反映消费者在收到电子邮件后所看到的内容。为了显示这种差异，所有具有 **白色背景** 的屏幕截图均来自消费者的 Azure 账户。

8. 消费者将收到如下所示的电子邮件：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_361_1.png)

Figure 10.33 – Invitation email for the consumer

9. 点击 View Invitation 后，请求将被转发到消费者的 Azure 门户。登录门户后，将出现以下屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_362_0.png)

Figure 10.34 – The customer viewing the invitation

点击 golddata 并提供以下值：
  - Data share account name: golddatashare
  - Subscription: Free Tier or Paid Subscription
  - Resource group: training_rg
  - Location: East US 2

点击 Create。你将收到以下错误：

The subscription is not registered to use namespace 'Microsoft.DataShare'. See https://aka.ms/rps-not-found for how to register subscriptions.

无需惊慌 – Azure 订阅默认不会启用数据共享。

10. 导航到 Subscriptions 页面并点击你的 Azure 订阅：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_363_0.png)

Figure 10.35 – Enabling data sharing in the consumer's subscription

11. 现在，使用左侧菜单点击 Resource providers：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_363_1.png)

Figure 10.36 – Navigating to the resource providers in the consumer's subscription

12. 使用顶部的搜索栏，输入 datashare。注意 Microsoft.Datashare 目前尚未注册：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_364_0.png)

Figure 10.37 – Checking the state of Microsoft.Datashare in the consumer's subscription

13. 选择 Microsoft.Datashare 并点击 Register。Microsoft.Datashare 现在将为消费者的 Azure 订阅注册。请注意，此步骤可能需要几分钟才能完成：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_365_0.png)

Figure 10.38 – Registering Microsoft.Datashare in the consumer's subscription

14. 一旦状态更改为 Registered，返回数据共享账户选项卡并再次点击 Create。希望这次过程将不会出现任何错误：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_365_1.png)

Figure 10.39 – Creating a new data share in the consumer's subscription

15. 数据共享邀请现在将被显示。输入以下屏幕截图中显示的详细信息，然后点击 Accept and configure：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_366_0.png)

Figure 10.40 – Setting the data share parameters in the consumer's subscription

16. 成功数据共享的确认屏幕应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_366_1.png)

Figure 10.41 – Home screen of the data share in the consumer's subscription





17. 现在，单击 Datasets。注意，数据共享尚未映射到接收方侧的存储帐户：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_367_0.png)

图 10.42 – 在消费者的订阅中检查接收到的共享

18. 单击 Map to target 并提供以下值：

- Target data type: Azure data lake storage Gen2
- Subscription: Free Tier or Paid Subscription
- Resource group: training_rg
- Storage accounts: <Consumers Storage Account>

消费者可以自由选择其订阅中的任何存储帐户来接收共享数据。

- Path: goldshared:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_368_0.png)

图 10.43 – 在消费者的订阅中将数据集映射到目标

19. 单击 Map to target。确认屏幕应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_368_1.png)

图 10.44 – 在消费者的订阅中确认数据映射

20. 现在，我们已准备好**触发**一次完整的快照，将共享数据从**提供者**帐户复制到**消费者**存储帐户。

在我们触发**完整快照**之前，我们需要为存储帐户授予一个角色。

使用新的浏览器标签页，导航到 Azure 门户。

单击 **Home > All Resources > <Consumers Storage Account>**。

然后，使用左侧菜单，单击 **Access Control (IAM)**。

在此处，单击 **Add role assignment**。

现在，选择 **Owner** 并单击 **Next**。

选择 **User, group, or service principal**，然后单击 **Select members**。

选择 Azure **principal**，其名称以用户登录 Azure 门户时使用的相同帐户名称开头。单击 **Select** 然后 **Next**：

## Add role assignment

Role
Members
Review + assign

**Selected role**
Owner

**Assign access to**
● User, group, or service principal
○ Managed identity

**Members**
+ Select members

| Name | Object ID | Type |
|------|-----------|------|
| [redacted] | 39ea52c6-0ad8-4736-b35a-da3222e579... | User |

图 10.45 – 为消费者的订阅添加角色分配

最后，单击 **Review + assign**。

21. 返回到 Data share 选项卡。单击 Trigger snapshot 并选择 Full copy：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_370_0.png)

图 10.46 – 在消费者的订阅中触发新的快照

22. 这将触发从提供者帐户到消费者帐户的完整快照复制。注意通知区域中的消息。此区域应显示快照复制的状态：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_370_1.png)

图 10.47 – 在消费者的订阅中检查快照创建的通知

23. 经过几分钟后，返回存储帐户。你应该在存储帐户中看到一个名为 goldshared 的新容器，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_371_0.png)

图 10.48 – 在消费者的订阅中检查新的快照

24. 现在，如果你单击 goldshared，你应该会看到以下文件夹：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_371_1.png)

图 10.49 – 在消费者的订阅中检查数据共享的内容

25. 现在，单击任意文件夹并检查文件是否已成功复制，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_372_0.png)

图 10.50 – 在消费者的订阅中检查 blob

在这里，我们能够以简单、安全和可问责的方式在提供者和消费者之间成功共享数据。接下来，我们将研究组织在处理敏感数据时面临的另一个关键挑战。

##### Data governance

我以“每字节数据都有一个故事可讲。真正的问题是这个故事是否被准确、安全、高效地讲述”作为本书的开篇。虽然组织忙于利用数据的真正力量，但数据治理和安全经常被忽视。但它不能被忽视太久。GDPR 等法规正在对未能满足与数据隐私、保留和可移植性相关的治理政策的组织施加法律问责和严格的处罚。

有效的 data governance 路径通常始于有效的数据发现、分类和跟踪血缘的方法。考虑到数据种类、量和速度的巨大差异，这可能是一项艰巨的任务。Azure Purview 是一种新的统一治理服务，可自动化诸如发现、分类和血缘等任务。

使用 Azure Purview，注册的数据源可以自动发现一次或定期按计划发现。发现的数据基于预定义或自定义规则进行分类。一旦数据被编目，用户可以通过提供描述、标签和访问规则手动增强元数据。让我们探索 Azure Purview 如何有效地处理此问题。

##### Preparing the Azure resources

我们将开始准备 Azure Purview 帐户：

- 1. 在 Azure 门户搜索中，搜索 `Purview accounts` 并导航到它。
- 2. 在 **Purview accounts** 页面，单击 **Create Purview account** 并提供以下值：
  - **Subscription:** `Free Tier or Paid Subscription`
  - **Resource group:** `training_rg`
  - **Purview account name:** `trainingcatalog`
  - **Location:** `East US 2:`

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_373_0.png)

图 10.51 – 创建新的 Purview 帐户

单击 Review + Create：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_374_0.png)

图 10.52 – 指定新 Purview 帐户的参数

单击 Create。部署完成后，单击 Go to Resource：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_374_1.png)

图 10.53 – 新 Purview 帐户的主屏幕

现在我们有了 Azure Purview 帐户，我们已准备好创建数据目录。

##### Creating a data catalog

数据目录是数据资产及其关系的集合。它通过使终端用户能够使用术语、分类和敏感性标签发现相关数据来促进有效的治理。按照以下步骤创建它：

- 1. 单击 **Open Purview Studio:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_375_0.png)

图 10.54 – trainingcatalog 的 Purview Studio 主屏幕

2. 要注册数据源，单击 Register Sources，然后单击 Register：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_376_0.png)

图 10.55 – 在 trainingcatalog 中选择 Azure Data Lake Storage Gen2

选择 Azure Data Lake Storage Gen2。

单击 Continue 并提供以下值：

- Name: trainingsource
- Subscription: Free Tier or Paid Subscription
- Storage account name: traininglakehouse
- Endpoint: 根据之前的选择自动填充

- Select a collection: None:

## Register sources (Azure Data Lake Storage Gen2)

Name *
trainingsource

Azure subscription
Azure subscription 1 (0b35426c-85f3-45e8-9f96-577a13b738d5)

Storage account name *
traininglakehouse

Endpoint
https://traininglakehouse.dfs.core.windows.net/

Select a collection
None

图 10.56 – 在 trainingcatalog 中注册新源

3. 单击 Register：

## Sources

Register   New collection   Refresh   Map view   Table view

Filter by keyword

Showing 0 collections, 1 source

trainingsource
Azure Data Lake Storage Gen2
View details

图 10.57 – 确认 trainingsource 的创建

4. 我们现在准备扫描我们创建的数据源。在继续之前，我们需要确保 trainingsource 具有扫描 traininglakehouse 的适当权限。

使用 Azure 门户，导航到 Home > All resources > traininglakehouse。

5. 使用左侧菜单，单击 Access Control (IAM)：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_378_0.png)

图 10.58 – traininglakehouse 的 Access Control (IAM)

单击 + Add，然后 Add role assignment。提供以下值：

- Role: Storage Blob Data Reader
- Assign access to: User, group, or service principal
- Select: trainingcatalog:



![](img/dae8a2243a3c3cbcd9c56ff3d2217672_379_0.png)

图 10.59 – 为 trainingcatalog 授予角色

点击 Save。

6. 现在，返回到 Azure Purview Studio：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_379_1.png)

图 10.60 – 调用新的扫描

7. 要开始扫描，请点击 New Scan 图标。提供以下值：

- Name: Scan Data
- Connect via integration runtime: 使用默认值

+   - **Credential:** 使用默认值：

## Scan "AzureDataLakeStorage-UG8"

Name *

Scan-Data

Connect via integration runtime * ⓘ

Azure AutoResolveIntegrationRuntime

Credential *

Purview MSI

> 💡 在设置扫描之前，您必须授予 Purview 账户托管标识连接到 Azure Data Lake Storage Gen2 的权限。了解更多

Continue

Connection successful

Test connection

Cancel

图 10.61 – 新扫描详情

点击 Test Connection，然后点击 Continue。

8. 在 Scope your scan 屏幕上，选择 bronze，如下所示：

## Scope your scan

Refresh

所有未来位于某个父项下的资产，如果父项被完全或部分选中，将自动被选中。

Search

AzureDataLakeStorage-UG8

bronze

gold

silver

Continue

Back

Cancel

图 10.62 – 为新扫描选择 bronze 层

点击 **Continue。**

> **Important**
> bronze 层是湖屋中的数据第一入口。因此，它是扫描可能包含分类信息（例如 PII）数据的良好候选。

9. 对于 **Select a scan rule set**，选择 **AdlsGen2**。AdlsGen2 是默认扫描规则集，包含所有支持的用于模式提取和分类的文件类型，以及所有支持的系统分类规则：

## Select a scan rule set

+ New scan rule set  ↻ Refresh

选择一个扫描规则集供您的扫描使用。

**AdlsGen2** [SYSTEM DEFAULT]
Microsoft 默认扫描规则集，包含所有支持的用于模式提取和分类的文件类型，以及所有支持的系统分类规则 View detail

Continue   Back                                   Cancel

图 10.63 – 为新扫描选择扫描规则

10. 点击 **Continue。** 在 **Set a scan trigger** 屏幕上，您可以设置重复运行或仅运行一次的扫描：

## Set a scan trigger

设置扫描触发器以在特定日期和时间运行扫描。如果选择一次，扫描将在设置完成后开始。如果选择重复，扫描将在您选择的日期和时间开始。初始扫描为全量扫描，后续每次扫描为增量扫描。

○ Recurring    ● Once

Continue   Back                                   Cancel

图 10.64 – 为新扫描设置扫描触发器

现在，我们选择 **Once**。点击 **Continue**：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_382_0.png)

图 10.65 – 查看新扫描的设置

11. 点击 **Save and Run**。您现在应该回到 **Sources** 屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_382_1.png)

图 10.66 – 查看新扫描详情

12. 此时，bronze 层中的数据正在被扫描。等待几分钟让扫描完成。要检查运行状态，请点击 View Details：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_383_0.png)

图 10.67 – 新扫描状态

扫描完成后，其状态将为 Completed。

> Important
> 
> 在生产环境中，每次运行摄取管道时您都可能收到新数据。因此，建议使用重复选项，以便定期扫描增量数据。

13. 我们现在准备浏览之前扫描的资产。使用左侧菜单，点击 Home：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_384_0.png)

图 10.68 – 浏览已扫描的资产

14. 点击 Browse Assets，然后点击 Azure Data Lake Storage Gen2。

15. 通过进入 traininglakehouse > bronze > sales > [dbo].[store_customers] > T Z 来浏览目录结构。

您将看到 Schema classifications，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_384_1.png)

图 10.69 – 检查分类

注意 Azure Purview 中的扫描如何自动为两个列发现了 **Classifications**：**Email Address** 和 **Credit Card Number**。正如您在前几章中回忆的那样，我们在 store customer 表中有这些列。那么，为什么对数据进行分类如此重要呢？

**数据分类**是满足合规要求和数据治理的重要第一步。基于数据的敏感性定义适当的标签、标记和元数据有助于在查询时定义数据保密性。

> **Important**
> 根据业务数据的类型，分类通常是基于数据内容、上下文和用户级别执行的。

## Cleaning up Azure resources

为节省成本，您可能希望清理以下 Azure 资源：

+   1. 删除 Azure Purview 账户；即 `trainingcatalog`。
+   2. 删除 Azure Data Share 账户；即 `trainingshare`。

现在，让我们总结本章。

## Summary

本章因以下几个原因非常重要。通过几个示例，我们了解了数据工程领域常见的挑战。处理诸如模式演变和治理等挑战对于现代数据工程项目至关重要。这些挑战随着时间的推移而演变，同样使数据工程师的工作变得更轻松的技术和工具也在不断发展。

在下一章中，我们将了解数据工程师的 DevOps 基础知识。DevOps 正在迅速成为数据工程师必备的附加技能，因此我们将学习如何使用 **基础设施即代码 (IaC)** 范式自动配置云资源。利用 IaC 的强大功能，数据工程师可以快速、可重复且安全地启动使用云资源的数据管道。

### 11
Infrastructure Provisioning

随着数据分析需求的增长，数据工程师在市场上正变得昂贵且难以找到。与此同时，雇用数据工程师的组织正在寻找创新方法以用更少资源做更多事情，从而证明高资源成本的合理性。在最近的趋势中，大多数这些组织开始使用自动化基础设施配置来简化云部署。直到最近，基础设施配置工作通常由 DevOps 团队处理，但现在不再如此。

在上一章中，我们谈到了数据工程师工作角色的动态性。现代数据工程师需要跟上这一最新趋势，并在 DevOps 技能方面进行培训。本章和下一章旨在教授数据工程师一些关键的 DevOps 技能。

> **Important Note**
> 在当今的就业市场中，精通 DevOps 的数据工程师是致命的组合。现代组织更愿意雇用一个同时具备这两种技能的资源，而不是雇用两个。

在本章中，我们将涵盖以下主题：

+   - Infrastructure as code
+   - Deploying infrastructure using Azure Resource Manager
+   - Deploying multiple environments using IaC

#### Infrastructure as code

直到最近，基础设施部署一直是一个极其耗时且昂贵的过程。在典型的组织中，系统管理员团队负责使用一系列手动执行步骤在多个环境中部署硬件和软件。由于其手动性质，该过程存在主要缺陷：

+   - **Unique settings per environment**: 每个独立环境都可能具有独特的设置。为满足这些要求，手动步骤需要为每个环境记录。
+   - **Time-consuming**: 手动步骤的记录和执行非常耗时。
+   - **Inconsistent and error-prone**: 遵循手动步骤进行基础设施部署可能导致不一致和错误。

**基础设施即代码 (IaC)** 是一种使用源代码部署和管理基础设施组件的做法。使用 IaC，系统管理团队（也称为 DevOps）不再受限于手动实践和记录流程。相反，他们现在可以使用配置文件执行自动化和可重复的部署。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_388_0.png)

图 11.1 – 使用 IaC 的好处





使用 IaC 实践部署的基础设施是准确的、一致的、可重用的，并且速度快。DevOps 团队以与应用程序团队创建应用程序代码类似的方式创建、管理和版本控制基础设施配置文件。IaC 实践遵循相同的错误修复、测试和代码推广协议集。

市场上有多款开源和商业化的 IaC 工具可供选择，例如 Ansible、Terraform、Chef 和 Puppet。对于基础设施配置，Azure Resource Manager（ARM）是 Azure 中的原生平台。现代组织正在利用 ARM 的强大功能来组织和标准化其云部署，以实现统一性、安全性和性能。

##### 使用 Azure Resource Manager 部署基础设施

在 ARM 于 2014 年出现之前，Azure Service Manager（ASM）用于基础设施部署。然而，由于资源之间的相互依赖，ASM 方法存在一些严重的缺点。在部署资源之前，你必须仔细了解依赖关系，并在脚本中相应地排序操作。

值得庆幸的是，ARM 已经解决了相互依赖问题，可以轻松、统一、 seamlessly 地部署 Azure 资源。ARM 使用资源组作为云资产的逻辑分组。使用资源组可确保资源配置、安全性和拆除的一致生命周期。ARM 将资源组用作部署和管理的单一单元。

ARM 使用模板来部署 IaC。一旦创建，这些模板就会像其他类型的应用程序代码一样成为企业代码库的一部分。

##### 创建 ARM 模板

ARM 模板只是一个嵌入了资源配置的 JSON 格式文件。ARM 模板使用声明式语法，这意味着你可以在一个模板中部署多个 Azure 资源。在运行时，ARM 会自动按正确顺序排列操作，并在可能的情况下并行执行。

在我们为 IaC 部署创建新模板之前，我们需要了解 ARM 模板的格式。

```
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#", << Version of template language
  "contentVersion": "", << Version of template
  "apiProfile": "",      << Collection of API versions
  "parameters": { },      << External values provided to template
  "variables": { },       << Internal values created using expressions
  "functions": [ ],       << User defined functions
  "resources": [ ],       << Resource types to be deployed
  "outputs": { }          << Output values returned by deployment
}
```

图 11.2 – ARM 模板的解剖结构

模板中的每个元素都有多个可以在其中设置的属性。让我通过以下示例来演示这一点。在本章的整个过程中，我们将引用 Packt Git 仓库中以下位置的 ARM 模板：

https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/tree/main/project/infra

对于以下示例，让我们了解一个部署 Electroniz Azure Data Lake Storage 账户的简单模板。

###### Parameters

ARM 模板的参数部分保留用于输入值。参数对于强制执行 ARM 模板的可重用性非常重要。你可能会注意到这里有两个名为 environment 和 project 的参数。这些参数可用于在不同项目上启动不同的环境，例如 dev、test 或 prod。

```
"$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json",
"contentVersion": "1.0.0.0",
"parameters": {
  "environment": {
    "type": "string"
  },
  "project": {
    "type": "string"
  },
  "storageAccountName": {
    "type": "string",
    "metadata": {
      "description": "The name of the storage account"
    }
  },
  "bronzeLayer": {
    "type": "string"
  },
  "silverLayer": {
    "type": "string"
  },
  "goldLayer": {
    "type": "string"
  },
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]",
    "metadata": {
      "description": "The location in which the resources should be deployed."
    }
  }
}
```

图 11.3 – ARM 模板中的参数

## Variables

ARM 模板的变量部分保留用于创建可在整个模板中使用的自定义值。自定义值是使用两个方括号 ([ ]) 表示的表达式创建的。在表达式中，你可以使用 ARM 提供的多个预定义函数，例如 CONCAT、MIN 和 MAX。

```
"variables": {
  "storageAccountName": "[concat(parameters('environment'), parameters('project'), parameters('storageAccountName'))]"
},
```

图 11.4 – ARM 模板中的变量

注意 storageAccountName 变量中使用了 CONCAT 函数。此表达式将项目名称和环境名称添加到 storageAccountName 参数中。这使得更容易识别 Azure 资源。

###### Resources

ARM 模板的资源部分保留用于定义所需资源（一个或多个）。在以下示例中，需要使用 storageAccountName 变量创建一个新的存储账户。请注意 isHnsEnabled 设置为 true，这意味着这是 Azure Data Lake Storage Gen2。

```
"resources": [
  {
    "name": "[variables('storageAccountName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2021-02-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "StorageV2",
    "location": "[parameters('location')]",
    "properties": {
      "isHnsEnabled": true
    },
    "tags": {
      "owner":"data engineering",
      "project":"lakehouse",
      "environment":"production"
    }
  },
  "resources": [
    {
      "name": "[concat('default/', parameters('bronzeLayer'))]",
      "type": "blobServices/containers",
      "apiVersion": "2019-06-01",
      "dependsOn": [
        "[variables('storageAccountName')]"
      ],
      "properties": {
        "publicAccess": "None"
      }
    }
  ]
```

图 11.5 – ARM 模板中的资源定义

另外，请注意在外部资源内嵌入了一个新的子资源。子资源用于在父资源内创建资源。在此实例中，我们在给定的存储账户中创建一个容器。请注意 dependsOn 属性的使用，它用于建立父子关系。

##### 使用 Azure 门户部署 ARM 模板

与 Azure 中的许多其他服务一样，ARM 模板可以从 Azure 门户和 Azure 客户端部署。在项目的初始阶段，使用 Azure 门户提供了一种方便的方法来开发、测试和部署基础设施代码。一旦模板正常工作，它们可以通过脚本使用 Azure 客户端部署。首先，我们将使用 Azure 门户部署一个模板：

- 1. 在 Azure 门户上，导航到 Home。现在点击 Create a resource 图标。
- 2. 在搜索框中，输入 Templates，然后按 Enter。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_393_0.png)

图 11.6 – 在 Azure 门户上使用 ARM 模板创建资源

- 3. 选择 Template deployment（deploy using custom templates）。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_393_1.png)

图 11.7 – 创建自定义 ARM 模板

点击 **Create**。

- 4. 我们现在准备创建新模板。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_394_0.png)

点击 **Build your own template in the editor**。

- 5. 复制以下文件的内容并粘贴到文本框中。这与上一节中的模板相同：

```
https://raw.githubusercontent.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/main/project/infra/storage_accounts/storage_accounts.json
```

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_394_1.png)





注意模板的读取方式，ARM 成功识别了此模板创建的正确参数数量、变量和资源。单击“保存”。

6. 在自定义部署屏幕中，输入以下值：

- **订阅：** 免费套餐或付费订阅。
- **资源组：** 单击“创建新”。
- **名称：** `elz_prod`。

单击“确定”。

现在输入以下值：

- **区域：** 美国东部 2
- **环境：** prod
- **项目：** dlake

在许多情况下，Azure 资源名称在全球范围内是唯一的。尽量使项目名称尽可能唯一。由于大量读者会使用相同的说明，具有唯一性将消除与唯一资源名称相关的不必要错误：

- **存储帐户名称：** elz
- **青铜层：** bronze
- **白银层：** silver
- **黄金层：** gold
- **位置：** [resourceGroup().location]

单击“查看 + 创建”。

7. 检查“验证通过”消息。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_396_0.png)

图 11.10 – 验证自定义 ARM 模板

单击“创建”。

8. ARM 现在将开始部署。你现在应该会看到如下的“部署进行中”屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_396_1.png)

图 11.11 – 部署自定义 ARM 模板

9. 部署完成后，你应该会看到如下的“**部署完成**”屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_397_0.png)

图 11.12 – 部署完成确认消息

单击“**转到资源组**”。

10. 检查是否已创建新的存储帐户。注意存储帐户的名称嵌入了环境和项目名称。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_397_1.png)

图 11.13 – 新创建资源组中的 Azure 资源

单击“**存储帐户名称 (proddlakeelz)**”。

11. 现在单击“**容器**”。注意 ARM 模板中作为子资源创建的三个新容器。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_398_0.png)

图 11.14 – 新创建资源组中的 Azure 资源

12. 返回到“**主页 | 资源组 | elz_prod**”。

使用左侧菜单，单击“**部署**”。注意 ARM 维护了所有已执行部署的日志。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_398_1.png)

图 11.15 – 检查新创建资源组中的部署

##### 使用 Azure CLI 部署 ARM 模板

一旦使用 Azure 门户开发并测试了模板，就必须使用 Azure 客户端运行它。这将为我们提供模板已准备好在未来使用部署脚本使用的保证。

> **重要说明**
> 尽管使用 Azure 门户部署 ARM 模板是一个相对简单的过程，但许多管理员更喜欢使用 **Azure CLI** 进行自动化部署。

这就是如何使用 Bash 部署 Lakehouse_Storage_Account 模板。 使用 Azure Cloud Shell 调用以下命令：

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

正如我们所见，这非常简单易行。

##### 部署包含机密的 ARM 模板

在上一节中，我们部署了一个相对简单的 ARM 模板。如果 Azure 资源需要在创建资源时设置管理员凭据，则模板的复杂性会增加。由于我们使用代码创建基础设施，因此在配置文件中留下暴露的引用密码是不安全且违反安全实践的。在这种情况下，必须妥善解决模板的安全方面。此安全问题在启动 Azure SQL 和 Azure Synapse 等资源时经常遇到。幸运的是，Azure ARM 可以轻松满足此要求。它可以在部署过程中在运行时从 Azure Key Vault 检索机密。流程始于在 Azure Key Vault 中创建基础设施机密。做法如下：

- 1. 在 Azure Cloud Shell 中运行以下命令：

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

如果运行正确，你应该会看到以下输出：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_400_0.png)

图 11.16 – 新 Azure Key Vault 创建的输出

2. 我们现在将向新创建的 Azure Key Vault 添加机密。使用 Azure 门户，导航到“主页 | 资源组 | elz_prod | deploykvelz”。

单击“机密”。然后使用“生成/导入”创建两个新机密：

- 名称：SQLUSER   值：salesadmin
- 名称：SQLPASSWORD   值：<Azure SQL 管理员的密码>

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_400_1.png)

图 11.17 – 检查 Azure Key Vault 中新创建的机密

###### 重要说明

尽管可以在 Azure Key Vault 中自动创建机密，但强烈建议手动执行此步骤。你可以请求客户的 IT 团队为你创建机密。使用此方法可确保他们方面的保密性。

3. 我们现在准备部署 Stores_Azure_SQL_Database 模板。使用 Azure Cloud SQL 运行以下命令。这些命令将部署 Azure SQL 数据库模板，同时使用先前创建的 Azure Key Vault 中的机密动态获取数据库凭据：

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

4. 导航到“主页 | 资源组 | elz_prod”。如果前面的命令成功，你应该能够看到新创建的 SQL 服务器和数据库，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_402_0.png)

图 11.18 – 检查 Azure SQL 数据库的创建

5. 有趣的是可以了解 ARM 中是如何实现机密的动态获取的。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_402_1.png)

图 11.19 – ARM 从 Azure Key Vault 获取机密的过程

ARM 允许使用嵌套模板。使用从命令行传递的参数（vaultName、adminLoginUsersecretName、adminPasswordsecretName），父模板为机密生成 Azure Key Vault 资源 ID。

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

图 11.20 – 父 ARM 模板生成 Azure 密钥库资源 ID

这些资源 ID 由嵌套模板用于从 Azure Key Vault 检索机密的实际值。

> **重要说明**
> 在高度安全的项目中，将生产机密隐藏在数据工程师之外并非标准做法。在这种情况下，机密由客户的安全团队创建。创建后，这些机密在运行时动态检索，而不会暴露给未经授权的实体。

##### 使用 IaC 部署多个环境

在上一节中，我们了解到一旦 DevOps 团队创建了 IaC 资产，就可以通过重复部署来创建不同的环境，每次都能实现一致的结果。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_403_0.png)

图 11.21 – 使用 IaC 部署多个环境的过程





让我们按照以下步骤开始：

+   1. 如果你的两个模板都按预期工作，就没有必要一个一个地创建下一组资源。让我们像专业人士一样操作，运行一个脚本来为我们创建剩余的资源。

+   2. 这就是部署脚本（electroniz_isc.sh）的样子。它为需要创建的每个 Azure 资源都有一个部分。注意部署脚本中模板文件和参数文件的使用。模板和参数文件的路径作为参数传递给此脚本。这意味着我们可以重用同一个脚本来部署多个环境。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_404_0.png)

图 11.22 – IaC 部署脚本

+   3. 真相揭晓的时刻到了。在 Azure Cloud Shell 中执行以下命令并观看其运行。脚本将在几分钟内创建整个基础设施。这就是 IaC 的魔力。请确保根据你的环境将以下命令中的 <HOME_DIR> 值替换为实际值：

```
IAC_ENVIRONMENT=prod
HOME_DIR_PATH=<HOME_DIR>
sh Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/infra/electroniz_isc.sh IACRESOURCEGROUPNAME=elz_prod RESOURCEGROUPNAME=elz_prod LOCATION=eastus TEMPLATE_FILE_PATH=$HOME_DIR_PATH/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/infra PARAMETERS_FILE_PATH=$HOME_DIR_PATH/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/infra/parameters/$IAC_ENVIRONMENT
```

注意前述命令中使用了 IAC_ENVIRONMENT 和 PARAMETERS_FILE_PATH 参数，这可以强制执行 IaC 脚本的可重用性方面。要构建多个环境，你只需在源代码管理中为每个环境创建一个文件夹，并在前述命令中引用它即可。ARM 模板文件对每次构建都保持不变。以下是生产环境的参数文件夹示例：

## 图 11.23 – IaC 环境参数

坐下来，放松，让部署脚本完成它的工作。脚本完成需要几分钟时间。

+   4. 现在是验证脚本是否按预期完成工作的时候了。
+      让我们通过返回 Home | Resource group | elz_prod 来检查。

## 图 11.24 – 部署后 Azure 资源列表

5. 部署脚本似乎按预期工作。我们可以看到在 ez_prod 资源组下部署了多个 Azure 资源。此外，我们可以通过导航到 Home | Resource group | ez_prod | Deployments 来检查部署的状态。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_406_0.png)

图 11.25 – 检查当前部署

使用 IaC 创建云资源是实现更快、更一致且可重复部署的第一步。IaC 流程可以轻松集成到现有的企业软件开发周期中。从长远来看，它已被证明不仅可以节省时间，还可以节省大量与使用手动部署相关的不必要重复成本。

然而，一个巨大的问题仍然没有得到解答……快速和一致就足够了吗？与其他事物一样，云资源和管道会随着时间而演变。如果没有自动化，我们仍然需要有人在每次更改时调用这些脚本。如果更改能自动部署就太好了？我们将在下一章探讨对此可以做什么。

##### 清理 Azure 资源

在离开之前，请确保使用以下命令清理 ez_prod 资源组。让资源闲置没有任何意义，月底会产生高额账单：

```
RESOURCEGROUPNAME="ez_prod"
az group delete --name $RESOURCEGROUPNAME
```

这会产生以下输出：

```
Azure> RESOURCEGROUPNAME="elz_prod"
Azure> az group delete --name $RESOURCEGROUPNAME
Are you sure you want to perform this operation? (y/n): y
Azure>
```

图 11.26 – 删除资源组时的确认屏幕

##### 总结

在本章中，我们首次体验了使用 IaC 原则部署云资源。整体部署流程可通过此原则大幅加速。通过引入自动化，此流程可以进一步改进和简化。

在最后一章，第 12 章《数据管道的持续集成和部署（CI/CD）》中，我们将学习如何自动化管道部署。使用 CI/CD 是成熟云部署的标志性实践。CI/CD 可以在保持高效率的同时大幅加速代码交付。在下一章中，我们将了解如何使用 CI/CD 自动化基础设施部署和代码交付。
