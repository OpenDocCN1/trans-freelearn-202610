

## 答案

+   1. B
+   2. B
+   3. A
+   4. A
+   5. B
+   6. C
+   7. B
+   8. B
+   9. A
+   10. B
+   11. A, C
+   12. D
+   13. D
+   14. B, C, E
+   15. A
+   16. D
+   17. A, C

## 6
用于数据处理的 AWS 服务

在上一章中，我们学习了在 AWS 中存储数据的几种方式。在本章中，我们将探讨使用这些数据并从中获得洞察的技术。存在需要处理数据或将数据加载到 Hive 数据仓库以便查询和分析数据的用例。如果您使用的是 AWS 且数据存储在 S3 中，那么您需要在 AWS EMR 上创建 Hive 表来查询它们。为了以托管服务的形式提供相同功能，AWS 提供了一个名为 Athena 的产品，您需要在其中创建数据目录并在 S3 上查询您的数据。如果需要转换数据，则 AWS Glue 是转换并将其恢复到 S3 的最佳选择。让我们设想一个用例，我们需要在其中流式传输数据并在此数据上创建分析报告。对于此类场景，我们可以选择 AWS Kinesis Data Streams 来流式传输数据并将其存储在 S3 中。使用 Glue，相同的数据可以复制到 Redshift 以供进一步分析。现在，让我们学习它们，我们将简要介绍以下内容。

- 使用 Glue 设计 ETL 作业
- 使用 Athena 查询 S3 数据
- 通过 AWS Kinesis Data Streams 流式传输数据并使用 Kinesis Firehose 存储数据

- 从本地位置摄取数据到 AWS
- 将数据迁移到 AWS 并将本地数据中心扩展到 AWS
- 在 AWS 上处理数据

## 技术要求

您可以在 GitHub 上下载示例中使用的数据，地址如下：https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-6。

## 在 AWS Glue 上创建 ETL 作业

在现代数据管道中，有多个阶段，例如生成数据、收集数据、存储数据、执行 ETL、分析和可视化。在本节中，我们将从高层次介绍这些内容并深入了解 ETL（提取、转换、加载）部分：

- 数据可以从多种设备生成，包括移动设备或物联网、网络日志、社交媒体、交易数据、在线游戏等。
- 这种海量的生成数据可以通过轮询服务收集，也可以通过与 AWS Lambda 集成的 API 网关收集数据，或通过流式服务如 AWS Kinesis 或 AWS 管理的 Kafka 或 Kinesis Firehose 进行收集。如果您有本地数据库并希望将该数据收集到 AWS，则选择 AWS DMS。您可以通过 AWS DataSync 将本地数据同步到 Amazon S3、Amazon EFS 或 Amazon FSx。AWS Snowball 用于将数据收集/传输到 AWS 以及从 AWS 传输。
- 下一步涉及存储数据，我们在上一章中学习了一些服务，例如 AWS S3、EBS、EFS、Amazon RDS、Amazon Redshift 和 DynamoDB。
- 一旦我们了解了数据存储，就可设计 ETL 作业来提取-转换-加载或提取-加载-转换我们的结构化或非结构化数据到我们期望的格式以供进一步分析。例如，我们可以考虑使用 AWS Lambda 在飞行中转换数据并将转换后的数据存储到 S3，或者在 EMR 集群上运行 Spark 应用程序来转换数据并将其存储到 S3 或 Redshift 或 RDS。
- AWS 中有许多服务可用于对转换后的数据进行分析；例如，Amazon EMR、Amazon Athena、Amazon Redshift、Amazon Redshift Spectrum 和 Kinesis Analytics。

- 一旦数据被分析，您可以使用 AWS Quicksight 可视化数据以了解模式或趋势。数据科学家或机器学习专业人员会喜欢应用统计分析以更好地理解数据分布。业务用户使用它来准备报告。我们已经在 *第 4 章，理解和可视化数据* 中学习了各种呈现和可视化数据的方法。

我们从传统数据管道中了解到，ETL 完全是关于在服务器上编写和维护代码以使其顺利运行。如果数据格式以任何方式发生变化，则需要更改代码，这会导致目标模式发生变化。如果数据源发生变化，则代码也必须能够处理这种情况，这是一个开销。*我们是否需要编写代码来识别数据源中的这些变化？我们是否需要一个系统来适应变化并为我们发现数据？* 答案是 **AWS Glue**。现在，让我们了解为什么 AWS Glue 如此著名。

## AWS Glue 的功能

AWS Glue 是 AWS 上的完全托管的无服务器 ETL 服务。它具有以下功能：

- 它通过连接到数据源自动发现和分类您的数据并生成数据目录。
- Amazon Athena、Amazon Redshift 和 Amazon EMR 等服务可以使用数据目录查询数据。
- AWS Glue 生成 ETL 代码，该代码是 Python 或 Scala 中 Spark 的扩展，也可以进行修改。
- 它会自动扩展以匹配运行 ETL 作业并将数据加载到目标所需的 Spark 应用程序要求。

AWS Glue 具有 **数据目录**，这就是它成功的秘诀。它有助于从数据源中发现数据并让我们对其有所了解：

- 数据目录自动发现新数据并提取模式定义。它检测模式更改并对表进行版本控制。它检测 Amazon S3 上的 Apache Hive 风格分区。
- 数据目录带有流行数据类型的内置分类器。可以使用 **Grok 表达式** 编写自定义分类器。分类器有助于检测模式。

- Glue 爬虫可以按需或按计划运行以更新 Glue 数据目录中的元数据。Glue 爬虫必须与具有足够访问权限以读取数据源的 IAM 角色相关联，例如 Amazon RDS、Amazon Redshift 和 Amazon S3。

既然我们对 AWS Glue 有了初步了解，让我们运行以下示例来动手实践。

## 动手实践 AWS Glue 数据目录组件

在本示例中，我们将创建一个作业，使用 AWS Glue 将数据从 S3 复制到 Redshift。我的所有组件都创建在 us-east-1 区域。让我们开始创建存储桶：

1. 导航到 AWS S3 控制台并创建存储桶。我将存储桶命名为 aws-glue-example-01。
2. 点击 Create Folder 并将其命名为 input-data。
3. 进入文件夹内部并点击 Upload 按钮上传 sales-records.csv 数据集。数据可在以下 GitHub 位置获得：https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-6/AWS-Glue-Demo/input-data。
由于我们已将数据上传到 S3 存储桶，让我们创建一个 VPC，在其中创建我们的 Redshift 集群。
4. 通过访问 https://console.aws.amazon.com/vpc/home?region=us-east-1# URL 导航到 VPC 控制台，并在左侧菜单中点击 Endpoints。点击 Create Endpoint，然后按如下所示填写字段：
   a) Service Category: AWS services
   b) Select a service: com.amazonaws.us-east-1.s3 (Gateway type)
   c) VPC: 选择默认 VPC（我们将使用此默认 VPC，在其中创建我们的 Redshift 集群）
5. 保持其他字段不变并点击 Create Endpoint。

6. 从 VPC 控制台点击 **Security Groups**。为您的安全组命名，例如 `redshift-self`，并选择默认 VPC 下拉菜单。提供 Redshift 安全组的适当描述。点击 **Create security group**。

7. 点击 **Actions** 下拉菜单并选择 **Edit Inbound rules**。点击 Add rule 并按如下所示完成字段：

   a) **Type:** `All traffic`
   b) **Source:** Custom
   c) 在搜索字段中，选择相同的安全组（`redshift-self`）

8. 点击 **Save Rules.**

现在，让我们创建我们的 Redshift 集群。

9. 导航到 Amazon Redshift 控制台。点击 **Create Cluster** 并完成图 6.1 中显示的高亮字段：

![](img/209e007237d34506e7ba16180f2e905c_197_0.png)

图 6.1 – Amazon Redshift 的创建集群截图

