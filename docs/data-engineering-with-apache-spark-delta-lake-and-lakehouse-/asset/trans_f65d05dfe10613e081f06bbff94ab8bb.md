

在 **Activities** 面板中，搜索 **Copy data**。现在，将 **Copy data** 活动拖到右侧面板：

Name: `Copy Website Tracking Data`

点击 **Source**。

在 **Source** 数据集中，点击 **New**。然后，点击 **Azure Blob Storage**。最后，点击 **Continue**。

选择 **Format**。点击 **JSON**。然后，点击 **Continue**。

在 **Set properties** 窗格中，输入以下内容：

Name: `TrackingJSON`

Linked service: **websitetrackinglogs**

File path: `ecommercelogs`

点击 **OK**。返回到 **Copy Website Tracking Data** 活动。

对于 **File path type**，执行以下操作：

选择 **Prefix**。

Prefix: `electroniz_access`

点击 **Sink**。

在 **Sink** 数据集中，点击 **New**。然后，点击 **Azure Data Lake Storage Gen 2**。最后，点击 **Continue**。

选择 **Format** 并点击 **JSON**。然后，点击 **Continue**。

在 **Set properties** 窗格中，输入以下内容：

Name: `TrackingJSONSync`

Linked service: **traininglakehouse**

在 **File path** 中，输入以下内容：

Filesystem: `bronze`

点击 **OK**。在 **Sink** 数据集旁边，点击 **Open**。

选择以下目录：`logs/@
{formatDateTime(utcnow(), 'yyyy')}/@
{formatDateTime(utcnow(), 'MM')}/@
{formatDateTime(utcnow(), 'dd')}/@
{formatDateTime(utcnow(), 'HH')}`

31. 在 **Pipelines** 下，点击之前创建的管道 **electroniz_batch_ingestion_pipeline**。通过将箭头从 **For Each Currency** 拖到 **Copy Website Tracking Data** 来连接活动。此外，将箭头从 **Copy website tracking data** 拖到 **Delete Website Tracking Logs**。

最后，我们希望清理已成功摄取的电子商务网站跟踪日志。在 **Activities** 面板中，搜索 **Delete**。现在，将 **Delete** 活动拖到右侧面板：

Name: `Delete Website Tracking Logs`

点击 **Source**：

Dataset: **TrackingJSON**

File path type: **File path in dataset**

在日志设置中，在 Enable logging 中，选择 **Unchecked**。

32. 现在从顶部菜单点击 **Publish all**，然后点击 **Publish** 按钮。

发布后，管道应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_161_0.png)

Figure 5.45 – The final Electroniz batch ingestion pipeline

全部完成！现在是时候检查新创建的 **electroniz_batch_ingestion_pipeline** 管道是否正常工作了。继续执行以下步骤对管道进行单元测试。

## Testing the ingestion pipelines

我们现在将测试 **electroniz_batch_ingestion_pipeline** 管道。你可能还记得，Electroniz 希望其销售数据库与湖仓之间最大延迟为 1 小时。将来，我们将配置一个触发器，每小时运行一次 **electroniz_batch_ingestion_pipeline** 管道。现在，我们将手动多次调用此管道以测试结果。

## Testing the batch ingestion pipeline for historical data

执行以下步骤：

- 1. 要调用管道，请在 Azure Cloud Shell 上执行以下命令：

```
PIPELINENAME="electroniz_batch_ingestion_pipeline"
DATAFACTORYNAME="trainingdatafactory100"
RESOURCEGROUPNAME="training_rg"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME
```

- 2. 现在点击 Pipeline runs。过一段时间后，Status 将变为 Succeeded。这表示运行成功：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_162_0.png)

Figure 5.46 – The status of the Electroniz batch ingestion pipeline on completion

- 3. 在 Pipeline name 下方，点击 electroniz_batch_ingestion_pipeline 管道。注意每个活动都已成功。如果将光标悬停在活动名称上，你将能够看到每个活动的输入和输出日志。花尽可能多的时间熟悉这些输出。如果管道出现错误，将在 Error 列下报告：

## Activity runs

Pipeline run ID 119153a8-cc5d-4a2e-a3fe-da350c297099

All status

Showing 1 - 22 of 22 items

| Activity name | Activity type | Run start | Duration | Status | Integration runtime |
|---|---|---|---|---|---|
| Delete Website Tracking Logs | Delete | 4/28/21, 2:16:29 PM | 00:00:13 | Succeeded | DefaultIntegrationRuntime (East US) |
| Copy Website Tracking Data | Copy data | 4/28/21, 2:16:18 PM | 00:00:07 | Succeeded | DefaultIntegrationRuntime (East US) |
| Get Currency Value | Copy data | 4/28/21, 2:16:04 PM | 00:00:06 | Succeeded | DefaultIntegrationRuntime (East US) |
| Get Currency Value | Copy data | 4/28/21, 2:16:04 PM | 00:00:06 | Succeeded | DefaultIntegrationRuntime (East US) |
| Get Currency Value | Copy data | 4/28/21, 2:16:04 PM | 00:00:06 | Succeeded | DefaultIntegrationRuntime (East US) |
| For Each Currency | ForEach | 4/28/21, 2:16:03 PM | 00:00:14 | Succeeded |  |
| Find Currencies | Lookup | 4/28/21, 2:15:57 PM | 00:00:04 | Succeeded | DefaultIntegrationRuntime (East US) |
| Update Watermark | Stored procedure | 4/28/21, 2:15:46 PM | 00:00:02 | Succeeded | DefaultIntegrationRuntime (East US) |
| Copy Table Data | Copy data | 4/28/21, 2:15:37 PM | 00:00:05 | Succeeded | DefaultIntegrationRuntime (East US) |
| Update Watermark | Stored procedure | 4/28/21, 2:15:35 PM | 00:00:02 | Succeeded | DefaultIntegrationRuntime (East US) |
| Update Watermark | Stored procedure | 4/28/21, 2:15:34 PM | 00:00:12 | Succeeded | DefaultIntegrationRuntime (East US) |
| Copy Table Data | Copy data | 4/28/21, 2:15:28 PM | 00:00:05 | Succeeded | DefaultIntegrationRuntime (East US) |

Figure 5.47 – The Electroniz batch ingestion pipeline run details

## Important

从运营角度来看，数据工程师应非常熟悉管道日志记录机制。由于多种原因，你应该预期你的管道总有一天会在生产环境中失败，而那一天，你的调试技术将派上用场。

4. 让我们验证数据是否已正确摄取。在 Azure 门户中，导航到 Home | All resources | traininglakehouse。

点击 Containers | Bronze。你应该会看到以下四个文件夹：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_164_0.png)

Figure 5.48 – The data ingested using the Electroniz batch pipeline

5. 花一些时间点击上述文件夹，并验证数据是否符合预期。

由于你将在与管道运行时不同的数据上运行，因此各个文件夹的日期和小时可能与以下屏幕截图不完全匹配：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_164_1.png)

Figure 5.49 – Delimited data for the sales.product table as ingested by the Electroniz batch pipeline

从目前来看，electroniz_batch_ingestion_pipeline 管道在第一次运行中按预期工作。

## Testing the batch ingestion pipeline for incremental data

我们现在将检查管道对商店数据库中增量数据的反应。为此，我们将模拟对数据库的更改，包括插入、更新和删除。之后，我们将重新触发 electroniz_batch_ingestion_pipeline 三次，并验证新添加的数据是否已成功传输到湖仓。

## The second run of the pipeline – inserts

目前，初始摄取后的第一个小时正在进行。我们假设此时，sales 数据库已完全运行，并且正在插入新客户和订单。为了模拟插入活动，让我们在 sales 数据库中插入一些数据：

- 1. 返回 Azure SQL 数据库并检查最新的水印。

导航到 Home | All Resources | salesdb | Query Editor。运行以下查询：

```
SELECT TOP (1000) * FROM [dbo].[watermarktable]
```

注意每个表的 watermark 值。此时间戳代表 electroniz_batch_ingestion_pipeline 最后一次摄取数据的时刻：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_165_0.png)

Figure 5.50 – The watermark values after the first run of the batch ingestion pipeline

