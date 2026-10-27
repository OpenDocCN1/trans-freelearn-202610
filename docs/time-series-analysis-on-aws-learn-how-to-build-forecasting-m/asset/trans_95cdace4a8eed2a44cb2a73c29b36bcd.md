

![](img/e4ee48104c730845e796d8c469dfeecc_235_0.png)

Figure 7.28 – AWS CloudFormation 堆栈列表

+   4. 选择 `energy-consumption-forecast-stack` 并点击顶部的 Delete 按钮。

删除过程开始，几分钟后，你的账户将被清理。此解决方案创建的 Amazon S3 存储桶将由该过程清空并删除。但是，如果你在实验期间创建了其他存储桶并上传了其他文件，你将需要导航到 Amazon S3 控制台并手动删除这些资源。

## Summary

在本章中，你深入了解了预测准确性指标，并学习了如何执行一些分析以更好地了解预测器的性能。特别是，你揭示了数据集中的任何行为变化（数据漂移）可能对预测练习的准确性产生的影响。你还学习了如何摆脱使用控制台，借助 AWS 验证并积极维护的无服务器解决方案，将 Amazon Forecast 结果集成到你的业务运营中。

恭喜！本章结束了专门介绍使用 Amazon Forecast 进行预测的部分。在下一章中，你将开始进入多元时间序列异常检测之旅！

## Section 2:
Detecting Abnormal Behavior
in Multivariate Time Series
with Amazon Lookout
for Equipment

在本节中，你将学习如何训练一个模型来发现多元传感器数据中的异常，并创建调度程序来监控你的设备。你还将阅读有关 Amazon Lookout for Equipment 可以提供的见解，以帮助你改进异常检测和预测实践。

本节包含以下章节：

- Chapter 8, An Overview of Amazon Lookout for Equipment
- Chapter 9, Creating a Dataset and Ingesting Your Data
- Chapter 10, Training and Evaluating a Model
- Chapter 11, Scheduling Regular Inferences
- Chapter 12, Reducing Time to Insights for Anomaly Detections

## An Overview of Amazon Lookout for Equipment

在本章中，你将了解 **Amazon Lookout for Equipment** 可以做什么，它是如何工作的，以及它更适合的应用类型。你还将大致了解如何准备你的数据集以及如何将服务结果集成到你自己的业务流程中。

与 **Amazon Forecast** 和 **Amazon Lookout for Metrics**（你将在本书最后一部分了解）一起，Amazon Lookout for Equipment 是专门针对围绕时间序列数据构建的问题的 **人工智能（AI）/机器学习（ML）托管** 服务之一。Amazon Lookout for Equipment 是一种异常检测服务，可以为工业设备和预测性维护团队提供有价值的见解。

在本章中，我们将涵盖以下主要主题：

- What is Amazon Lookout for Equipment?
- What are the different approaches to tackle anomaly detection?
- The challenges encountered with multivariate time series data
- How does Amazon Lookout for Equipment work?
- How do you choose the right applications?

### Technical requirements

无需具备 **Python** 或 **R** 等语言的动手经验即可跟随本章内容。然而，我们强烈建议你在 AWS 控制台中跟随操作，在那里你可以访问 Amazon Lookout for Equipment 服务。

如果你已有 AWS 账户，你可以连接到 AWS 控制台，点击顶部的搜索栏，并在 **Services** 部分输入 Lookout for Equipment。然后，点击 **Amazon Lookout for Equipment** 以打开服务控制台的登陆页面。

如果你没有 AWS 账户，你需要在登录控制台之前创建一个。要创建 AWS 账户并登录 Amazon Forecast 控制台，请参阅 *Chapter 2, An Overview of Amazon Forecast* 的 *Technical requirements* 部分。

现在你准备好使用 Amazon Lookout for Equipment 了！

### What is Amazon Lookout for Equipment?

Amazon Lookout for Equipment 是可在 AWS 云平台上使用的 AI/ML 托管服务之一。该服务利用你工业设备上的传感器数据，并基于该设备的特定数据自动训练无监督 ML 模型，而无需任何 ML 专业知识。Amazon Lookout for Equipment 分析你传入的传感器时间序列数据，以标记可能导致设备或制造过程出现异常或故障的早期预警信号（例如事件）。Amazon Lookout for Equipment 是 **资产无关** 的。这意味着除了时间序列数据之外，你无需提供正在监控的过程或设备的特征（例如设备的品牌和型号、时间序列数据的测量单位、资产的年龄等）即可使用该服务。

**托管服务** 是仅由终端用户提供数据并配置参数以满足其需求的服务。所有其他任务，*被认为是无差别的繁重工作*，均由服务代表用户执行。这包括所有基础设施管理的自动化。作为 Amazon Lookout for Equipment 用户，你无需配置和管理虚拟机、配置用户账户、实施安全、规划如果请求量增加时的可扩展性，或停用未使用的资源。

在 AI/ML 托管服务的情况下，一些数据准备、摄取任务和模型管理活动也在后台执行，使你能够主要专注于要解决的问题。Amazon Lookout for Equipment 是一种可扩展的托管服务，可自动化从数据摄取到模型部署和提供推理结果的端到端异常检测管道。此外，该服务在后台处理数据准备。Amazon Lookout for Equipment 可以执行以下功能：

- 它可以为你填充数据集中的 *缺失值*。
- 它可以处理 *时间戳未对齐* 的情况（即时间序列之间没有共享的通用日期时间索引）。
- 它可以 *准备时间序列*，使其可被多个算法摄取。Amazon Lookout for Equipment 可以为深度学习或适用于异常检测的统计模型准备适当的时间序列数据集。
- 它可以根据你的历史时间序列数据 *计算智能阈值*，以决定给定的数据点是否应标记为异常。许多无监督方法要求你提供污染水平，即数据集中存在的外<|message|> 