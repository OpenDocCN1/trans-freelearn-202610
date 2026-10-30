

## 9
## Amazon SageMaker 建模

在上一章中，我们学习了多种模型优化和评估技术。我们还学习了存储数据、处理数据以及对数据应用不同统计方法的不同方式。那么，我们现在如何为此构建管道呢？好吧，我们可以读取数据、处理数据，并在处理后的数据上构建机器学习模型。但如果我的第一个机器学习模型表现不佳怎么办？我可以微调我的模型吗？答案是肯定的；你几乎可以使用 Amazon SageMaker 完成所有操作。在本章中，我们将使用 Amazon SageMaker 带你了解以下主题：

- 了解 Amazon SageMaker 的不同实例
- 在 Amazon SageMaker 的 Jupyter Notebook 中清理和准备数据
- 在 Amazon SageMaker 中进行模型训练
- 使用 SageMaker 的内置机器学习算法
- 在 SageMaker 中编写自定义训练和推理代码

### 技术要求

你可以在 GitHub 上下载本章示例中使用的数据，地址为 https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-9。

### 在 Amazon SageMaker 中创建笔记本

如果你从事机器学习工作，那么你需要执行诸如存储数据、处理数据、为模型训练准备数据、模型训练以及部署模型进行推理等操作。它们并不容易，每个阶段都需要一台机器来执行任务。有了 Amazon SageMaker，执行这些步骤时生活会变得轻松得多。

### 什么是 Amazon SageMaker？

SageMaker 提供训练实例，使用数据训练模型，并提供端点实例，使用模型进行推理。它还提供笔记本实例，运行 Jupyter Notebook，以清理和理解数据。如果你对清理过程满意，那么你应该将它们存储在 S3 中作为训练的暂存。你可以启动训练实例来消费此训练数据并生成机器学习模型。机器学习模型可以存储在 S3 中，端点实例可以消费该模型为终端用户生成结果。

如果你将其绘制成块图，那么它将看起来类似于图 9.1：

![](img/209e007237d34506e7ba16180f2e905c_288_0.png)

图 9.1 – Amazon SageMaker 实例不同层的图形表示

现在，让我们看看 Amazon SageMaker 控制台，以便更好地了解它。一旦你登录到你的 AWS 账户并进入 Amazon SageMaker，你会看到类似于图 9.2 的内容：

![](img/209e007237d34506e7ba16180f2e905c_289_0.png)

图 9.2 – SageMaker 控制台的快速浏览

有三个不同的部分，包括 Notebook、Training 和 Inference，它们在图 9.2 中已展开，以便我们可以深入了解它们。

Notebook 有三个不同的选项可供使用：

- **Notebook instances**：这有助于我们创建、打开、启动和停止笔记本实例。这些实例负责运行 Jupyter Notebook。它们允许我们根据用例的工作负载选择实例类型。最佳实践是使用笔记本实例来编排处理大型数据集的数据管道。例如，从笔记本实例向 AWS Glue 发出调用以进行 ETL 服务，或向 Amazon EMR 运行 Spark 应用程序。如果你被要求在 AWS 外部创建安全的笔记本实例，那么你需要处理端点安全、网络安全、启动机器、在其上管理存储以及管理实例上运行的 Jupyter Notebook 应用程序。用户无需使用 SageMaker 管理这些内容。

- **Lifecycle configurations**：当存在需要不同库的用例，而该库在笔记本实例中不可用时，这很有用。为了安装库，用户将执行 `pip install` 或 `conda install`。然而，一旦笔记本实例终止，定制就会丢失。为了避免这种情况，你可以通过 **Lifecycle configurations** 提供的脚本来自定义你的笔记本实例。你可以选择 `/home/ec2-user/anaconda3/envs/` 中存在的任何环境，并根据需要自定义特定环境。
- **Git repositories**：AWS CodeCommit、GitHub 或任何其他 Git 服务器可以与笔记本实例关联以实现笔记本的持久化。如果获得访问权限，则同一笔记本可以被其他开发人员使用以协作并以源代码控制的方式保存。Git 仓库可以通过此选项单独添加，也可以在创建时与笔记本实例关联。

正如你在 *图 9.2* 中看到的，**Training** 提供了 **Algorithms**、**Training jobs** 和 **Hyperparameter tuning jobs**。让我们了解它们的使用方法：

- **Algorithms**：这是决定要在清理后的数据上运行的算法的第一步。你可以选择自定义算法或根据用例创建自定义算法。否则，你可以在清理后的数据上运行 SageMaker 算法。
- **Training jobs**：你可以通过 API 调用从笔记本实例创建训练作业。你可以设置实例数量，输入数据源详细信息，执行检查点配置和输出数据配置。Amazon SageMaker 管理训练实例，并将模型工件作为输出存储在指定位置。增量训练（即不时训练模型以获得更好的结果）和托管 Spot 训练（即降低成本）也可以实现。
- **Hyperparameter tuning jobs**：通常，超参数在训练过程之前为算法设置。在训练过程中，我们让算法找出这些参数的最佳值。通过超参数调优，我们获得具有超参数最佳值的最佳模型。这可以通过控制台或通过 API 调用完成。也可以从笔记本实例编排。

**Inference** 提供了许多功能，并且每天都在发展：

- **Compilation jobs**：如果你的模型使用 Keras、MXNet、ONNX、PyTorch、TFLite、TensorFlow 或 XGBoost 等机器学习框架训练，并且你的模型工件可在 S3 存储桶上获得，那么你可以选择 **Target device** 或 **Target platform**。目标设备用于部署你的模型，例如 AWS SageMaker 机器学习实例或 AWS IoT Greengrass 设备。目标平台用于决定你希望模型运行的操作系统、架构和加速器。你还可以将编译后的模块存储在你的 S3 存储桶中以供将来使用。这本质上帮助你在跨平台模型部署。
- **Model packages**：这些用于创建可部署的 SageMaker 模型。你可以创建自己的算法，使用模型包 API 对其进行打包，并将其发布到 AWS Marketplace。
- **Models**：模型是使用模型工件创建的。它们类似于带有变量的数学方程；也就是说，你输入变量的值并得到输出。这些模型存储在 S3 中，并将由端点用于推理。
- **Endpoint configurations**：Amazon SageMaker 允许你将多个加权模型部署到单个端点。这意味着你可以将特定数量的请求路由到一个端点。*这意味着什么？* 嗯，假设你有一个正在使用的模型。你想用更新的模型替换它。然而，你不能简单地移除已经在使用的第一个模型。在这种情况下，你可以使用 `VariantWeight` API 使端点以 80% 的请求使用旧模型，以 20% 的请求使用新模型。这是数据快速变化且模型需要定期训练和调优的最常见生产场景。另一个可能的使用场景是使用实时数据测试模型结果，然后可以将一定比例的请求路由到新模型，并监控结果以证明模型在实时未见数据上的准确性。
- **Endpoints**：这些用于创建模型公开的 URL，可以请求它以响应提供模型结果。
- **Batch transform jobs**：如果用例要求你在数百万条记录上进行推理，那么而不是单独的推理作业，你可以在未见数据上运行批量转换作业。请注意，当你解析数千条未见数据作为一批后从模型接收数千个结果时可能会产生一些混淆。为了克服这种混淆，可以使用 `InputFilter`、`JoinSource`、`OutputFilter` API 将输入记录与输出结果关联。

