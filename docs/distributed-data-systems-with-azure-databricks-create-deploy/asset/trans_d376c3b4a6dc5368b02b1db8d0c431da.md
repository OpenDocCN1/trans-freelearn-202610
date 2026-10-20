

这些参数将直接影响我们模型的性能和训练时间，其选择在我们模型的成功中起着至关重要的作用。例如，学习率过低的神经网络将无法准确捕捉观察数据中的模式。找到良好的超参数需要我们尽可能高效地映射搜索空间，这可能是一项艰巨的任务。这是因为为了找到每组良好的值，我们需要训练一个新模型，而这在时间和计算资源方面可能是一项昂贵的操作。用于此的一些技术包括常见的算法，如网格搜索、随机搜索和贝叶斯优化。

我们可以应用的一种更高效地搜索最佳超参数的技术是 **AutoML**，它代表 **自动化机器学习**。这是自动应用机器学习来解决优化问题的过程，在我们的案例中是优化训练算法中使用的超参数。这有助于我们克服手动搜索正确超参数或应用诸如网格搜索和随机搜索等技术时可能出现的问题，这些算法必须运行很长时间。这是因为它们会在不评估这些区域可能有多有希望的情况下搜索搜索空间中的所有可能值。

Azure Databricks Runtime for Machine Learning（Databricks Runtime ML）为我们提供了两种自动映射可能超参数搜索空间的选项。它们被称为 MLflow 和 Hyperopt，这两个开源库将 AutoML 应用于自动化模型选择和超参数调优。

HyperOpt 是一个 Python 库，专为大规模贝叶斯优化模型的超参数而设计。它可以用于以分布式方式将搜索过程扩展到多个计算核心。虽然我们将只关注超参数优化方面，但它也可以用于优化管道，以及数据预处理、学习算法选择，当然还有超参数调优。要使用 HyperOpt，我们需要定义一个优化器，该优化器将应用于要优化的期望函数以优化任何函数，在我们的案例中可以是最大化性能指标或最小化损失函数。在我们的示例中，HyperOpt 将采用超参数的搜索空间，并根据之前的结果移动，从而以一种知情的方式在搜索空间中移动，这使其与随机搜索和网格搜索等算法不同。

Azure Databricks Runtime for Machine Learning 中用于自动映射可能超参数搜索空间的另一个可用库是 MLflow，我们已在前几章中讨论过。MLflow 是一个开源平台，用于管理端到端机器学习和深度学习模型生命周期，并支持自动化模型调优跟踪。这种自动化模型跟踪来自其与 Spark MLlib 库的集成。它允许我们通过使用 CrossValidator 和 TrainValidationSplit 来跟踪哪些超参数能产生最佳结果，这些工具会自动记录验证指标和超参数，以便更轻松地获得最佳模型。

在接下来的章节中，我们将学习如何将 HyperOpt 和 MLflow 应用于 Azure Databricks Runtime for Machine Learning 训练的模型，以跟踪并获得最佳超参数。

## 使用 MLflow 自动化模型跟踪

正如我们之前提到的，MLflow 是一个用于管理机器和深度学习模型生命周期的开源平台，它允许我们进行实验、确保可重复性并支持轻松的模型部署。它还为我们提供了一个集中的模型注册表。总体而言，MLflow 的组件如下：

- MLflow Tracking：它记录与实验相关的所有数据，例如代码、数据、配置和结果。
- MLflow Projects：它以确保结果可以在不同平台间运行之间重现的格式封装代码。
- MLflow Models：这为我们提供了机器学习和深度学习模型的部署平台。
- Model Registry：我们机器学习和深度学习模型的中央存储库。

在本节中，我们将重点关注 MLflow Tracking，这是允许我们记录和注册代码、属性、超参数、工件以及与训练深度学习和机器学习模型相关的其他组件的组件。MLflow Tracking 组件依赖于两个概念，即实验和运行。实验是我们执行训练过程的地方，是主要组织单元；所有运行都属于一个实验。因此，这些实验指的是特定的运行，我们可以可视化它、比较它，并下载与其相关的日志和工件。以下信息存储在每个 MLflow 中：

- Source：这是实验运行所在的笔记本。
- Version：如果运行是由 MLflow Project 触发的，则为笔记本版本或 Git 提交哈希。
- Start and end time：训练过程的开始和结束时间。
- Parameters：包含训练过程中使用的模型参数的字典。
- Metrics：包含模型评估指标的字典。MLflow 记录并让你可视化模型在运行过程中的性能。
- Tags：这些作为键值对保存的运行元数据。你可以在运行完成期间和之后更新标签。键和值都是字符串。
- Artifacts：这些是运行中使用的任何格式的数据文件，例如模型本身、训练数据或使用的任何其他类型的数据。

在 Azure Databricks Runtime for Machine Learning 中，每当我们在训练算法的代码中使用 CrossValidator 或 TrainValidationSplit 时，MLflow 都会存储超参数和评估指标，以便更轻松地可视化并最终找到最佳模型。

在接下来的章节中，我们将学习如何在训练算法时使用 CrossValidator 和 TrainValidationSplit 来利用 MLflow 的优势并可视化产生最佳结果的超参数。

## 管理 MLflow 运行

当我们在训练模型时使用 CrossValidator 或 TrainValidationSplit 时，我们将拥有嵌套的 MLflow 运行。嵌套方式如下：

- Main run：CrossValidator 或 TrainValidationSplit 的信息记录到主运行中。如果没有活动的运行，MLflow 将创建一个新运行并记录到其中，并在退出进程前结束它。
- Child runs：每个超参数值及其对应的性能指标都记录在依赖于父运行的子运行中。

当我们执行多步骤处理的超参数搜索时，这些嵌套运行很常见。例如，我们可以拥有如下所示的嵌套运行：

```
with mlflow.start_run(nested=True):
    mlflow.log_param("lr", 0.05)
    mlflow.log_param("batch_size", 256)
    with mlflow.start_run(nested=True):
        mlflow.log_param("max_runs", 16)
        mlflow.log_param("epochs", 10)
        mlflow.log_metric("rmse", 98)
mlflow.end_run()
```

这些嵌套运行将在 MLflow UI 中显示为可展开的树，以便我们可以更详细地查看结果。这使我们能够在主运行中保持不同子运行的结果井然有序。

在讨论 MLflow 运行管理时，我们需要确保我们正在记录到主运行。为了确保这一点，当调用 fit() 函数时，它应被包裹在 mlflow.start_run() 语句中以将信息记录到正确的运行中。正如我们已经看到的，这样我们就可以轻松地将指标和参数放置在运行中。如果在同一个活动运行中多次调用 fit() 函数，MLflow 将为参数和指标的名称附加唯一标识符以避免任何可能的命名冲突。

在下一节中，我们将看到一个使用 MLflow tracking 与 MLlib 结合找到机器学习模型最佳超参数集的示例。

