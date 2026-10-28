

我们将在活动栏中选择 **Azure** 图标。接下来，在 **AZURE** 区域中，我们将选择 **Create new project**：

![](img/66aceec30d3e7dead2f71bb68a887688_113_0.png)

我们将在选择运行时堆栈后为新的函数应用输入一个唯一名称，我们的示例中为 .NET 6。之后，我们将选择位置然后选择资源组。

我们也可以创建一个本地项目并将其发布到 Azure。要做到这一点，我们使用相同的 **Azure** 图标并选择 **Create new project**。之后，我们需要提供在提示中要求的所有信息：

- **Select a language:** 选择 C#
- **Select a .NET runtime:** 选择 .NET 6
- **Select a template for your project’s first function:** 选择 **Http trigger**
- **Provide a function name:** 输入 `HttpExample1`
- **Provide a namespace:** 输入 `Functions1`
- **Authorization level:** 选择 **Anonymous**，这样任何人都可以调用你的函数端点
- **Select how you would like to open your project:** 选择 **Add to workspace**

如果我们需要执行该函数，我们将进入 AZURE 区域，并展开 Local Project | Functions。右键单击函数名称并选择 Execute Function Now...：

![](img/66aceec30d3e7dead2f71bb68a887688_114_0.png)

图 4.18 – 执行函数功能

之前，我们使用 Visual Studio 2022 和 Visual Studio Code 创建了 Azure Functions 应用，但我们也可以使用 Azure 门户创建 Azure 函数并添加和运行多个函数而无需实现应用程序。

## 在 Azure 门户中创建 Azure Functions 应用

我们可以使用不同的工具创建 Azure Functions 应用。在本节中，我们将使用 Azure 门户创建 Azure Functions 应用。

要创建 Azure Functions 应用，我们将打开 Azure 门户并选择与 Azure Functions 相关的资源。以下图展示了不同的配置设置：

![](img/66aceec30d3e7dead2f71bb68a887688_115_0.png)

图 4.19 – 创建 Function App 窗口

我们将填写创建函数应用所需的所有信息，如下表所示（见图 4.18）：

| Setting | Description |
|---|---|
| Subscription | 用于创建新函数应用的订阅。 |
| Resource group | 可以创建新的资源组或选择现有资源组。 |
| Function app name | 用于标识函数应用的名称。可使用的字符为 a-z（不区分大小写）、0-9 和 -。 |
| Publish | 选择代码文件或 Docker 容器。 |
| Runtime stack | 选择将支持函数的运行时，例如 C# 类库、C# 脚本、JavaScript、PowerShell、Java 和 TypeScript。 |
| Version | 已安装的运行时版本。 |
| Region | 选择靠近你的区域或函数将访问的其他服务所在的区域。 |
| Plan type | 我们有三种计划：Consumption plan、Premium plan 和 Dedicated plan。如果需要有关这些计划的更多详细信息，可以查看此链接：`https://learn.microsoft.com/en-us/azure/azure-functions/functions-scale`。我们将使用 Consumption plan，仅为函数应用执行付费。 |

表 4.1 – 创建 Azure Functions 应用的设置和描述

当函数应用创建后，我们将检查资源组。以下资源被添加到资源组中：

- Application Insights
- Storage account
- Function app
- App Service plan

在下图中，我们将在 **Functions** 部分下选择 **Functions**：

![](img/66aceec30d3e7dead2f71bb68a887688_117_0.png)

图 4.20 – Functions 设置

## 在 Azure 中创建和部署函数应用

我们将选择一个现有的函数应用，但我们可以添加更多函数：

![](img/66aceec30d3e7dead2f71bb68a887688_118_0.png)

图 4.21 – 选择或创建函数

我们只能使用门户来配置 Azure Functions。我们可以在函数被触发时使用输出绑定通过 HTTP 请求创建一个队列中的消息：

![](img/66aceec30d3e7dead2f71bb68a887688_118_1.png)

图 4.22 – 创建新函数

选择 Code + Test。我们有 function.json，要运行它，我们必须选择 Test/Run：

![](img/66aceec30d3e7dead2f71bb68a887688_119_0.png)

图 4.23 – 测试和运行函数应用

选择 Integration，然后选择 + Add output：

![](img/66aceec30d3e7dead2f71bb68a887688_119_1.png)

图 4.24 – Integration

我们可以通过选择 **Add input** 添加输入绑定。输入可以是 **Azure Blob Storage, Azure Cosmos DB, Azure Table Storage, Durable Client, Orchestration Client,** 或 **SignalR Connection Info**：

![](img/66aceec30d3e7dead2f71bb68a887688_120_0.png)

图 4.25 – 添加输入绑定

对于输出绑定，我们可以选择更多类型：**Azure Blob Storage, Azure Cosmos DB, Azure Event Hubs, Azure Queue Storage, Azure Service Bus, Azure Table Storage, HTTP, Kafka Output, SendGrid, SignalR,** 和 **Twilio SMS**。

![](img/66aceec30d3e7dead2f71bb68a887688_120_1.png)

图 4.26 – 添加输出绑定

在本节中，我们在 Azure Functions 应用中添加了输入和输出绑定。

在常规函数和持久函数之间进行选择时，决策的关键因素之一是你的应用程序是否实现了顺序的多步骤工作流。如果你的应用中存在依赖于先前步骤或特定状态来触发下一步的功能，持久函数可以使这些工作流的多个棘手部分变得容易得多。那么，持久函数是什么，我们如何开发它们？

## 开发持久函数

在本节中，我们将描述用于持久函数的各种类型和模式，并将解释持久编排、计时器和事件的使用。Azure Durable Functions 是 Azure Functions 的扩展，用于编写有状态函数。Durable Functions 支持多种语言，如 C#、JavaScript、Python、F# 和 PowerShell。

本章后面我们将描述可以从持久函数中受益的常见应用程序模式：

- Function chaining
- Fan-out, fan-in
- Asynchronous HTTP APIs
- Monitor
- Human interaction

现在我们已经定义了持久函数并介绍了使用的不同模式，我们将更详细地描述持久函数以及需要其使用的不同场景。

## 持久函数简介

为了使用无服务器函数实现更复杂的场景，我们需要一种机制来允许我们在这些函数执行期间保存状态。

此函数扩展允许我们编写有状态函数并定义其他函数的编排。它自动处理状态、检查点和重启。使用 Azure Durable Functions 在我们的项目中有几个好处。

我们将首先构建一个有状态的工作流，以克服平台固有的无状态特性。接下来，作为编排器的已定义函数可以同步或异步调用其他函数。由编排器调用的函数的结果可以保存为局部变量。编排器和其余函数的全部进度都存储在本地。我们必须确保永远不会丢失状态。这是平台基本功能的一部分。当活动函数以粗体显示时，你可以指定自动重试策略。被视为编排器的函数可以是 Service Orchestration、Resource Orchestration 和 Lifecycle Orchestration。函数之间存在依赖关系和连续性。你还可以并行运行编排器函数的多个实例。编排器函数可以有多个状态，例如 Started、Consulted 或 Canceled。最后，Azure Durable Functions 扩展由本地 Azure 函数和你计算机上已有的 SDK 开发工具支持。

## 实现编排器函数

因此，在此 FunctionAppSample 项目中，我们将右键单击并选择 Add | New Azure Function... 选项并将其命名为 FunctionOrchestrator：

![](img/66aceec30d3e7dead2f71bb68a887688_122_0.png)

现在，我们将使用 Durable Functions Orchestration 模板，如以下屏幕截图所示：

![](img/66aceec30d3e7dead2f71bb68a887688_122_1.png)

