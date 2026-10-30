

+   - **Internet access**: 你可以在没有直接互联网访问的情况下启动 Jupyter Notebook 服务器。它可以在私有子网中启动，并通过 NAT 或通过虚拟私有网关访问互联网。对于训练和部署推理容器，你可以在调用 `CreateTrainingJob`、`CreateHyperParameterTuningJob` 或 `CreateModel` 时将 `EnableNetworkIsolation` 参数设置为 `True`。网络隔离可以与 VPC 结合使用，确保容器无法进行任何出站网络调用。

+   - **Connecting a private network to your VPC**: 你可以在 VPC 的私有子网中启动 SageMaker 笔记本实例。它可以通过与私有网络通信来访问私有网络的数据，这可以通过使用 Amazon VPN 或 AWS Direct Connect 将私有网络连接到你的 VPC 来实现。

在本节中，我们学习了几种保护 SageMaker 笔记本的方法。在下一节中，我们将学习如何使用 Lambda 函数创建 SageMaker 管道。

## Creating alternative pipelines with Lambda Functions

确实，SageMaker 是一个很棒的平台，你可以用它来创建训练和推理管道。然而，我们总可以与其他服务配合，构建类似的解决方案。我们接下来要学习的一个服务就是 **Lambda Functions**。

AWS Lambda 是一种无服务器计算服务，你可以在其中将函数作为服务运行。换句话说，你只需专注于编写函数。然后，你只需告诉 AWS 如何运行它（即环境和资源配置），这样所有必要的资源将被配置以运行你的代码，并在完成后停止。

在 *Chapter 6, AWS Services for Data Processing* 整个章节中，你探索了 Lambda 函数如何与许多不同的服务集成，例如 Kinesis 和 AWS Batch。确实，AWS 在将 Lambda 与 140 个服务集成方面做得非常出色（并且该列表还在不断增加）。这意味着当你使用特定的 AWS 服务时，你会记得它很可能与 Lambda 集成。

这一点很重要，因为 Lambda 函数确实可以扩展你创建可扩展和集成架构的可能性。例如，当文件上传到 S3 时，你可以触发一个 Lambda 函数，以便在将数据加载到 Redshift 之前预处理数据。或者，你可以创建一个 API，在每个端点执行时触发 Lambda 函数。再次说明，使用这种强大的服务，可能性是无穷无尽的。

另外，有用的是，你可以用不同的编程语言编写函数，例如 Node.js、Python、Go、Java 等。你的函数不一定需要由另一个 AWS 服务触发，也就是说，你可以手动触发它，例如用于你的 Web 或移动应用程序。

在部署方面，你可以将函数作为 ZIP 文件或容器镜像上传。虽然这不适合自动化部署流程，但直接在 AWS Lambda 控制台中编码也是可能的。

与其他服务一样，这个服务也有一些缺点，你应该注意：

+   - Memory allocation for your function: 这是 128 MB 到 10,240 MB（AWS 最近将此限制从 3 GB 增加到 10 GB，如前所述）。
+   - Function timeout: 这是最多 900 秒（15 分钟）。
+   - Function layer: 这是最多 5 层。
+   - Burst concurrency: 这取决于区域，从 500 到 3,000 不等。
+   - Deployment package size: 这是 250 MB 未压缩，包含层。
+   - Container image code package size: 这是 10 GB。
+   - Available space in the /tmp directory: 这是 512 MB。

在选择 Lambda 函数之前，请确保这些限制适合你的用例。将 Lambda 函数更接近 SageMaker 替代管道的范围，Lambda 的一个潜在用途是为我们的模型创建推理管道。

正如你所知，SageMaker 有一个非常方便的 `.deploy()` 方法，它将为模型推理创建端点。这样你就可以调用它来传递输入数据以接收预测结果。在这里，我们可以使用 API 网关和 Lambda 函数创建此推理端点。

如果不需要推理端点，而只是想进行预测并以批处理方式将结果存储在某处，那么我们只需要一个 Lambda 函数即可，它能够获取输入数据、实例化模型对象、进行预测并将结果存储在适当的位置。当然，它会考虑我们之前讨论过的所有限制。

好了，现在我们对 Lambda 有了良好的背景知识以及一些用例，让我们来看看考试中我们应该注意的最重要的配置。

## Creating and configuring a Lambda Function

首先，你应该知道，你可以通过多种方式创建 Lambda 函数，例如通过 AWS CLI（Lambda API reference）、AWS Lambda 控制台，甚至部署框架（例如 serverless framework）。

无服务器框架通常与提供商和编程语言无关。换句话说，它们通常允许你从各种云提供商和编程语言的列表中选择部署无服务器基础设施的位置。

> **Important note**
> 无服务器架构的概念并非 AWS 特有。事实上，许多云提供商都提供与 AWS Lambda 函数类似的其他服务。这就是为什么构建了这些无服务器框架：帮助开发人员和工程师部署他们的服务，无论他们想要在哪里部署，包括 AWS。这不太可能出现在你的考试中，但这绝对是你应该知道的事情，以便你了解作为数据科学家或数据工程师解决挑战的不同方式。

由于我们希望通过 AWS Machine Learning Specialty 考试，这里我们采用了通过 AWS Lambda 控制台逐步操作的方法。这样你将更熟悉他们的界面和最重要的配置选项。

当你导航到 Lambda 控制台并请求新的 Lambda 函数时，AWS 会为你提供一些起始选项：

+   - Author from scratch: 如果你想从头创建函数。
+   - Use a blueprint: 如果你想从示例代码和常见用例的配置预设创建函数。
+   - Container image: 如果你想选择容器镜像来部署你的函数。
+   - Browse serverless app repository: 如果你想从 AWS Serverless Application Repository 部署示例 Lambda 应用程序。

从头开始，下一步是设置你的 Lambda 配置。AWS 将这些配置分为基本和高级设置。在基本配置中，你将设置函数名称、运行时环境和权限。图 9.14 显示了这些配置：

![](img/209e007237d34506e7ba16180f2e905c_311_0.png)

Figure 9.14 – Creating a new Lambda Function from the AWS Lambda console

这里有一个你在考试中应该记住的非常重要配置：**execution role**。你的 Lambda 函数可能需要权限来访问其他 AWS 资源，例如 S3、Redshift 等。执行角色授予你的 Lambda 函数权限，以便按需访问资源。

你必须记住，你的 VPC 和安全组配置也会影响 Lambda 函数的运行方式。例如，如果你想创建一个需要互联网访问来下载某些内容的函数，那么你必须在具有互联网访问的 VPC 中部署此函数。同样适用于其他资源，例如访问关系数据库、Kinesis、Redshift 等。

此外，为了正确配置 Lambda 函数，我们至少需要编写其代码、设置执行角色，并确保 VPC 和安全组配置符合我们的需求。接下来，让我们看看其他配置。

## Completing your configurations and deploying a Lambda Function

一旦你在 AWS 控制台中创建了 Lambda，你就可以在部署函数之前设置其他配置。其中一个配置是事件触发器。正如我们之前提到的，你的 Lambda 函数可以由各种服务触发，甚至可以手动触发。

> **Important note**
> 触发器的一个非常常见的例子是 **Event Bridge**。这是一个 AWS 服务，你可以在其中安排函数的执行。

