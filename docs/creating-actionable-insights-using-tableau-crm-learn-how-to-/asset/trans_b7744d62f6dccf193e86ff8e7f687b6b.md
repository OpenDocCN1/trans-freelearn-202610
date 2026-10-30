

## 数据集的定义、设计和构建

首先，什么是数据集？下图给出了一个简明的定义：

### 什么是数据集？

一种数据存储库（存储），类似于文件系统存储，但采用专有格式，并通过算法使用倒排索引来展平数据（以提高容量和速度）。

![](img/06379e43783b9ddfee45e9da5192efe1_71_0.png)

图 3.1 – 什么是数据集？

数据集是分析透镜和仪表板的构建基块，也是洞察的源泉。俗话说，垃圾进，垃圾出；在数据分析领域，这句话再贴切不过。因此，在设计和构建 TCRM 仪表板时，你需要投入大量思考和精力来打造驱动指标和可视化效果的数据集。

那么，TCRM 如何以及从哪里提取、存储、转换数据，并将其加载到数据集中？下图概述了 TCRM 的数据版图，以及在平台中摄取数据的各种方式：

![](img/06379e43783b9ddfee45e9da5192efe1_72_0.png)

图 3.2 – Tableau CRM 数据版图

那么，你能从*图 3.2*中了解到什么？请看以下几点：

- 多种数据源会被复制/同步（请参阅*数据同步*部分），并通过数据流和配方导入 TCRM。这些数据包括来自主要连接的 Salesforce 组织、其他 Salesforce 组织、Marketing Cloud 以及其他 TCRM 连接器的数据。
- Salesforce 趋势报告数据可以直接导入 TCRM。
- **平面文件**可以通过手动将 CSV 文件加载到 TCRM，并在数据流或配方中与其他数据合并和转换，从而补充到数据集中。
- **外部数据、ETL 工具和 API**（例如数据湖、SQL 和 Oracle）会按照各自的手动计划，为 TCRM 中的数据集补充数据。
- TCRM 数据流可以接收各种数据源，并将这些数据与其他来源及 TCRM 数据集中的数据合并和转换。相关内容将在*第 4 章，构建数据配方*和*第 5 章，使用 TCRM Data Prep 进行高级 ETL*中详细介绍。
- 默认情况下，Tableau CRM 执行增量同步。你可以指定同步是提取每个 Salesforce 对象的增量变更，还是提取所有记录。如前所述，增量同步运行速度更快。
- 如果你的 TCRM 管理员在 Winter '20 版本发布之前启用了 TCRM，则必须启用数据同步和连接选项，以优化数据流并连接到外部数据。请注意，如果你在 Winter '20 版本发布之后启用 TCRM，则默认会启用数据同步。
- 你可以通过 Tableau CRM 的 Snowflake 输出连接器，将原始的本地 Salesforce 数据导出到 Snowflake。

在下一节中，你将学习如何在 TCRM 中提取和使用外部数据。

### 将外部数据与 Salesforce 数据摄取并合并

部署和构建 Tableau CRM 仪表板的一个主要原因，是将 Salesforce 数据与外部数据合并到统一可信的数据源中。那么，具体该怎么做？下面通过一个示例来说明。

登录你的 TCRM 开发组织，导航至 **Analytics Studio**，然后执行以下步骤：

1. 点击 **Create** 按钮并选择 **Dataset**。
2. 选择 **External Data** 选项，随后将进入 **Data Manager**。
3. 点击左侧的 **Connect** 链接。
4. 点击 **Connect to Data** 按钮。
5. 在 **Input Connections** 选项卡下，点击 **Add Connection** 按钮，随后会出现以下窗口：

**为新的远程连接选择连接类型**

![](img/06379e43783b9ddfee45e9da5192efe1_75_0.png)

图 3.3 – 远程连接示例

6. 浏览可用选项，你会看到这里提供了多种数据源。选择 Redshift Connector 选项，随后会出现一个对话框，需要填写连接器相关信息，例如：

- (a) **连接名称**
- (b) **开发者名称**
- (c) **描述**
- (d) **AWS Secret Access Key**
- (e) **用户名**
- (f) **AWS Access Key ID**

需要填写的详细信息因连接器而异。填写完必要信息后，需要保存并测试连接器。正确配置并成功测试后，即可在 TCRM 中使用这些数据进行合并和转换。可以在 **Data Manager** 的 **Connect** 部分对连接器进行计划安排、编辑或删除。

### TCRM 可用的远程数据连接

截至 2021 年 4 月，TCRM 提供以下远程数据连接器：

| Salesforce | 应用程序 | 数据库 | Analytics Mulesoft | 对象存储和 No SQL | 实时连接 |
|---|---|---|---|---|---|
| 外部 Salesforce 组织 | Google Analytics | Amazon RDS (Amazon 关系数据库服务) | Microsoft SQL Server | Amazon S3 | Snowflake Direct |
| Marketing Cloud Contacts OAuth 2.0 | Microsoft Dynamics | Amazon Redshift | MySQL |  |  |
| Marketing Cloud Contacts | NetSuite | Google BigQuery - Legacy SQL and Standard SQL | Oracle |  |  |
| Customer 360 Global Profile Data | Oracle Eloqua | Google Cloud Spanner |  |  |  |
|  | Marketo | Heroku Postgres |  |  |  |
|  |  | Microsoft Azure SQL Data Warehouse |  |  |  |
|  |  | Microsoft Azure SQL Database |  |  |  |
|  |  | SAP HANA |  |  |  |
|  |  | Snowflake |  |  |  |
|  |  | Amazon RDS PostgreSQL |  |  |  |
|  |  | Amazon RDS SQL Server |  |  |  |
|  |  | Amazon RDS SQL MySQL |  |  |  |
|  |  | Amazon RDS SQL MariaDB |  |  |  |
|  |  | Amazon RDS SQL Aurora |  |  |  |
|  |  | Amazon RDS SQL Aurora PostgreSQL |  |  |  |

图 3.4 – 截至 2021 年 4 月 TCRM 可用的远程数据连接

Tableau CRM 平台持续扩展和发展。最新的连接器列表，请参阅 Salesforce 支持文档：

`https://help.salesforce.com/articleView?id=sf.bi_integrate_connectors_remote.htm&type=5`

TCRM 数据连接框架和工具并非没有局限性，下一节将对此进行讨论。

### TCRM 数据连接的局限性

你已经了解了 TCRM 在数据集成方面能够实现的功能。现在，我们来看看数据集成的一些局限性，如下所示：

- Salesforce 公式字段中存储的结果，只有在对相关对象执行完整复制后，才会在 TCRM 数据集中更新。
- 根据 Spring '21 版本，数据同步的当前上限为 100 个对象。
- 每个对象只能执行一次数据同步。如果你需要从同一个对象执行多次数据同步，这一点就很重要。

> 重要提示
> 在 TCRM 不具备所需 ETL 功能的情况下，可以使用 Tableau Prep Builder。更多信息请参阅 https://www.tableau.com/products/prep。

现在是时候使用 Salesforce 数据创建你的第一个自定义数据集了。

### 将 Salesforce 数据导入 TCRM

本节将详细介绍如何摄取 Salesforce 数据，并创建适用于分析和探索的数据集。在构建第一个数据集之前，你需要先考虑数据集的设计。

#### 设计数据集

TCRM 数据以倒排索引结构存储。理想情况下，你应从最低、最细粒度的层级（即子表）开始，沿着层级路径向上连接到父表，构建一个能够回答相关业务问题的数据集。你可以使用多个数据集并将其补充为一个大型数据集，也可以在仪表板编辑器中使用 SAQL 即时计算和汇总数据。

你必须理解数据集，并根据业务问题的需要来使用它。也就是说，设计数据集时，请考虑以下事项：

- 避免试图在一个数据集中构建数据仓库。
- 同时，数据集也不是单个表的替代品；它应包含与分析用例和相关业务问题有关的对象字段（例如数据表中的字段）。
- 始终考虑通过多个数据集互相补充来获得所需结果，或在仪表板中利用不同数据集之间相互作用的图表（每个图表都在最低粒度级别回答相应的业务问题）。可以根据需要将数据集连接到仪表板，以启用切面筛选。
- **SAQL（Salesforce Analytics Query Language）**也是解决某些用例的不错选择。

例如，假设企业想要分析产品对商机赢单率的影响。可用于分析的相关对象包括：

- Account
- Contact
- Opportunity
- User
- Product
- Product family

