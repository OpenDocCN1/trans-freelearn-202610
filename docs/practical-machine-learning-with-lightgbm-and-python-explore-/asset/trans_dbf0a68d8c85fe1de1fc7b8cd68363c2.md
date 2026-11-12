

关于 $\Omega$ 有多种定义。决策树中各项的典型实现如下：

```
$$\Omega(w) = \alpha \sum_i^n |w_i| + \lambda \sum_i^n w_i^2$$
```

这里，$\alpha \sum_i^n |w_i|$ 是 L1 正则化项，由参数 $\alpha$ 控制，$0 \le \alpha \le 1$，而 $\lambda \sum_i^n w_i^2$ 是 L2 正则化项，由参数 $\lambda$ 控制。

L1 正则化通过惩罚绝对输出较大的叶子，使叶子得分趋向于零。较小的叶子输出对树的预测影响较小，从而有效简化了树。

L2 正则化类似，但由于对输出取平方，它对异常值叶子有过大影响。

最后，当构建更大的树（叶子更多，因此 $w$ 向量更大）时，$\Omega(w)$ 的两个求和项都会增加，从而增加目标函数的输出。因此，较大的树会受到惩罚，过拟合也就减少了。

## LightGBM 优化总结

总而言之，LightGBM 通过以下方式改进了标准集成算法：

- 实现基于直方图的特征采样，以降低寻找最佳分裂点的计算成本
- 计算互斥特征束，以减少稀疏数据集中的特征数量
- 应用 GOSS 对训练数据进行下采样而不损失精度
- 按叶子生长树以提高精度
- 可通过 L1 和 L2 正则化及其他控制参数来控制过拟合

这些优化共同使 LightGBM 的计算性能相对于标准 GBDT 算法提升了**数量级 (OOM)**。此外，LightGBM 使用 C++ 实现并提供 Python 接口，因此其代码比基于 Python 的 GBDT（例如 scikit-learn 中的 GBDT）快得多。

最后，LightGBM 还支持改进的数据并行和特征并行分布式训练。分布式训练和 GPU 支持将在后续的第 11 章“使用 LightGBM 进行分布式和基于 GPU 的学习”中讨论。

## 超参数

LightGBM 暴露了许多可用于自定义训练过程、目标和性能的参数。接下来，我们将讨论最显著的参数以及如何使用它们来控制特定现象。

**注意**
核心 LightGBM 框架使用 C++ 开发，但包含用于在 C、Python 和 R 中使用 LightGBM 的 API。本节讨论的参数是框架参数，各 API 会以不同方式公开这些参数。下一节将讨论使用 Python 时可用的参数。

以下是用于控制优化过程和目标的 **核心框架参数**：

- **objective**: LightGBM 支持以下优化目标（不限于此）——回归（包括使用其他损失函数（如 Huber 和 Fair）的回归应用）、二分类（分类）、多分类（分类）、交叉熵以及用于排序问题的 lambdarank。
- **boosting**: boosting 参数控制 boosting 类型。默认情况下，此参数设置为 `gbdt`，即标准 GBDT 算法。其他选项包括 `dart` 和 `rf`（用于随机森林）。随机森林模式不执行 boosting，而是构建随机森林。
- **num_iterations**（或 **n_estimators**）：控制 boosting 迭代次数，从而控制构建的树的数量。
- **num_leaves**：控制单棵树中叶子的最大数量。
- **learning_rate**：控制学习率（或缩减率），即每棵树对整体预测的贡献。

LightGBM 还提供许多用于控制学习过程的参数。我们将根据这些参数如何用于调整训练的特定方面来讨论它们。

以下控制参数可用于提高 **准确性**：

- **boosting**: 使用 `dart`，已证明其优于标准 GBDT。
- **learning_rate**: 为获得更好的准确性，学习率必须与 `num_iterations` 共同调整。学习率小且 `num_iterations` 值大可以提高准确性，但会以优化速度为代价。
- **num_leaves**: 叶子数量越多，准确性越高，但可能导致过拟合。
- **max_bin**: 构建直方图时特征分桶的最大桶数。`max_bin` 值越大，训练速度越慢，内存占用越高，但可能提高准确性。

以下 **学习控制参数** 可用于处理 **过拟合**：

- `early_stopping_round`: 启用早停，并控制用于确定是否停止训练的迭代次数。如果在 `early_stopping_round` 设置的迭代次数内，任何指标均无改进，则停止训练。
- `min_data_in_leaf`: 叶子中允许的最小样本数。值越大，过拟合越少。
- `min_gain_to_split`: 执行分裂所需的最小信息增益。值越高，过拟合越少。
- `reg_alpha`: 控制 L1 正则化。值越高，过拟合越少。
- `reg_lambda`: 控制 L2 正则化。值越高，过拟合越少。
- `max_depth`: 控制单棵树的最大深度。树越浅，过拟合越少。
- `max_drop`: 控制使用 DART 算法时丢弃的树的最大数量（仅当 `boosting` 设置为 `dart` 时使用）。值越大，过拟合越少。
- `extra_trees`: 启用 **极度随机树 (ExtraTrees)** 算法。随后，LightGBM 会为每个特征随机选择分裂阈值。启用 Extra-Trees 可以减少过拟合。该参数可与任何 boosting 模式配合使用。

此处讨论的参数仅包含 LightGBM 中可用参数的一部分，并侧重于提高准确性和控制过拟合。完整参数列表可通过以下链接获取：https://lightgbm.readthedocs.io/en/latest/Parameters.html。

## LightGBM 的局限性

LightGBM 旨在比传统方法更高效、更有效。它尤其以其处理大型数据集的能力而闻名。然而，与任何算法或框架一样，它也有其局限性和潜在缺点，包括以下内容：

- **对过拟合敏感**：LightGBM 可能对过拟合比较敏感，尤其是在处理小型或带噪声的数据集时。使用 LightGBM 时，应注意监控并控制过拟合。
- **最佳性能需要调参**：如前所述，LightGBM 有许多超参数，需要对其进行适当调优才能从算法中获得最佳性能。
- **缺乏表示学习**：与深度学习（DL）方法不同，深度学习方法擅长从原始数据中学习，而 LightGBM 在学习前需要对数据进行特征工程。特征工程是一个耗时且需要领域知识的过程。
- **处理序列数据**：LightGBM 并非天生用于处理序列数据（如时间序列）。要使用 LightGBM 处理时间序列数据，需要应用特征工程来创建滞后特征并捕获时间依赖性。
- **复杂交互和非线性**：LightGBM 是一种基于决策树的方法，可能无法捕捉复杂的特征交互和非线性。需要应用适当的特征工程，以确保算法对这些进行建模。

虽然这些是使用该算法的潜在局限性，但它们可能并不适用于所有用例。在合适的情况下，LightGBM 通常是一个非常有效的工具。与任何模型一样，理解权衡对于为您的应用做出正确选择至关重要。

在下一节中，我们将介绍如何开始在 Python 中使用各种 LightGBM API。

## 在 Python 中开始使用 LightGBM

LightGBM 使用 C++ 实现，但提供官方 C、R 和 Python API。本节将讨论可用于使用 LightGBM 的 Python API。LightGBM 提供三个 Python API：标准 **LightGBM** API、**scikit-learn** API（与其他 scikit-learn 功能完全兼容）以及用于使用 Dask 的 **Dask** API。Dask 是一个并行计算库，将在第 11 章“使用 LightGBM 进行分布式和基于 GPU 的学习”（https://www.dask.org/）中讨论。

在本书的其余部分，我们主要使用 LightGBM 的 scikit-learn API，但首先让我们来看看标准 Python API。

### LightGBM Python API

深入学习 Python API 的最佳方式是通过动手示例。以下是代码清单中的摘录，用于说明 LightGBM Python API 的用法。完整代码示例可在以下网址获取：https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-3。

需要导入 LightGBM。导入通常缩写为 lgb：

```
import lightgbm as lgb
```

LightGBM 提供了一个 Dataset 包装器类用于处理数据。Dataset 支持多种格式。通常，它用于包装 numpy 数组或 pandas DataFrame。Dataset 还接受指向 CSV、TSV、LIBSVM 文本文件或 LightGBM Dataset 二进制文件的路径。当提供路径时，LightGBM 会从磁盘加载数据。

这里，我们从 sklearn 加载 Forest Cover 数据集，并将 numpy 数组包装在 LightGBM Dataset 中：

```
dataset = datasets.fetch_covtype()
X_train, X_test, y_train, y_test = train_test_split(dataset.data,
dataset.target, random_state=179)
training_set = lgb.Dataset(X_train, y_train - 1)
test_set = lgb.Dataset(X_test, y_test - 1)
```

我们从 y_train 和 y_test 数组中减去 1，因为 sklearn 提供的类标记范围为 [1, 7]，而 LightGBM 期望零索引的类标记，范围为 [0, 7]。

