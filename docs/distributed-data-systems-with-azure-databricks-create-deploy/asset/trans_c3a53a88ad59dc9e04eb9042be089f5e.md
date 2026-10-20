

return {'inputs': {name: data[name].tolist() for name in data.keys()} if isinstance(data, dict) else data.tolist()}

2. 然后，我们可以使用将传递给模型进行预测的数据创建一个 Pandas 数据框，如下所示：

```
data = pd.DataFrame([{
    "sepal_length": 4.2,
    "sepal_width": 3.1,
    "petal_length": 2.1,
    "petal_width": 0.1
}])
```

3. 现在，我们可以将此数据序列化为 JSON 并作为 POST 请求发送到模型端点，如下所示：

```
import requests
headers = {
    "Authorization": f"Bearer {databricks_token}",
    "Content-Type": "application/json",
}
data_json = data.to_dict(orient='records')
response = requests.request(method='POST',
                            headers=headers,
                            url=model_uri,
                            json=data_json)
if response.status_code != 200:
    raise Exception(f"Request failed with status {response.status_code}, {response.text}")
print(response.json())
```

这样，我们就可以使用 Pandas 数据框对模型进行评分，但我们也可以使用 NumPy 数组，因为两者都具有将数据序列化为字典的方法，这些字典可以轻松转换为 JSON 格式。

4. 在这里，我们将数据作为 NumPy 数组传递：

```
data = np.array([[5.1, 3.5, 1.4, 0.2]])
data_json = create_tf_serving_json(data)
response = requests.request(method='POST',
                            headers=headers,
                            url=model_uri,
                            json=data_json)
if response.status_code != 200:
    raise Exception(f"Request failed with status {response.status_code}, {response.text}")
print(response.json())
```

在模型注册表 UI 的 **Serving** 选项卡中，您将被提示提供服务集群的状态以及可用的各个模型版本的数据及其 URI。您可以在 **Model Events** 选项卡中检查集群的状态，该选项卡还保存与该模型关联的所有事件的数据。您也可以在 **Model Versions** 选项卡中查询特定版本事件。

## Summary

机器学习模型的开发过程可能是一项复杂的工作，因为该学科本身具有混合背景，并且通常与常见的软件开发生命周期脱节。此外，如果我们无法导出用于提取数据特征的预处理管道，在将模型从开发转移到生产时，我们将遇到问题。

正如本章所述，我们可以使用 MLflow 来管理模型生命周期并对使用的模型应用分级和版本控制，并有效地序列化预处理管道以用于预处理待推断的数据。

在下一章中，我们将探讨分布式学习的概念，这是一种可以将深度学习模型的训练过程有效地分发给许多工作者的技术。

## 12 Distributed Deep Learning in Azure Databricks

在上一章中，我们学习了如何在 Azure Databricks 中有效地序列化机器学习管道并管理机器学习模型的完整开发生命周期。本章将重点介绍如何在 Azure Databricks 中应用分布式训练。

深度学习模型的分布式训练是一种将训练过程分布在计算机集群中的工作者中的技术。此过程并不简单，其实现需要我们微调工作者之间通信和传输数据的方式，否则分布式训练可能比单机训练花费更长时间。Azure Databricks Runtime for Machine Learning 包含 Horovod，这是一个库，可以让我们解决分布式训练深度学习算法时出现的大多数问题。我们还将展示如何利用 TensorFlow 机器学习框架的原生 Spark 支持在 Azure Databricks 中训练深度学习模型。

本章中我们将介绍的概念如下：

- Distributed training for deep learning
- Using the Horovod distributed learning library in Azure Databricks
- Using the Spark TensorFlow Distributor package

在深入研究这些概念之前，让我们先了解一下执行此处所示示例的要求。

## Technical requirements

在本章中，我们将使用与数据科学和优化任务中的常见主题相关的概念和工具。我们假设您已经很好地理解了诸如神经网络和超参数调优之类的概念，并且对 TensorFlow、PyTorch 或 Keras 等机器学习框架也有一般了解。

在本章中，我们将讨论各种技术，以便使用 Azure Databricks 分布和优化深度学习模型的训练，因此如果您不熟悉这些术语，建议您查阅 TensorFlow 文档，了解神经网络是如何设计和训练的。

为了在该章中处理给出的示例，您需要以下内容：

- An Azure Databricks subscription.
- An Azure Databricks notebook attached to a running cluster with Databricks Runtime ML version 7.0 or higher.

## Distributed training for deep learning

深度神经网络（DNNs）在过去几十年里推动了人工智能（AI）在计算机视觉和神经网络处理等领域的发展。这些网络每天都被应用于解决各种用例中的挑战。

为了扩展模型性能，有必要开发具有数百万个可训练参数的复杂模型架构，使得训练所需的计算成为一项资源密集型操作。随着用于训练模型的可获得数据量的增加，我们需要扩展深度学习模型的训练管道，以便能够使用这些可用数据。

通常，为了训练 DNN，我们需要遵循以下三个基本步骤，如下所示：

- 1. 将数据通过网络的层传递以计算模型损失，此操作称为前向传播。
- 2. 将此损失从输出层反向传播到第一层以计算梯度。
- 3. 我们使用计算出的梯度来更新训练过程中模型权重的优化步骤。

DNN 训练过程的可扩展性可以通过以下两种方式实现：

- Using data parallelization
- Using model parallelization

数据并行化策略涉及将模型复制到每个可用工作者中并进行拟合，使用分配的数据部分。之后，工作者之间相互通信，以便正确更新模型的权重，使用平均计算的梯度。

尽管听起来很简单，但实现起来可能相当复杂，如果操作不当，由这种方式带来的复杂性完全会抵消分布式训练带来的性能提升。

现代神经网络在过去十年中发展迅速。图像处理中使用的一些架构具有数百万个参数，需要在大量数据上进行训练。这种计算密集型的训练过程导致了将过程并行化到多个工作者中的技术的发展。

随着用于训练模型的数据量的增加，我们可以增加运行计算的工作者数量。通常，工作者接收神经网络的副本，并且小批量数据被分发给每个工作者以根据训练数据批次计算梯度。这些梯度被平均以重新计算神经网络的新权重。

当数据增加且模型具有数百万个可训练参数时，这些梯度可能在内存中占用数 GB 的空间，并且工作者之间的通信开始成为一个问题，因为我们达到了网络的最大带宽。

