
在下一节中，你将了解另一个名为 CloudWatch Logs 的功能。

## Amazon CloudWatch Logs

借助 CloudWatch Logs，你可以捕获、存储和监控日志数据。你可以从其他 AWS 服务捕获日志，例如 VPC Flow Logs、CloudTrail、Elastic Beanstalk、Lambda、IoT 设备以及本地服务器。你还可以存储 Route 53 DNS 日志请求。

CloudWatch Logs 还允许你从操作系统和应用程序捕获系统和应用程序日志数据，并将其导入 CloudWatch。对于此类日志数据，你必须安装 CloudWatch 代理或使用 AWS SDK、API 或 CLI。

你可以为日志文件定义过期策略，并对日志进行加密。你可以在 CloudWatch Logs 中查看日志数据，并将其发送到 Amazon S3、Kinesis Data Streams 和 Firehose。

*图 16.6* 中的图表说明了 CloudWatch 日志记录的工作原理。

![](img/34ff9b5b5a734dae3450ab701e223c73_669_0.png)

图 16.6 – CloudWatch Logs 架构

上图注明了 CloudWatch Logs 服务的各个组件。在此示例中，从三个与 Apache 访问日志数据相关的 EC2 实例捕获日志信息，并将其导入一个 CloudWatch Logs 日志组。

CloudWatch Logs 服务的核心组件包括以下部分：

- **日志事件：** 日志事件由时间戳和原始消息组成。事件消息必须采用 UTF-8 编码。日志文件中的每条记录称为一个日志事件。
- **日志流：** 一系列共享同一来源的日志事件。因此，在图 16.6 的示例中，你会看到三个日志流，分别对应每个正在捕获 Apache 日志访问文件数据的 EC2 实例。
- **日志组：** 对于给定的资源，你会有一个日志组，其中包含一个或多个日志流。在每个日志组中，日志流将共享相同的保留期、监控设置以及通过 IAM 角色和策略设置的任何访问权限。

使用 CloudWatch Logs 的一个重要好处是能够创建**指标筛选器**。这些筛选器允许你在日志流中的 CloudWatch Logs 事件里识别模式，然后从该筛选器创建一个指标并开始监控。例如，你可以使用指标筛选器提取并监控失败的登录或错误，一旦超过指定的阈值，就可以触发告警。

CloudWatch Logs 还附带其他功能，这些功能将在接下来的章节中进一步讨论。

### 导出和流式传输 CloudWatch Logs 数据

你还可以通过将日志数据发送到 Amazon S3 来从 CloudWatch 导出日志数据。这样可以长期保留日志数据，或供其他应用程序和系统用于分析和处理。此处，使用 `CreateExportTask` 操作导出日志数据。请注意，日志数据最多可能需要 12 小时才能用于导出。

除了将日志数据导出到 Amazon S3，你还可以使用订阅将 CloudWatch 日志数据配置为实时或近实时地处理并发送到其他目标。这些目标包括 Kinesis Data Streams、Firehose 或 AWS Lambda，可用于自定义处理或加载到其他 AWS 服务。你可以定义订阅筛选器，以筛选哪些日志事件被传送到目标资源。

### CloudWatch Logs Insights

CloudWatch Logs Insights 提供一种专用的查询语言，使你可以按照图 16.7 中的截图，以交互方式在 CloudWatch 中搜索和分析日志数据。

![](img/34ff9b5b5a734dae3450ab701e223c73_671_0.png)

图 16.7 – CloudWatch Logs Insights

CloudWatch Logs Insights 可以快速发现来自各种 AWS 服务（如 Lambda、CloudTrail 和 VPC Flow Logs）日志中的字段，或任何提供日志事件（如 JSON）的自定义日志中的字段。

你还可以使用自然语言创建 CloudWatch Logs Insights 查询。你可以询问或描述你要查找的数据。AI 辅助功能会根据你的提示生成查询，并逐行解释该查询的工作原理。

> **重要说明**
>
> CloudWatch Logs Insights 无法访问时间戳早于日志组创建时间的日志事件。

在下一节中，你将进一步了解 CloudWatch Events。

## CloudWatch Events（Amazon EventBridge）

CloudWatch Events 现已发展成为一个成熟的产品，并更名为 **Amazon EventBridge**。这项完全托管的服务使构建事件驱动型应用程序成为可能，让你能够进一步设计松耦合架构，使其真正分布式，并避免单点故障。

从高层来看，EventBridge 可以监控你的 AWS 账户中何时发生事件，然后做出响应。EventBridge 使用事件总线（event bus）的概念，它本质上是你账户中任何 AWS 服务和资源所发生事件的流。该事件总线监控状态变化，如果发生变化，它可以触发一个事件来调用目标。目标可以是另一个受支持的 AWS 服务，例如 Lambda 函数或发送到 SNS 主题的通知。

例如，当你的某个用户停止特定 EC2 实例时，EventBridge 可以触发向 SNS 主题发送通知，如图 16.8 所示。你可能需要收到此类事件的通知，例如用于审计目的。

![](img/34ff9b5b5a734dae3450ab701e223c73_672_0.png)

图 16.8 – 以 SNS 主题为目标的 EventBridge

使用 Cron Job 工具，你还可以配置 EventBridge，以安排在特定时间触发事件。例如，你可能希望触发一个事件来调用 Lambda 函数，在工作日结束时（下午 6 点）自动停止测试服务器。你可能还希望每天上午 9 点重新启动这些实例，并在每个工作日自动重复此操作。此外，你还可以配置事件按速率表达式发生，例如每 10 分钟触发一次操作。

Amazon CloudWatch 可以为你 AWS 账户及其资源的运行状况提供有价值的洞察。它提供广泛的日志记录、监控以及必要的修复操作能力。

在下一节中，你将了解另一项 AWS 服务 AWS CloudTrail，它帮助你从审计角度监控 AWS 账户，并确保合规性和安全性。

## AWS CloudTrail 简介

AWS CloudTrail 记录你 AWS 账户中的事件，提供用户操作、角色或 AWS 服务的详细记录。CloudTrail 捕获的每个事件都提供全面的信息，可用于安全分析、合规审计和运维故障排查。

你对 AWS 账户进行的每一次 API 调用，无论是通过管理控制台、CLI 还是使用 SDK，都会被记录在 AWS CloudTrail 中。这使你能够确定账户中发生了什么操作、由谁执行、何时执行，从而形成全面的审计跟踪。默认情况下，管理事件会被记录并保留 90 天。但是，你可以创建 CloudTrail 跟踪，将事件记录到 Amazon S3 存储桶并无限期保留。你还可以将捕获的事件发送到 CloudWatch Logs 和 Amazon EventBridge。

CloudTrail 跟踪可以为单个区域创建，也可以作为多区域跟踪创建。多区域跟踪捕获所有区域的事件，是推荐的最佳实践。如果 AWS 新建了一个区域，它会自动添加到你的多区域跟踪中，使你能够捕获全球基础设施中与资源相关的所有活动。使用 AWS Management Console 创建跟踪时，它会创建一个多区域跟踪。如果需要创建单区域跟踪，则必须使用 AWS CLI。

CloudTrail 可以捕获三种类型的事件，如下所示：

- **管理事件**提供有关对 AWS 账户中资源执行操作的信息，例如启动新的 EC2 实例、创建新的 IAM 角色、附加 IAM 策略或终止 RDS 数据库实例。默认情况下，跟踪配置为记录所有管理事件。
- **数据事件**提供有关对资源执行或在资源内部执行的资源操作的详细信息。例如，当用户尝试下载对象时，会对 S3 存储桶中的对象执行 GetObject 操作。你还可以在 Lambda 函数执行时捕获其执行活动，例如 Invoke API。
- **洞察事件**使你能够识别并响应由写 API 调用或读/写 API 的错误率引起的异常活动。CloudTrail Insights 通过分析正常 API 调用和 API 错误率来建立基线，并在这些调用量或错误率超出正常模式时生成事件。

管理 CloudTrail 事件的安全性至关重要，使用加密和 IAM 权限可以确保只有授权用户才能加密和解密 CloudTrail 日志文件。默认情况下，CloudTrail 使用带有 KMS 密钥的服务器端加密（SSE-KMS）将日志文件传送到你的存储桶，除非你未启用 KMS 加密。在这种情况下，CloudTrail 将使用 SSE-S3 加密日志文件。

请注意，要使用带有 S3 存储桶密钥的现有 S3 存储桶，你需要在密钥策略中授予 CloudTrail 执行 GenerateDataKey 和 DescribeKey AWS KMS 操作的权限。如果没有这些权限，你将无法创建或更新跟踪。

使用 KMS 进行加密的好处在于你可以自行管理密钥，并且可以使用同一密钥在所有区域的所有账户中加密和解密日志文件。要从 S3 存储桶访问日志文件，用户必须对该存储桶具有读取权限，并且应用了允许通过 KMS 密钥策略进行解密的策略或角色。这进一步增强了你的安全性。

在下一节中，你将学习如何使用 Amazon EventBridge 捕获特定 CloudTrail 事件并响应这些事件。
