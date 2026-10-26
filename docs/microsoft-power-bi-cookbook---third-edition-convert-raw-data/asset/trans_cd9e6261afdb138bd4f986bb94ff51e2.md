

## 开始准备

要准备本配方，请执行以下步骤：

- 1. 确保在您的 Azure 租户中已配置 Azure Data Lake Storage Gen2 帐户。
- 2. 确保已配置 Power BI Premium 容量（P SKU）、Power BI Premium Per User（PPU）许可证、Microsoft Fabric 容量（F SKU）或 Power BI Embedded 容量（A SKU）。
- 3. 在 Power BI 服务中，打开管理门户（请参阅第 14 章中的 Accessing the Admin Portal 配方）。
- 4. 对于 Premium、Embedded 和 Fabric 容量，从管理门户的左侧导航窗格中选择容量设置。
- 5. 选择 Power BI Premium、Power BI Embedded 或 Fabric 容量，然后展开工作负载部分并确保 XMLA 端点设置设置为读取写入。 此更改生效可能需要等待长达 10 分钟。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_564_0.png)

图 13.17：XMLA 端点设置

- 6. 如果使用 PPU 许可证，从管理门户的左侧导航窗格中选择 Premium Per User，并在数据集工作负载设置或语义模型工作负载设置下，确保 XMLA 端点设置为读取写入。
- 7. 导航到服务中的工作区并选择工作区设置。 在工作区设置对话框的 Premium 选项卡上，确保许可证模式为 Premium 容量、Premium per user、Fabric 容量或 Embedded。
- 8. 在工作区设置对话框的 Premium 选项卡底部，复制工作区连接字符串：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_564_1.png)

图 13.18：工作区 XMLA 端点

- 9. 单击工作区设置对话框的 Azure 连接选项卡，配置连接 Azure Data Lake Gen2 存储帐户设置，然后单击连接 Azure 按钮。 工作区中不得包含任何数据流，否则连接将失败。

工作区设置

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_565_0.png)

图 13.19：将工作区连接到 Azure Data Lake Gen2 存储帐户

- 10. 退出工作区设置对话框并将 Power BI Desktop 文件发布到工作区，或在工作区内创建语义模型。

您现在可以完成本配方。

## 如何备份语义模型

要实现本配方，请执行以下操作：

- 1. 打开 SQL Server Management Studio（SSMS）。
- 2. 在连接到服务器对话框中，将服务器类型选择为 Analysis Services，将服务器名称粘贴为在开始步骤 8 中复制的工作区 XMLA 端点，并选择 Microsoft Entra MFA 作为身份验证。
- 5. 查看备份数据库对话框中的选项，然后单击确定按钮：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_567_0.png)

图 13.22：SSMS 中的备份数据库选项

这完成了语义模型的备份。

## 其工作原理

将工作区连接到 Azure Data Lake Gen2 存储帐户并正确配置工作区的 XMLA 端点为读取写入。 备份语义模型需要 XMLA 端点配置为读写访问似乎很奇怪，因为备份通常只需要读取访问。 然而，请考虑备份文件（.abf）是通过 XMLA 端点备份到 Azure Data Lake Gen2 存储帐户，因此 XMLA 端点需要写入访问。

## 还有更多...

除了通过 SSMS 用户界面备份语义模型外，还可以通过运行查询来执行备份。 下面查询可用于通过 XMLA 端点备份连接的 Power BI 工作区的语义模型：

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

最后，您可以通过 Azure Storage Explorer 确认备份文件是否已创建。 Azure Storage Explorer 可在此处下载：https://azure.microsoft.com/en-us/products/storage/storage-explorer/。

在 Azure Storage Explorer 中，导航到本配方 *开始* 部分中配置的存储帐户。 在存储帐户下方的左侧导航中展开 **Blob Containers** 部分。 在 **Blob Containers** 下将有一个名为 **power-bi-backup** 的容器。 在该 blob 中有一个与工作区同名的文件夹。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_568_0.png)

*图 13.23：Azure Storage Explorer*

备份文件（.abf）位于工作区的文件夹中。

## 另请参阅

- *使用 XMLA 端点的语义模型连接：* https://bit.ly/3SXSO Gh

## 利用 Direct Lake

历史上，Power BI 有两种语义模型模式，**导入**和**DirectQuery**。 使用导入模式，数据从源系统摄取到 Analysis Services 语义模型中的表。 使用 DirectQuery，语义模型在需要时动态查询源系统以获取数据。 许多人还将实时连接作为第三种语义模型模式。 然而，这在技术上是不正确的，因为实时连接只是创建报表与现有语义模型之间的连接，因此本身并不是语义模型模式。 Microsoft 后来引入了复合语义模型模式，该模式允许在单个语义模型中组合导入模式和 DirectQuery 模式。

除了语义模型模式外，Power BI 还具有语义模型中表的存储模式。 自然地，导入模式语义模型使用导入的表存储模式，而 DirectQuery 语义模型使用 DirectQuery 的表存储模式。 当引入复合语义模型时，Microsoft 添加了另一种表存储模式，称为 **双存储模式**，它使表能够以缓存模式（导入）或直接查询数据源（DirectQuery）运行。 Microsoft 还引入了混合表的概念，表包含多个分区，至少一个分区处于导入存储模式，另一个分区处于 DirectQuery 存储模式。

当 Microsoft 发布 Fabric 时，引入了第三种表存储模式 Direct Lake。 本配方演示如何创建具有 Direct Lake 存储模式表的语义模型。

## 开始准备

要准备本配方，请执行以下步骤：

- 1. 确保已配置 Power BI Premium 容量（P SKU）或 Microsoft Fabric 容量（F SKU）。
- 2. 完成第 1 章 Installing and Licensing Power BI Tools 中的 Installing SQL Server and the AdventureWorks Database 配方。
- 3. 确保已安装数据网关并配置了到 SQL Server 数据库的网关连接。 请参阅第 1 章中的 Creating an On-Premises Data Gateway 配方和第 11 章中的 Configuring On-Premises Gateway Data Connections。
- 4. 创建新的工作区并将工作区分配给 Premium 或 Fabric 容量。
- 5. 导航到工作区并在右上角选择 New，然后选择 More options。
- 6. 在数据工程下，选择 Lakehouse，为您的湖仓提供名称，然后单击创建按钮。
- 7. 使用新的 Dataflow Gen2 数据流填充您的湖仓。
- 8. 使用您的 SQL Server 上的 AdventureWorks 数据库作为数据源，选择各种维度和事实表，如 DimCustomer、DimDate、DimProduct、DimReseller、DimSalesTerritory、FactInternetSales 和 FactResellerSales。
- 9. 允许数据流完成其初始刷新。
- 10. 打开湖仓并验证表是否已加载到湖仓中。

您现在可以完成本配方。

## 如何利用 Direct Lake

要实现本配方，请执行以下操作：

- 1. 在湖仓打开时，从功能区中选择新建语义模型：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_569_0.png)

图 13.24：为湖仓创建新的语义模型

