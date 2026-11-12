## 使用 LightGBM 进行分布式和基于 GPU 的学习

本章探讨在分布式计算集群和 GPU 上训练 LightGBM 模型。分布式计算可以显著加速训练工作负载，并能够训练比单机内存可用容量大得多的数据集。我们将探讨利用 Dask 进行分布式计算以及 LightGBM 对基于 GPU 训练的支持。

本章涵盖的主题如下：

- 使用 LightGBM 和 Dask 进行分布式学习
- LightGBM 的 GPU 训练

## 技术要求

本章包含在分布式计算集群和 GPU 上训练和运行 LightGBM 模型的示例。运行示例需要 Dask 环境和 GPU。完整代码示例可在 https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-11 获取。

## 使用 LightGBM 和 Dask 进行分布式学习

Dask 是一个用于分布式计算的开源 Python 库。它旨在与现有的 Python 库和工具（包括 scikit-learn 和 LightGBM）无缝集成。本节将探讨使用 Dask 为 LightGBM 运行分布式训练工作负载。

Dask (https://www.dask.org/) 允许您在单机和多机上设置集群。在单机上运行 Dask 是默认设置，无需配置。但是，在单机集群（或调度器）上运行的工作负载也可以轻松使用分布式调度器运行。

Dask 提供了多种运行分布式集群的方式，包括集成 Kubernetes、MPI 或将自动化部署集成到 AWS 或 Google Cloud Platform 等超大规模云服务商中。

在单机上运行时，Dask 仍会将工作负载分布到多个线程中，这可以显著加速工作负载。

Dask 提供了集群管理工具类，以便轻松设置集群。本地集群可以按以下方式运行：

```
cluster = LocalCluster(n_workers=4, threads_per_worker=2)
client = Client(cluster)
```

前面的代码创建一个包含四个工作进程的本地集群，并将每个工作进程配置为运行两个线程。集群在 localhost 上运行，调度器默认在端口 8786 上运行。可以使用参数配置主机 IP 和端口。除了运行调度器之外，Dask 还会启动一个使用 Bokeh (https://docs.bokeh.org/en/latest/) 实现的诊断仪表盘。默认情况下，仪表盘在端口 8787 上运行。我们可以查看 **工作进程** 页面来了解运行中集群的状态，如图 11.1 所示。

![](img/31b70e4a77c3afbe11032f68ab874b02_230_0.png)

图 11.1 – Dask 诊断仪表盘，显示四个正在运行的工作进程及其部分技术统计信息

集群启动并运行后，我们可以为分布式集群准备数据。

Dask 提供了名为 Dask DataFrame 的数据帧实现。Dask DataFrame 由许多较小的 pandas DataFrame 组成，这些 DataFrame 根据索引拆分。每个部分可以存储在磁盘上或分布在网络中，从而允许处理比单机内存可容纳的大得多的数据集。对 Dask DataFrame 执行的操作会自动分发到 pandas DataFrame。

> **注意**
> 当数据集能装入内存时，建议使用标准 pandas DataFrame 而非 Dask DataFrame。

我们可以通过加载 CSV 文件来创建 Dask DataFrame。请注意，CSV 文件可能位于 S3 或 HDFS 上，并且可能非常大。以下代码从 CSV 文件创建 Dask DataFrame：

```python
import dask.dataframe as dd
df = dd.read_csv("covtype/covtype.csv", blocksize="64MB")
```

在此，我们还指定了加载 CSV 文件的块大小。块大小设置数据集被分割成的块，并让我们对单个 DataFrame 部分的内存进行细粒度控制。调用 df.shape 时，我们会得到一个有趣的结果：

```python
df.shape
# (Delayed('int-a0031dlf-945d-42b4-af29-ea5e40148f3f'), 55)
```

列数作为数字返回。但是，查看行数时，我们得到了一个名为 Delayed 的包装器类。这说明，即使我们创建了 DataFrame，数据也未加载到内存中。相反，Dask 在使用数据的工作进程上按需加载数据。我们可以强制 Dask 计算行数，方法如下：

```python
df.shape[0].compute()
# 581012
```

有了 DataFrame 中的数据后，我们可以将其准备用于训练。我们使用 dask_ml 中的 train_test_split 函数将数据拆分为训练集和测试集：

```python
X = df.iloc[:, :-1]
y = df.iloc[:, -1]
X_train, X_test, y_train, y_test = dask_ml.model_selection.train_test_split(X, y)
```

尽管 dask_ml 中的函数镜像了 scikit-learn 的 train_test_split 功能，但 Dask 版本保持了底层 Dask DataFrame 的分布式特性。

我们的集群现已设置完毕，数据也为训练做好了准备。我们现在可以着手训练 LightGBM 模型。

LightGBM 库团队提供并维护每个可用学习算法的 Dask 版本：DaskLGBMRegressor、DaskLGBMClassifier 和 DaskLGBMRanker。这些是围绕标准 LightGBM scikit-learn 接口的包装器，并具有额外功能以指定要使用的 Dask 集群客户端。

当 LightGBM 在 Dask 集群上运行时，每个 Dask 工作进程对应一个 LightGBM 工作进程进行训练。LightGBM 将单个工作进程上的所有数据分区连接成一个数据集，并且每个 LightGBM 工作进程独立使用本地数据集。

然后，每个 LightGBM 工作进程协同工作来训练单个 LightGBM 模型，并使用 Dask 集群进行通信。在执行数据并行训练时（如 Dask 所做的那样），LightGBM 使用 **Reduce-Scatter** 策略：

- 1. 在直方图构建阶段，每个工作进程为不同的非重叠特征构建直方图。然后，执行 **Reduce-Scatter** 操作：每个工作进程将其直方图的一部分共享给每个其他工作进程。
- 2. 在 **Reduce-Scatter** 之后，每个工作进程都有特征子集的完整直方图，然后为这些特征找到最佳分割点。
- 3. 最后，执行聚集操作：每个工作进程将其最佳分割点共享给所有其他工作进程，因此所有工作进程都拥有所有最佳分割点。

将选择最佳特征分割点，并相应地对数据进行分区。

幸运的是，分布式算法的复杂性对我们是隐藏的，训练代码与我们习惯的 scikit-learn 训练代码相同：

```
dask_model = lgb.DaskLGBMClassifier(n_estimators=200, client=client)
dask_model.fit(X_train, y_train)
```

运行前面的代码会训练 LightGBM 模型，我们可以通过查看 Dask 仪表盘的 **状态** 页面来查看进度，如图 11.2 所示。

![](img/31b70e4a77c3afbe11032f68ab874b02_232_0.png)

图 11.2 – Dask 仪表盘的状态页面，显示 LightGBM 模型训练期间的任务流





Dask LightGBM 模型可以使用 **Pickle** 或 **joblib** 进行完全序列化，我们可以按以下方式将模型保存到磁盘：

```python
with open("dask-model.pkl", "wb") as f:
    pickle.dump(dask_model, f)
```

可以通过调用模型的 `predict` 方法进行预测。请注意，Dask 模型期望输入为 Dask DataFrame 或数组：

```python
predictions = dask_model.predict(X_test)
```

与获取 Dask DataFrame 的形状类似，预测操作也是延迟的，仅在需要时才会计算。我们可以使用 `compute` 获取预测值：

```python
predictions.compute()
```

至此，我们完成了对使用 Dask 进行 LightGBM 分布式训练的探讨。借助 Dask，LightGBM 可以在远超单台服务器计算能力的海量数据集上训练模型。Dask 会随着您的需求进行扩展，因此您可以从本地笔记本电脑开始，随着数据量的增长，迁移到高性能计算环境或云基础设施。此外，如前所示，Dask 旨在与 pandas、NumPy 和 scikit-learn 等成熟的 Python 库和谐协作，为数据科学家提供熟悉的环境，同时扩展这些工具的功能。

接下来，我们将探讨当需要使用 GPU 训练大型模型时如何加速 LightGBM 训练。

## LightGBM 的 GPU 训练

LightGBM 库原生支持在 GPU 上训练模型 [1]。支持两个 GPU 平台：通过 OpenCL 的 GPU 和 CUDA。通过 OpenCL 使用 GPU 支持最广泛的 GPU 范围（包括 AMD GPU），并且比在 CPU 上运行模型快得多。但是，如果您有可用的 NVIDIA GPU，CUDA 平台提供最快的运行时。

## 为 GPU 设置 LightGBM

设置环境以使用 GPU 可能有点棘手，但我们将在此回顾核心步骤。

> **注意**
>
> 此处讨论的 GPU 设置步骤仅作为设置环境过程的指南和概述提供。此处列出的库和驱动程序的确切版本号可能已过时，建议您查看官方文档以获取最新版本：`https://lightgbm.readthedocs.io/en/latest/GPU-Tutorial.html`。

要使用 GPU，我们必须 *从源代码编译和构建 LightGBM 库*。以下说明假设使用 Ubuntu Linux 构建环境；其他平台的步骤类似。在构建库之前，必须安装一些依赖项。

重要的是，首先，为您的环境安装 GPU 驱动程序。如果您有 NVIDIA GPU，还应安装 CUDA。相关供应商网站上提供了操作说明：

- `https://docs.nvidia.com/cuda/`
- `https://www.amd.com/en/support`

接下来，我们需要安装 OpenCL 头文件：

```
sudo apt install --no-install-recommends
sudo apt install --no-install-recommends nvidia-opencl-dev opencl-headers
```

最后，安装库构建依赖项：

```
sudo apt install --no-install-recommends git cmake build-essential libboost-dev libboost-system-dev libboost-filesystem-dev
```

现在我们已准备好编译带有 GPU 支持的 LightGBM 库。克隆仓库并构建库，设置 USE_GPU 标志：

```
git clone --recursive https://github.com/microsoft/LightGBM
cd LightGBM
mkdir build
cd build
cmake -DUSE_GPU=1 ..
make -j$(nproc)
cd ..
```

如在 *第 2 章，集成学习 – 装袋与提升* 中所述。LightGBM 是一个带有 Python 接口的 C++ 库。通过前面的说明，我们已经构建了带有 GPU 支持的库，但必须构建并安装 Python 包才能从 Python 使用该库（包括 scikit-learn API）：

```
cd python-package/
python setup.py install --user --precompile
```

## 在 GPU 上运行 LightGBM

在 GPU 上运行训练代码非常简单。我们将 device 参数设置为 `gpu` 或 `cuda`：

```python
model = lgb.LGBMClassifier(
    n_estimators=150,
    device="cuda",
    is_enable_sparse=False
)
model = model.fit(X_train, y_train)
```

如前面的代码所示，我们通过将 is_enable_sparse 设置为 False 来关闭 LightGBM 的稀疏矩阵优化。GPU 设备不支持 LightGBM 的稀疏功能。此外，取决于您的数据集，您可能会收到以下警告，指出 multi_logloss 未实现：

```
Metric multi_logloss is not implemented in cuda version. Fall back to evaluation on CPU.
```

值得注意的是，仅对评估执行回退，而不对训练执行回退；训练仍在 GPU 上进行。我们可以通过检查 nvidia-smi（适用于 NVIDIA GPU）来验证是否使用了 GPU：

![](img/31b70e4a77c3afbe11032f68ab874b02_235_0.png)

图 11.3 – LightGBM 训练运行时的 nvidia-smi 输出（如我们所见，GPU 利用率为 40%）

实现的加速程度取决于您的 GPU。在 Forest Cover 数据集上，150 次迭代的训练时间从 171 秒缩短至 11 秒（实现了 15 倍加速）。

使用 GPU 带来的巨大性能提升在执行参数调优时尤其有用。例如，我们可以使用基于 GPU 的训练配合 Optuna 来显著加速最佳参数的搜索。所需的全部操作是将目标函数中的模型训练移至 GPU 设备。在定义目标函数时，我们像往常一样指定 Optuna 参数范围：

```python
def objective(trial):
    lambda_l1 = trial.suggest_float(
        'lambda_l1', 1e-8, 10.0, log=True),
```

```python
lambda_l2 = trial.suggest_float(
    'lambda_l2', 1e-8, 10.0, log=True),
...
```

然后我们使用 Optuna 参数创建模型，并确保将 device 指定为 cuda（或 gpu）：

```python
model = lgb.LGBMClassifier(
    force_row_wise=True,
    boosting_type=boosting_type,
    n_estimators=n_estimators,
    lambda_l1=lambda_l1,
    lambda_l2=lambda_l2,
    ...
    learning_rate=learning_rate,
    max_bin=max_bin,
    device="cuda")
```

目标函数的最后一部分是返回交叉验证分数：

```python
scores = cross_val_score(model, X_train, y_train, scoring="f1_macro")
return scores.mean()
```

然后我们可以像往常一样运行参数研究：

```python
sampler = optuna.samplers.TPESampler()
pruner = optuna.pruners.HyperbandPruner(
    min_resource=10, max_resource=400, reduction_factor=3)
study = optuna.create_study(
    direction='maximize', sampler=sampler,
    pruner=pruner
)
study.optimize(objective(), n_trials=10, gc_after_trial=True, n_jobs=1)
```

重要的是，此处我们还将 n_jobs 参数设置为 1，因为利用 GPU 运行并行作业可能会引起不必要的竞争和开销。

在 GPU 上训练以获得最佳性能时，有几条值得注意的最佳实践：

- 始终验证是否正在使用 GPU。尽管设置了 device=gpu，如果 GPU 不可用，LightGBM 会回退到 CPU 训练。检查的好方法是使用 nvidia-smi 等工具，如前所示，或将训练时间与参考基准进行比较。

- 使用更小的 max_bin 大小。大型数据集会减小较小 max_bin 大小的影响，而较少的箱数有利于 GPU 上的训练。同样，如果您的 GPU 支持，请使用单精度浮点数以提高性能。

- GPU 训练最适合大型密集型数据集。数据需要移动到 GPU 的 VRAM 中进行训练，如果数据集太小，移动数据所涉及的开销就太大了。

- 避免对特征列进行 one-hot 编码，因为这会导致稀疏特征矩阵，而稀疏特征矩阵在 GPU 上效果不佳。

至此，我们结束了关于如何在 LightGBM 中使用 GPU 的部分。尽管设置可能更复杂，但 GPU 由于能够同时处理数千个线程，从而实现高效的并行处理，尤其适用于大型数据集，因此能显著提升训练速度。GPU 的大规模并行性对 LightGBM 中基于直方图的算法尤其有益，使构建直方图等操作更加高效和有效。

## 总结

在本章中，我们讨论了使用 LightGBM 加速计算的两种方式。第一种是使用 Python 库 Dask 在多台机器上进行大规模分布式训练。我们展示了如何设置 Dask 集群、如何使用 Dask DataFrame 将数据分发到集群，以及如何在集群上运行 LightGBM。

第二，我们还探讨了如何在 LightGBM 中利用 GPU。值得注意的是，GPU 设置较为复杂，但当 GPU 可用时，可以实现显著的加速。我们还讨论了在 GPU 上训练 LightGBM 模型的一些最佳实践。

## 参考文献

[1] H. Zhang, S. Si, 和 C.-J. Hsieh，面向大规模树提升的 GPU 加速，2017。




