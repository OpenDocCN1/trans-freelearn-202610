

# 存储训练数据

首先，你可以使用多个 AWS 服务来为机器学习准备数据，例如 EMR、Redshift、Glue 等。预处理训练数据后，你应该将其存储在 S3 中，采用算法期望的格式。下表显示了每种算法可接受的数据格式列表：

| Data format | Algorithm |
| application/x-image | Object Detection Algorithm, Semantic Segmentation |
| application/x-recordio | Object Detection Algorithm |
| application/x-recordio-protobuf | Factorization Machines, K-means, KNN, Latent Dirichlet Allocation, Linear Learner, NTM, PCA, RCF, Sequence-to-Sequence |
| application/jsonlines | BlazingText, DeepAR |
| image/.jpeg | Object Detection Algorithm, Semantic Segmentation |
| image/.png | Object Detection Algorithm, Semantic Segmentation |
| text/.csv | IP Insights, K-means, KNN, Latent Dirichlet Allocation, Linear Learner, NTM, PCA, RCF, XGBoost |
| text/.libsvm | XGBoost |

Figure 7.1 – Data formats that are acceptable per AWS algorithm

正如我们所见，许多算法都接受 text/.csv 格式。请记住，如果你想使用该格式，应遵循以下规则：

- 你的 CSV 文件不能有标题行。
- 对于监督学习，目标变量必须在第一列。
- 在配置训练管道时，将输入数据通道设置为 content_type 等于 text/csv。
- 对于无监督学习，将 content_type 中的 label_size 设置为 'content_type=text/csv;label_size=0'。

尽管 text/.csv 格式对许多用例来说是可以的，但大多数情况下，AWS 的内置算法使用 recordIO-protobuf 的效果更好。这是一种优化的数据格式，用于训练 AWS 的内置算法，其中 SageMaker 将数据集中的每个观测值转换为一组 4 字节浮点数的二进制表示。

RecordIO-protobuf 接受两种输入模式：pipe 模式和 file 模式。在 pipe 模式下，数据将直接从 S3 流式传输，这有助于优化存储。在 file 模式下，数据从 S3 复制到训练实例的存储卷。

我们几乎准备好了！现在，让我们快速了解一下一些建模定义，这将帮助你理解一些更高级的算法。

## 关于集成模型的一点说明

在开始深入研究算法之前，有一个重要的建模概念你应该了解，称为 **ensemble**。术语 ensemble 用于描述使用多个算法创建模型的方法。

例如，与其创建一个模型来预测欺诈交易，你可以创建多个执行相同任务的模型，并使用一种投票系统来选择预测结果。下表说明了这个简单示例：

| Transaction | Model A | Model B | Model C | Prediction |
|---|---|---|---|---|
| 1 | Fraud | Fraud | Not Fraud | Fraud |
| 2 | Not Fraud | Not Fraud | Not Fraud | Not Fraud |
| 3 | Fraud | Fraud | Fraud | Fraud |
| 4 | Not Fraud | Not Fraud | Fraud | Not Fraud |

Figure 7.2 – An example of a voting system on ensemble methods

同样的方法也适用于回归问题，其中，我们可以对每个模型的结果取平均值，并将其用作最终结果。

投票和平均只是集成方法的两个例子。其他强大的技术包括 **blending** 和 **stacking**，其中你可以创建多个模型，并使用每个模型的结果作为主模型特征。回顾前面的表格，列 "Model A"、"Model B" 和 "Model C" 将用作预测最终结果的特征。

事实证明，许多机器学习算法在训练时以嵌入方式使用集成方法。这些算法可以分为两大类：

- **Bootstrapping Aggregation or Bagging**：采用这种方法，会在数据的不同样本上训练多个模型。然后，通过投票或平均系统进行预测。该类别的主要算法被称为 **Random Forest**。
- **Boosting**：采用这种方法，会在数据的不同样本上训练多个模型。然后，一个模型试图通过惩罚错误预测来纠正下一个模型的错误。该类别的主要算法被称为 **Stochastic Gradient Boosting** 和 **AdaBoost**。

现在你知道了集成模型是什么，让我们继续研究一些可能出现在你考试中的机器学习算法。并非所有算法都使用集成方法，但我相信这会让你更容易识别出来。

我们将根据 AWS 算法类别将接下来的几个部分拆分：

- Supervised learning
- Unsupervised learning
- Textual analysis
- Image processing

最后，我们将提供 AWS 中强化学习的概述。

## Supervised learning

AWS 提供用于通用目的的监督学习算法（回归和分类任务）和用于更特定目的的监督学习算法（预测和向量化）。可在这些子类别中找到的内置算法列表如下：

- Linear learner algorithm
- Factorization machines algorithm
- XGBoost algorithm
- K-Nearest Neighbor algorithm
- Object2Vec algorithm
- DeepAR Forecasting algorithm

让我们从回归模型和线性学习算法开始。

## 使用回归模型

好吧；我知道现实问题通常既不线性也不简单。然而，研究 **linear regression** 模型是了解 **regression models** 总体内部情况的好方法（是的，回归模型可以是线性和非线性的）。这是每个数据科学家必备的知识，也可以帮助你解决实际挑战。我们将在以下小节中更详细地研究这个问题。

## 介绍回归算法

线性回归模型旨在根据一个或多个变量（X）预测数值（Y）。在数学上，我们可以将这种关系定义为 Y = f(X)，其中 Y 被称为因变量，X 被称为自变量。

对于回归模型，我们想要预测的成分（Y）始终是一个连续数字；例如，房屋价格或交易数量。我们在第 1 章《Machine Learning Fundamentals》表 2 中看到过这一点，当时我们在根据目标变量选择合适的监督学习算法类型。我们可以随时回顾一下。

当我们只使用一个变量来预测 Y 时，我们将此问题称为简单线性回归。另一方面，当我们使用多个变量来预测 Y 时，我们说我们有一个多元线性回归问题。

还有另一类回归模型，称为非线性回归。然而，让我们先放下这个问题，理解简单线性回归的含义。

回归模型属于机器学习的监督侧（另一侧是非监督）因为算法试图根据自变量和因变量之间已有的相关性来预测值。

但 Y=f(X) 中的 "f" 是什么意思？"f" 是负责根据 X 预测 Y 的回归函数。换句话说，这就是我们想要弄清楚的函数！当我们开始谈论简单线性回归时，请注意以下三个问题和答案：

1. 线性回归中 "f" 的形状是什么？
   线性的，当然！
2. 我们如何表示线性关系？
   使用一条直线（你将在几分钟内明白原因）。
3. 那么，定义一条线的函数是什么？
   ax + b（只需查看任何数学书籍）。

就是这样！线性回归模型由 y = ax + b 给出。一旦我们试图根据 X 预测 Y，我们只需要找出 "a" 和 "b" 的值。我们可以采用相同的逻辑来弄清楚其他种类回归的内部原理。

