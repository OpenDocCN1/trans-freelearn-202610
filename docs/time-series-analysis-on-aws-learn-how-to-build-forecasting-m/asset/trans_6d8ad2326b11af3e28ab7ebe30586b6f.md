

## 定价

与许多 AWS 服务一样，您只需按使用量付费，无需预先承诺。尽管构建概念验证的成本极低（尤其是如果您能利用免费套餐），但使用 Amazon Lookout for Metrics 实现异常检测管道只需考虑一个维度：检测器在给定月份分析的指标数量。无论您的指标分析频率如何，您都需要付费：无论您的指标是每小时还是每天发布一次数据，您都只需为单个指标付费。

如果这是您首次在某个账户中使用 Amazon Lookout for Metrics，您可以享受免费套餐，该套餐允许您免费使用该服务一个月。在此期间，分析最多 100 个指标将不收取任何费用。

AWS 服务开发人员不懈努力降低服务的运营成本，价格也会定期下调。在撰写本书时，您的 Amazon Lookout for Metrics 使用量按以下分级价格计费：

- 前 1,000 个指标：每个指标 $0.75
- 从 1,001 到 5,000 个指标：每个指标 $0.50
- 从 5,001 到 20,000 个指标：每个指标 $0.25
- 从 20,001 到 50,000 个指标：每个指标 $0.10
- 超过 50,000 个指标后的每个指标按 $0.05 计费。

如需了解最新定价，您可以访问 Amazon Lookout for Metrics 定价页面：https://aws.amazon.com/lookout-for-metrics/pricing/。

请注意，根据您使用 Amazon Lookout for Metrics 的方式，可能会产生其他费用。例如，如果您执行以下任何操作，就会产生费用：

- 使用 Amazon SNS 创建警报。
- 使用 AWS Lambda 函数创建自定义操作。
- 使用 AWS AppFlow 将数据导入 Lookout for Metrics 数据集。

有关这些服务定价模式的更多详细信息，请查看它们各自的定价页面：

- Amazon SNS：https://aws.amazon.com/sns/pricing/
- AWS Lambda：https://aws.amazon.com/lambda/pricing/
- AWS AppFlow：https://aws.amazon.com/appflow/pricing/

此定价概述结束了我对 Amazon Lookout for Metrics 的介绍。在下一节中，您将了解您的应用程序需要满足哪些不同要求，才能成功利用此服务的功能。

## 识别适合监控的指标

您已成功将机器学习项目定义为异常或离群值检测问题，并且可能已经收集了一些历史时间序列数据集。Amazon Lookout for Metrics 是否是提供您所需洞察的理想选择？让我们回顾一些注意事项，以帮助您了解 Amazon Lookout for Metrics 是否适合您的异常检测场景，具体如下：

- 数据集要求
- 用例要求

### 数据集要求

与许多其他 AI 服务（无论是否在 AWS 内）不同，Amazon Lookout for Metrics 可以在没有任何数据点的情况下启动：在获得足够信息执行冷启动检测之前，您不会收到任何检测到的异常（例如，如果您的数据按日间隔，您需要等待至少 14 天才能开始接收来自 Amazon Lookout for Metrics 的推理结果），但您可以开始构建管道和应用程序，而无需像其他 AI 服务那样等待足够的数据来训练模型。

此外，Amazon Lookout for Metrics 在底层利用了多种算法（从简单的统计模型到更依赖数据的深度学习模型），当您拥有足够的数据时，服务会自动从简单算法切换到更复杂的算法。

Amazon Lookout for Metrics 是一种单变量服务：每个指标单独监控，每个检测器最多可监控五个指标。每个检测器都会生成可操作的结果，并提供根因分析，帮助您在异常发生时发现这些指标之间的关系。

如果您需要分析超过五个指标，则需要构建多个检测器。如果您拥有多变量数据集，并且知道异常可能基于多个变量之间不断演变的关系而产生，那么 Amazon Lookout for Metrics 将无法捕获此类事件。

### 用例要求

- **使用日志文件作为输入：** 尽管日志文件高度基于时间，但将自由格式的文本日志文件用作 Lookout for Metrics 的数据源并不是一个好的选择。您需要预处理数据，并将其结构化为 Amazon Lookout for Metrics 可识别的输入格式（例如，位于 Amazon S3 中的 CSV 或 JSON Lines 文件），才能检测到某些内容。您还需要将自由格式文本预处理为易于处理的事件：例如，您可能能够捕获诊断代码来替代自由格式文本。
- **异常预测：** Amazon Lookout for Metrics 可以检测实时数据中的异常。但是，它无法通过根据近期模式预测异常可能发生的时间来提供异常预测。
- **随时间累积的异常：** 某些异常会逐渐出现并随时间推移而变得重要。Amazon Lookout for Metrics 可以检测在数小时或数天内发生的异常。如果您监控的指标在数周甚至数月内逐渐恶化，该服务将难以捕捉这种行为。

您现在应该对如何构建异常检测问题以利用 Amazon Lookout for Metrics 有了很好的理解。您还可以使用本节来识别如何丰富现有数据集以成功使用此服务（例如，在数据准备期间将自由格式文本文件转换为诊断代码，或以合适的时间间隔聚合数据）。

## 在 Lookout for Equipment 和 Lookout for Metrics 之间进行选择

本书第一部分专门介绍 Amazon Forecast，第二部分则聚焦于 Amazon Lookout for Equipment。本章帮助您开启了 Amazon Lookout for Metrics 之旅。除了这三种 AI/ML 托管服务之外，AWS 云平台还提供了许多选项来处理您的时间序列数据并从中获取有价值的洞察。以下思维导图以细粒度级别展示了这些不同选项：

![](img/e4ee48104c730845e796d8c469dfeecc_388_0.png)

图 13.2 – AWS 上时间序列的简化思维导图

现在您已经对前面提到的三种托管服务有了或多或少的深入了解，让我们看看有助于您在这三者之间进行选择的不同决策点：

1. 如果您拥有*多变量*数据集，并希望执行*异常检测*或预测，请使用 **Amazon Lookout for Equipment**。
2. 如果您拥有*单变量*或*多个时间序列*，请执行以下操作：
   A. 如果您想*预测*业务指标，请使用 **Amazon Forecast**。
   B. 如果您想执行*异常检测*（非预测），则使用 **Amazon Lookout for Metrics**。如果您的*异常随时间累积*，请尝试 **Amazon Lookout for Equipment**，尽管该服务在处理多变量数据集时会表现出色。
3. 对于*任何其他用例*，您可能需要组合多个服务，或利用您可以在 **Amazon SageMaker** 平台上构建的其他算法、库和模型。

如果您想更详细地查看此思维导图，并了解其他用例以及其他 AWS 服务和库的更多细节，可以通过以下链接下载详细的高分辨率思维导图：

```
https://packt-publishing-timeseries-on-aws-michaelhoarau.s3.eu-west-1.amazonaws.com/time-series-on-AWS-decision-tree.png
```

## 总结

Amazon Lookout for Metrics 是一种 AI/ML 托管服务，可以检测数据集中的离群值，并提供根因分析，帮助您从单变量或多个时间序列数据中收集洞察。

在本章中，您学习了如何构建适合 Amazon Lookout for Metrics 的异常检测问题，并了解了该服务使用的关键概念（数据集、检测器、指标、度量和维度）。本章对于帮助您深入理解哪些应用程序适合使用 Amazon Lookout for Metrics 解决非常重要。我们还讨论了如何改进您的异常检测框架方法，以最大程度地受益于此服务为您提供的无差别繁重工作。

在下一章中，我们将把 Amazon Lookout for Metrics 连接到数据集，并配置我们的第一个检测器。

