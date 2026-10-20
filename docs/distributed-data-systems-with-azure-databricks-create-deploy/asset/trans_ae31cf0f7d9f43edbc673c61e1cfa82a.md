

我们面临的另一个阻碍是，梯度通常的计算方式需要等待所有工作节点的梯度都计算完毕。解决这一问题的另一种方法是引入运行梯度异步计算的算法，但这些算法通常很难调试。

这时，源自高性能计算的一种技术——*ring allreduce* 技术派上了用场。它允许我们通过利用最优带宽 ring allreduce 来提高神经网络训练过程的性能。

## The ring allreduce technique

随着我们引入新的工作节点来处理计算梯度规模的增加，我们开始注意到通信成本开始线性增长。我们可以配置工作节点之间的通信，以便找到利用可用带宽的最佳策略。为此，我们使用 ring all reduce，这是一种通信算法，其中通信成本无论引入多少个工作节点都保持恒定，并且仅取决于系统中最慢的连接。

`ring allreduce` 算法将工作节点按逻辑环排列，其中每个工作节点只能向其右邻居发送数据并从其左邻居接收数据。该算法在以下图表中有所描述：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_376_0.png)

Figure 12.1 – How the Ring All Reduce algorithm allows communication between workers

ring allreduce 算法分为两个步骤工作。首先，每个工作节点交换数据，使每个工作节点拥有结果的一部分，然后运行 allgather 操作，使所有工作节点都拥有完整的结果。

我们可以通过利用神经网络中梯度的固有计算过程来进一步改善工作节点之间的通信。

为了训练神经网络，我们首先运行前向传播，然后运行反向传播以计算神经网络各层的梯度，从输出层开始一直到输入层。因此，最先可用的梯度是来自输出层的梯度。我们可以利用这一过程，在所有梯度完成之前就发送梯度，以避免带宽使用峰值和与其它工作节点的通信重叠。这样，我们就克服了必须等待每个工作节点完成对神经网络所有层的梯度计算的不便。

这种方法的优点在于它与用于创建模型的网络架构和框架无关，并且允许我们更好地利用并行计算的力量来分布式训练深度学习模型。

horovod 库应用 ring allreduce 算法，以在 Azure Databricks 等分布式计算系统中实现深度学习网络的分布式训练。在下一节中，我们将学习如何利用 HorovodRunner 高效地训练深度学习模型。

## Using the Horovod distributed learning library in Azure Databricks

horovod 是一个用于分布式深度学习训练的库。它支持常用的框架，如 TensorFlow、Keras 和 PyTorch。如前所述，它基于 tensorflow-allreduce 库，并实现了 ring allreduce 算法，以便从单图形处理单元 GPU 训练轻松迁移到并行 GPU 分布式训练。

为此，我们将单 GPU 深度学习模型的训练脚本改编为在训练过程中使用 horovod 库。一旦我们改编了脚本，它就可以在单 GPU 或多 GPU 上运行，而无需更改代码。

horovod 库使用数据并行化策略，通过以优化的方式实现 ring allreduce 算法来克服通信限制，从而允许高效地将训练分布到多个 GPU 并行处理。

它以一种方式实现，即每个 GPU 获取一批数据以独立计算梯度。例如，如果我们有 32 的批大小和两个可用 GPU，每个 GPU 将接收 16 条记录的集合来计算梯度更新。这些梯度更新随后在所有可用 GPU 之间同步并取平均值，以便最终用于更新模型权重。

在对数据批次的每次训练运行中，horovod 遵循以下概述的过程来处理 GPU 之间的分布式计算：

- 每个工作节点接收模型的副本和数据集。
- 一旦收到开始信号，工作节点就会在分配的数据批次上计算梯度。
- 使用 ring allreduce 算法在 GPU 之间同步和平均计算的梯度。
- 所有工作节点使用计算出的平均梯度更新其模型的权重。

此操作最关键的方面之一是保持工作节点之间模型的一致性。如果模型以某种方式分歧，这将导致梯度计算不一致。这就是 ring allreduce 算法在高效同步表示模型可训练参数的张量方面发挥重要作用的地方。

为了开发可高效用于深度学习模型分布式训练的训练管道，一个常见的流程概述如下：

- 使用 Horovod 支持的框架之一，如 TensorFlow、Keras 或 PyTorch，为单机创建训练脚本。
- 添加 hvd.init() 函数以初始化 horovod 库。
- 使用 config.gpu_options.visible_device_list 选项选择其中一个可用 GPU 服务器。
- 使用 hvd.DistributedOptimizer 包装优化器，以在工作节点之间分发梯度下降计算。
- 使用 hvd.BroadcastGlobalVariablesHook() 分发初始变量状态，以确保所有工作节点模型随机权重的初始化一致。另一种选择是使用 hvd.broadcast_global_variables() 广播全局起始变量。
- 将代码设置为在 0 号工作节点保存检查点，以避免因重叠的读写操作导致数据损坏。
- 使用 HorovodRunner 包装一个单一的 Python 函数，该函数打包训练过程。

horovod 是 Azure Databricks Runtime for machine learning 中可用的库。在本章的下一节中，我们将学习如何使用它来分发深度学习模型的训练并有效并行化超参数调优。

作为示例，我们将展示如何修改代码以能够以分布式模式运行的构造。我们将使用 TensorFlow 模型的玩具模型示例。这假设我们有一个 TensorFlow v1 模型，我们希望将其作为分布式模型进行训练。步骤如下：

1. 为此，我们首先需要导入 TensorFlow 和 Horovod 并初始化 Horovod，如下所示：

```
import tensorflow as tf
import horovod.tensorflow as hvd
hvd.init()
```

2. 接下来，我们将 ping 将用于处理节点本地 rank 的 GPU，如下所示：

```
config = tf.ConfigProto()
config.gpu_options.visible_device_list = str(hvd.local_rank())
```

3. 在此之后，我们可以开始构建模型并定义将使用的损失函数。我们将抽象此点并假设您已经有一个要优化的损失函数。

4. 下一步是应用 Horovod 分布式优化器到所选优化器，以分发梯度计算的训练。作为示例，我们将定义一个 TensorFlow AdagradOptimizer 优化器，并将学习率设置为 Horovod 传递给工作节点的 size 大小的变量依赖项，一个用于控制学习率的参数。代码如下所示片段所示：

```
opt = tf.train.AdagradOptimizer(0.01 * hvd.size())
```

5. 现在，我们可以使用 Horovod 分布式优化器来包装所选优化器，如下所示：

```
opt = hvd.DistributedOptimizer(opt)
```

