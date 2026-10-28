# 第7章 云服务与部署

### 8.1 基本概念

常微分方程：含有未知函數、未知函數的導數（或微分）與自變數的方程式，稱之為常微分方程。若未知函數為多變數函數，且方程式含有偏導數，則稱為偏微分方程。

定義：n 階常微分方程之形式為

$$
F(x, y, y', \ldots, y^{(n)}) = 0
$$

- 可使用多种常见的本地工具和服务，这些工具和服务既可在现场使用，也可在云中使用；例如，使用可在本地和云中工作的排队和 SQL 服务器。
- 通过 WebMatrix 等多种工具快速在云中开发网站。
- 按需服务可用性，仅使用您需要的服务，以及 Microsoft 工具来帮助报告您的资源和利用率以供计费验证。
- 随着软件模型的发展和需求的变化，能够迁移到其他云服务。许多在 Azure 中开发的业务软件应用程序可以在其他云服务中使用。
- 大数据资源报告和配置中简单的数据存储模型可用性。

所有这些功能总结起来就是 Microsoft 允许您使用 Microsoft 工具在本地或云中开发系统。它不会强迫您留在云中或在本地开发，而是允许您在认为合适的部分在云中开发，并在认为合适时将其从云中移除。许多其他云厂商可能会强迫您使用专用工具创建解决方案，当您将其移出云时，必须完全重写以放入不同的云服务。

### 介绍 Azure 存储服务

对于数据管理，我们可以在云中创建 SQL Azure 数据库，或在本地创建 SQL 服务器，或两者混合。一种更简单的方法是直接调用存储服务，从而提供直接映射到数据的行。Azure 存储服务既可通过使用 Azure SDK 在本地使用，也可在 Azure 云中使用。

Azure 存储可用于在表中存储 Saga 数据、在表中存储超时持久化、在表中存储订阅信息，以及在队列中传输消息。

除了为在云中存储数据提供 SQL Azure 外，Azure 还提供以下其他类型的存储，其中有三种基本类型：

- **Tables**：这基于键值 NoSQL 表格式。
- **Binary Large Object (BLOB)**：这用于二进制存储，例如视频文件。
- **Queues**：这用于存储消息。

表是 NoSQL 解决方案，而不是关系型 SQL 数据库。它可以在多台机器上存储数据。每个表可以在多台机器上包含分区。这些表具有实体、分区和行键来访问实体。它是用于存储数据的键值存储。这些表不强制执行模式。通过不使用模式，速度得到增强，并且表与对象松散耦合，因为它们由键管理。限制是无法执行 SQL 查询，但在大多数情况下，它可以占用更少的存储，从而降低数据成本。对于表，有分区键、行键和时间戳。

- 分区键：这是与分区相关联的唯一键，作为所有相关行的集合。这用于定义要访问哪个分区；例如，表的名称。
- 行键：这是用于在分区中标识行的唯一键，通常是唯一 ID。
- 时间戳：这是 Azure 更新该行的时间。

BLOB 是一个容器组，只是作为二进制存储在数据存储中存储的非结构化数据，例如视频或音频文件。

队列存储与在 MSMQ 中存储消息非常相似，只是管理工具位于 Azure 云中。就像许多 Azure 云项目可以通过 Azure SDK 和 Visual Studio 管理一样，存储队列也可以通过 Visual Studio 管理。我们在 Visual Studio 中看到超时状态、Saga 数据和订阅数据，如以下屏幕截图所示：

![](img/8188651cea6a881a16242b395a81399a_191_0.png)

除了使用 Visual Studio 外，还有许多开源工具，例如可在以下位置找到的 Azure Storage Explorer：

```
http://azurestorageexplorer.codeplex.com/
```





Azure 存储资源管理器显示在以下屏幕截图中：

![](img/8188651cea6a881a16242b395a81399a_192_0.png)

## Azure 服务总线和存储队列

NSB 同时支持在 Azure 存储队列和 Azure 服务总线队列中进行排队。

请注意，使用 Azure 存储队列与使用 Azure 服务总线队列之间存在许多差异。

> 请注意此示例中的不同名称。服务总线队列提供更多管理功能，例如保证 FIFO，而 Azure 队列的内置可管理性较低。访问 http://msdn.microsoft.com/en-us/library/hh767287.aspx 以获取更多信息。

当我们运行客户端时，我们将让 MVCApp 网站通过 saga 向 WCF 服务器上的 WCF 客户端发送一条消息。如下所示：

![](img/8188651cea6a881a16242b395a81399a_196_0.png)

我们还将让 MySaga 和 WCF 客户端同时运行，以便在 Azure 模拟器中创建队列和持久化。在 Visual Studio 2012 中，通过转到“视图 | 服务器资源管理器 | Azure | 存储”，我们可以查看正在运行的队列和消息，如下所示：

![](img/8188651cea6a881a16242b395a81399a_196_1.png)

可以在 Azure 门户中创建 Azure 服务总线，方法是首先创建服务总线命名空间，如下面的屏幕截图所示：

![](img/8188651cea6a881a16242b395a81399a_198_0.png)

我们可以在 Azure 门户中创建服务总线队列、主题和中继，并且在 Azure 门户中初始创建后，还可以通过 Visual Studio 管理服务总线。访问服务总线需要使用主密钥和连接字符串，这与 SQL 数据库的连接字符串并无太大不同。

![](img/8188651cea6a881a16242b395a81399a_198_1.png)

在创建 Azure 服务总线队列时，我们可以在此创建过程中看到一些附加配置设置，这些设置在存储队列中并未提供，如下面的屏幕截图所示：

![](img/8188651cea6a881a16242b395a81399a_199_0.png)

例如，我们还可以使用来自以下位置的 Azure 云服务总线队列的 NServiceBus 示例：

https://github.com/Particular/NServiceBus.Azure.Samples/tree/master/VideoStore.AzureServiceBus.Cloud

我们将按以下方式设置 NSB 以运行 Azure 存储队列：

- 1. 使用 PM> Install-Package NServiceBus.Azure.Transports.WindowsAzureServiceBus 从 NuGet 或项目的包管理器中为 NServiceBus 安装引用。

> 您可能需要将 NServiceBus 核心版本的兼容版本与相应的 NServiceBus Azure 版本进行匹配。一种想法是卸载当前的 NServiceBus 引用，让 NServiceBus Azure 引用安装所需的 NServiceBus 核心引用。

- 2. 在 IBus 配置上设置配置。在 MySaga 项目和 EndpointConfig.cs 文件中，我们按如下方式使用 AzureServiceBus 作为队列：

```
Configure.With()
.DefaultBuilder()  // Autofac Default Container
.UseTransport<NServiceBus.AzureServiceBus>()  // Azure Storage Queues
.AzureSubscriptionStorage()
.AzureSagaPersister()
.UseAzureTimeoutPersister()
.UnicastBus(); // Create the default unicast Bus
```

> 我们还设置了 AzureSagaPersisterConfig、AzureTimeoutPersisterConfig、AzureSubscriptionStorageConfig 配置，以仍然使用 Azure 存储进行数据库设置。我们还为所有项目添加了 AzureSubscriptionStorageConfig 配置。

我们需要根据需要调整项目设置，以将传输设置为 AzureServiceBus。

现在，我们已经将配置更改迁移到使用 AzureServiceBus。当我们运行应用程序时，可以从 Azure 管理门户查看所有队列，如下所示：

![](img/8188651cea6a881a16242b395a81399a_203_0.png)

我们在 Visual Studio 服务器资源管理器中查看连接：

其中有主题、发布-订阅消息传递，以及用于一对一消息的队列。

我们现在在 NSB 中运行着一个 Azure 服务总线。

## 总结

在本章中，我们深入探讨了**软件即服务（SaaS）**以及 NServiceBus 如何与云计算相关联。我们对云及其部分服务做了非常简要的介绍。我们讨论了 Azure 存储服务和 Azure 服务总线在提供持久化和消息队列方面的应用。Visual Studio 提供了许多工具，可用于构建本地部署和 Azure 云中的应用程序。我们在每种环境中都演练了一个示例，在示例中我们将所有超时、saga 和订阅信息都存储在 Azure 中。

- ESB 10-12
- ESB 设计
- 实际需求 13
- 事件驱动任务 14
- 可扩展应用程序标记语言 (XAML)
- URL 146
- 可扩展应用程序标记语言 (XAML)，使用
  - 用于电子邮件 saga 通知 148-150
  - 用于 SFTP saga 160

## G

- 网关模式
  - 关于 18
  - 源代码 19

## I

- 基础设施即服务 (IaaS) 175

## J

- Java 消息服务 (JMS) 139, 168
- Java 运行时环境 (JRE) 139
- JavaScript 对象表示法 (JSON) 62

## L

- 语言集成查询 (LINQ) 94

## M

- 消息流 48-55
- 消息加密模式
  - 关于 30
  - 源代码 30, 31
- 消息变更模式
  - 关于 28
  - 源代码 29
- Microsoft Azure
  - 关于 177, 178
  - 特性 177, 178
- Microsoft Azure PowerShell 工具
  - URL 177
- Microsoft 实体框架 (EF) 94
- Microsoft 消息队列 (MSMQ) 12
- Microsoft 任务计划程序 22
- 模型-视图-控制器 (MVC) 94

## N

- NSB
  - 关于 9
  - 与 Azure 存储队列 181-185
  - 与云 173, 174
  - Azure 服务总线 185-193
  - 优点 12
  - 组件 59
  - 主机，URL 71
- NSB 托管
  - URL 56
- NSB 版本 4
  - 升级到 5 39
  - 从 39, 41 升级到 5
- NSB 版本 5
  - 特性 39-41
- NServiceBus。参见 NSB
- 将 NServiceBus 添加到 MVC
  - 关于 120-122
  - 执行集成测试 130-134
  - 消息处理器单元测试 123-125
  - saga 处理器单元测试 128-130
- NServiceBus.RabbitMQ 包 138

## O

- 对象关系映射器 (ORM) 95
  - 生成样板代码 95
  - 加速开发 95
  - 支持面向对象 95

## P

- PaaS 175
- 模式
  - DataBus 模式 21, 22
  - 网关模式 18
  - 消息加密模式 30
  - 消息变更模式 28
  - 发布-订阅模式 15, 16
  - 请求-响应模式 17
  - saga 设计模式 32
  - ScaleOut 模式 32
  - 超时模式 22-28

## T

- 超时模式 22-28

## V

- 虚拟机 (VM) 176

## W

- WCF
  - 关于 96
  - 源代码 96
- WCF 客户端
  - 创建 111
  - 重新审视设计 116-118
  - 添加服务引用 112-114
  - 调用服务引用 114-116
  - 源代码 118, 119
- WCF 服务器
  - 添加配置 103-105
  - 部署注意事项 110
  - 创建 97-99
  - 添加消息处理器 102
  - 添加消息 100, 101
  - 添加跟踪 106-109
  - 查看 Web 服务 110
- Web 服务定义语言 (WSDL) 10
- Web 服务描述语言 (WSDL) 96
- Windows Communication Foundation (WCF) 13
- Windows Presentation Foundation (WPF) 145, 146
- Windows 任务计划程序 14

![](img/8188651cea6a881a16242b395a81399a_211_0.png)

## 感谢您购买《Learning NServiceBus Sagas》

### 关于 Packt Publishing

Packt，发音为 'packed'，于 2004 年 4 月出版了第一本书《Mastering phpMyAdmin for Effective MySQL Management》，此后一直专注于出版针对特定技术和解决方案的高度聚焦的书籍。

我们的书籍和出版物分享 IT 同行在调整和定制当今系统、应用程序和框架方面的经验。我们的基于解决方案的书籍为您提供定制软件和技术所需的知识和能力，以完成工作。Packt 的图书比您过去见过的 IT 图书更具体、更不那么笼统。我们独特的商业模式使我们能够为您带来更聚焦的信息，让您获得更多需要知道的内容，减少不需要知道的内容。

Packt 是一家现代而独特的出版公司，专注于为开发者、管理员和初学者等群体制作高质量、前沿的书籍。有关更多信息，请访问我们的网站：www.packtpub.com。

### 关于 Packt Enterprise

2010 年，Packt 推出了两个新品牌：Packt Enterprise 和 Packt Open Source，以继续专注于专业化。本书是 Packt Enterprise 品牌的一部分，该品牌出版有关企业软件的书籍——这些软件由主要供应商创建，包括（但不限于）IBM、Microsoft 和 Oracle，通常供其他公司使用。其书籍将为该软件的各种用户提供相关信息，包括管理员、开发人员、架构师和最终用户。

### 为 Packt 撰稿

我们欢迎所有有兴趣撰写书籍的人士前来咨询。图书提案应发送至 `author@packtpub.com`。如果您的图书创意仍处于早期阶段，并且希望在撰写正式图书提案之前先进行讨论，请与我们联系；我们的委任编辑之一将与您联系。

我们不仅仅寻找已出版过书籍的作者；如果您拥有扎实的技术技能但没有写作经验，我们经验丰富的编辑可以帮助您发展写作事业，或者仅仅为您的专业知识获得一些额外的回报。





![](img/8188651cea6a881a16242b395a81399a_212_0.png)

![](img/8188651cea6a881a16242b395a81399a_212_1.png)

## Learning NServiceBus

ISBN: 978-1-78216-634-4      Paperback: 136 pages

使用业界领先的 .NET 企业服务总线构建可靠且可扩展的分布式软件系统

- 1. 用可靠的流程替代批处理作业。
- 2. 创建能够补偿系统故障的应用程序。
- 3. 构建面向消息的系统。

![](img/8188651cea6a881a16242b395a81399a_212_2.png)

## Multithreading in C# 5.0 Cookbook

ISBN: 978-1-84969-764-4      Paperback: 268 pages

超过 70 个配方，帮助你快速高效地学习使用 C# 5.0 进行异步和并行编程

- 1. 深入 .NET 线程基础设施，并使用任务并行库进行异步编程。
- 2. 有效地扩展你的服务器应用程序。
- 3. 精通 C# 5.0 异步操作语言支持。

请访问 www.PacktPub.com 获取我们图书的详细信息

![](img/8188651cea6a881a16242b395a81399a_213_0.png)

## CryENGINE Game Programming with C++, C#, and Lua

ISBN: 978-1-84969-590-9      Paperback: 276 pages

掌握开发游戏所需的必备工具，使用功能强大且出色的 CryENGINE

- 1. 深入 CryENGINE 的各个子系统，快速学习如何掌握该引擎。
- 2. 在 CryENGINE 中使用 C++、C# 或 Lua 创建你自己的游戏。
- 3. 理解引擎的结构和设计。

![](img/8188651cea6a881a16242b395a81399a_213_1.png)

## Enterprise Integration with WSO2 ESB

ISBN: 978-1-78328-019-3      Paperback: 92 pages

超过 15 个配方，用于校准 SOA 的无缝模块化，并以零代码方式解决常见的企业集成挑战

- 1. 学习如何使用 WSO2 ESB 实现最常用的企业集成模式。
- 2. 了解如何将 WSO2 ESB 与 FIX、HL7 和 SAP 网关集成。
- 3. 理解 WSO2 ESB 背后的关键概念，并找到在生产环境中部署 WSO2 ESB 的优化建议。

