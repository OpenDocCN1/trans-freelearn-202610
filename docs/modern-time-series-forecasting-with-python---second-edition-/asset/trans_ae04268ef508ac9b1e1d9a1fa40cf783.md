
2. Slawek Smyl (2020). *指数平滑与循环神经网络的混合方法用于时间序列预测*。International Journal of Forecasting. 36-1: 75-85 https://doi.org/10.1016/j.ijforecast.2019.03.017

3. Montero-Manso, P., Hyndman, R.J.. (2020), *时间序列组预测的原则与算法：局部性与全局性*。arXiv:2008.00444 [cs.LG]: https://arxiv.org/abs/2008.00444

## 延伸阅读

您可以查看以下资源以进行延伸阅读：

* *来自 Google 机器学习速成课程的稀疏性正则化*：https://developers.google.com/machine-learning/crash-course/regularization-for-sparsity/l1-regularization

* *来自机器学习基础，Bloomberg ML EDU 的 L1 和 L2 正则化*：https://www.youtube.com/watch?v=d6XDOS4btck

* *来自 scikit-learn 的交叉验证：评估估计器性能*：https://scikit-learn.org/stable/modules/cross_validation.html

* *Rossmann 商店销售额*：https://www.kaggle.com/c/rossmann-store-sales

* *网络流量时间序列预测* – https://www.kaggle.com/c/web-traffic-time-series-forecasting

* *Corporación Favorita 食品杂货销售预测* – https://www.kaggle.com/c/favorita-grocery-sales-forecasting

* *M5 预测 – 准确性* – https://www.kaggle.com/c/m5-forecasting-accuracy

# 6 时间序列预测的特征工程

## 加入我们的 Discord 图书社区

![](media/file0.png)

[https://packt.link/EarlyAccess/](https://packt.link/EarlyAccess/)

在上一章中，我们开始探讨将机器学习（ML）作为解决时间序列预测问题的工具。我们还讨论了一些技术，如时间延迟嵌入和时间嵌入，这些技术从机器学习范式将时间序列预测问题转化为经典回归问题。在本章中，我们将详细研究这些技术，并使用贯穿全书的数据集进行实践演示。

在本章中，我们将涵盖以下主题：

* 特征工程
* 避免数据泄露
* 设定预测范围
* 时间延迟嵌入
* 时间嵌入

## 技术要求

* 您需要按照本书*前言*中的说明设置 `Anaconda` 环境，以获得一个包含本书代码所需的所有库和数据集的工作环境。任何额外的库将在运行笔记本时安装

在使用本章代码之前，您需要运行以下笔记本：

* `Chapter02` 中的 `02-Preprocessing London Smart Meter Dataset.ipynb`
* `Chapter04` 中的 `01-Setting up Experiment Harness.ipynb`

本章代码可在 https://github.com/PacktPublishing/Modern-Time-Series-Forecasting-with-Python-/tree/main/notebooks/Chapter06 找到。

## 特征工程

顾名思义，特征工程是从数据中构建特征的过程，主要利用领域知识，使学习过程更顺畅、更高效。在典型的机器学习设置中，构建良好的特征对于从任何机器学习模型获得良好性能至关重要。特征工程是机器学习中高度主观的部分，每个手头的问题都有不同的路径——一条专门为该问题手工打造的路径。

当我们将时间序列问题转化为回归问题时，可以应用一些标准技术。这是该过程中的关键步骤，因为机器学习模型对时间的理解程度取决于我们设计捕捉时间特征的效果。我们在第 4 章 *建立强基线预测* 中介绍的基线方法，是专门为时间序列预测用例创建的，因此问题的时间层面内嵌在这些模型中。例如，`ARIMA` 不需要任何特征工程来理解时间，因为这已内嵌在模型中。但标准回归模型对时间没有显式理解，因此我们需要创建良好的特征来嵌入问题的时间层面。

在上一章（第 5 章，*时间序列预测作为回归*）中，我们讨论了将时间编码到回归框架中的两个主要思想：时间延迟嵌入和时间嵌入。虽然我们在高层面上涉及了这些概念，但现在是时候深入挖掘并看到它们的实际应用了。

> 笔记本提示
>
> 要跟随完整代码，请使用 `chapter06` 文件夹中的 `01-Feature Engineering.ipynb` 笔记本。

我们已经将正在处理的数据集拆分为训练、验证和测试数据集。但由于我们生成的特征基于先前的观测值，从操作角度来看，拥有合并后的训练、验证和测试数据集会更好。稍后会清楚为什么，但现在请先相信这一点并继续前进。现在，让我们继续合并这些数据集：

```python
# 读取缺失值插补和训练测试拆分的数据
train_df = pd.read_parquet(preprocessed / "selected_blocks_train_missing_imputed.parquet")
val_df = pd.read_parquet(preprocessed / "selected_blocks_val_missing_imputed.parquet")
test_df = pd.read_parquet(preprocessed / "selected_blocks_test_missing_imputed.parquet")
# 在合并前添加训练、验证和测试标签以区分它们
train_df['type'] = "train"
val_df['type'] = "val"
test_df['type'] = "test"
full_df = pd.concat([train_df, val_df, test_df]).sort_values(["LCLid", "timestamp"])
del train_df, test_df, val_df
```

现在，我们有了一个合并了训练、验证和测试数据集的 `full_df`。你们中的一些人可能已经对合并训练集和测试集感到警觉。那数据泄露怎么办？让我们来看看。

## 避免数据泄露

当模型使用在预测时不可用的信息进行训练时，就会发生数据泄露。通常，这会导致训练集上性能很高，但在未见数据上性能很差。数据泄露有两种类型：

* **目标泄露** 是指我们试图预测的目标信息泄露到了模型的某些特征中，导致模型过度依赖这些特征，最终导致泛化能力差。这包括以任何方式使用目标的特征。

* **训练-测试污染** 是指训练集和测试集之间存在信息泄露。这可能是由于粗心的数据处理和拆分造成的。但也可能以更微妙的方式发生，例如在拆分训练集和测试集之前对数据集进行缩放。

当我们处理时间序列预测问题时，我们能犯的最大且最常见的错误就是目标泄露。我们必须仔细思考每个特征，确保我们没有使用任何在预测期间不可用的数据。下图将帮助我们记住并内化这一概念：

![Figure 6.1 – Usable and not-usable information to avoid data leakage](media/file166.jpg)

图 6.1 – 避免数据泄露的可用与不可用信息
