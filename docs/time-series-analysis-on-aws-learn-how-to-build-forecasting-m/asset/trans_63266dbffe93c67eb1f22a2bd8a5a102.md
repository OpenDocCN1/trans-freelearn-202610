

## NPTS

一个简单的预测器是一种算法，它使用过去观察到的值之一作为下一个时间步长的预测，如下所述：

- 一个朴素预测器将紧邻的前一个观测值作为下一个时间步长的预测。
- 一个季节性朴素预测器将过去季节的观测值作为下一个时间步长的预测。

NPTS 属于这种简单预测器的类别。

### 理论背景

正如刚才提到的，NPTS 属于简单预测器的类别。然而，它并不使用固定的时间索引作为最后一个值：相反，它随机采样过去的某个值来生成预测。通过多次采样，NPTS 能够生成一个预测分布，Amazon Forecast 可以用它来计算预测区间。

让我们称 $y_t$ 为我们初始时间序列的原始值，t 的范围从 0 到 T − 1，T 是我们想要交付预测 $\hat{y}_T$ 的时间步，如下所示：

$$\hat{y}_T = y_t$$

而时间索引 t 实际上是从采样分布 $q_T$ 中采样的。NPTS 使用以下采样分布：

$$q_T(t)=\frac{e^{-\alpha|T-t|}}{\sum_{t'=0}^{T-1}e^{-\alpha|T-t'|}}$$

这里，$\alpha$ 是一个核权重超参数，应根据数据进行调整，有助于控制权重衰减的程度。这使得 NPTS 能够以更高的概率采样最近的过去值，而不是遥远过去的观测值。

该超参数可以取 0 到无穷大的值。以下是这些极值含义：

- 当 $\alpha$ 为 0 时，所有权重都是均匀的：这导致了 **气候学预测器**。预测值是从过去中均匀采样的，仅考虑给定时间序列可能取值的统计分布，而不考虑 **当前行为** 的动态影响。
- 如果 $\alpha$ 设置为无穷大，你将得到 **朴素预测器**，它总是预测最后观测到的值。

一旦你为下一步生成了预测，你可以将此预测包含在你的过去数据集中并生成后续预测，同时赋予 NPTS 采样过去预测数据的能力。

如果你想深入了解该算法以及更一般地了解间歇性数据预测，可以深入阅读以下文章：

- *GluonTS: Probabilistic Time Series Models in Python* (https://arxiv.org/pdf/1906.05264.pdf)
- *Intermittent Demand Forecasting with Renewal Processes* (https://arxiv.org/pdf/2010.01550.pdf)

让我们现在看看该算法在 Amazon Forecast 中的实现方式。

### Amazon Forecast 中的实现

以下是 NPTS 算法允许你选择的几个关键参数：

- `context_length`: 这是 NPTS 用于进行预测的过去数据点数量。默认情况下，Amazon Forecast 使用训练范围内的所有点。
- `kernel_type`: 这是用于定义权重以采样过去观测值的方法。此参数可以是 `uniform` 或 `exponential`。默认值是 `exponential`。
- `exp_kernel_weights`: 当你选择 `exponential` 核类型时，你可以指定 α 参数来控制对过去值应用指数衰减的速度。该参数的默认值是 0.01。此参数应始终为正数。
- 你还可以选择仅采样过去的季节，而不是从所有可用观测值中采样。此行为由 `use_seasonal_model` 参数控制，可以设置为 `True`（默认值）或 `False`。

当使用季节性模型时，你还可以通过将 `use_default_time_features` 参数设置为 `True` 来请求 Amazon Forecast 自动提供并使用取决于预测粒度的时间特征。假设，例如，你的数据以小时级提供：如果此参数设置为 `True`，并且你想为下午 3 点给出预测，那么 NPTS 算法将只采样过去也在下午 3 点发生的观测值。

## Prophet

Prophet 是一种预测算法，由 Facebook 于 2017 年 2 月开源。

### 理论背景

Prophet 算法类似于具有四个组成部分的广义加性模型，如下所示：

$$y(t) = g(t) + s(t) + h(t) + \varepsilon_t$$

这里适用以下内容：

- $g(t)$ 是趋势函数：趋势使用分段逻辑增长模型进行建模。Prophet 还允许通过自动变化点选择来改变趋势（例如，每月定义一个变化点）。逻辑增长模型使用容量 $C(t)$（以事件数量衡量的最大市场规模）、增长率 $k$ 和偏移参数 $m$ 定义，如下所示：

  $$g(t) = \frac{C(t)}{1 + e^{-k(t-m)}}$$

- $s(t)$ 表示周期性变化，例如 yearly 或 weekly 季节性分量。如果 $P$ 是你期望时间序列具有的常规周期（$P = 365.25$ 表示 yearly 季节性效应，$P = 7$ 表示 weekly 周期），则可以定义一个近似的任意平滑季节性效应，如下所示：

  $$s(t) = \sum_{n=1}^{N} \left( a_n cos\left(\frac{2\pi n t}{P}\right) + b_n sin\left(\frac{2\pi n t}{P}\right) \right)$$

  默认情况下，Prophet 对 yearly 季节性使用 N = 10，对 weekly 季节性使用 N = 3。

- $h(t)$ 表示假日或更一般地，持续 1 天或多天的非规则事件的影响。当你决定使用假日补充功能时，Amazon Forecast 会将假日列表馈送到此字段（请参阅本章后面包含假日和天气数据的部分）。

- $\varepsilon_t$ 是一个误差项，说明模型未涵盖的所有内容。

如果你想了解更多关于 Prophet 的理论细节，可以阅读 Facebook 发表的原始论文，链接如下：https://peerj.com/preprints/3190/。

### Amazon Forecast 中的实现

Amazon Forecast 使用 Prophet 的 Python 实现中的 Prophet 类，并使用所有默认参数。

Amazon Forecast 将为你的数据集中可用的每个时间序列创建一个本地模型。在我们一直在运行的能源消耗示例中，每个家庭的用电量将作为单个 Prophet 模型进行建模。

如果你想更深入地了解 Prophet 默认参数选择的影响，我建议你阅读以下链接提供的全面 Prophet 文档：`https://facebook.github.io/prophet/`。

## DeepAR+

我们一直在回顾的经典方法——例如 ARIMA、ETS 或 NPTS——为数据集中提供的每个时间序列拟合一个单一模型。然后它们使用这些模型中的每一个为所需的时间序列提供预测。在某些应用中，你可能有数百或数千个并行演变的相似时间序列。这将是电子商务网站上每个产品的销售单位数量或能源供应商服务的每个家庭的用电量的情况。对于这些情况，利用从多个时间序列联合学习全局模型可能提供更准确的预测。这就是 DeepAR+ 所采用的方法。

### 理论背景

DeepAR+ 是一种用于 univariate 时间序列预测的有监督算法。它使用 **循环神经网络 (RNNs)** 和大量时间序列来训练单个全局模型。在训练时，对于摄入目标时间序列数据集的每个时间序列，DeepAR+ 会生成多个序列（时间序列片段），这些序列在原始时间序列中有不同的起点，以提高模型的学习能力。

底层理论细节超出了本书的范围：如果你想了解更多关于 DeepAR 算法的理论细节，可以执行以下操作：

- 你可以前往此 YouTube 视频，了解如何将人工神经网络适应于数据序列（例如时间序列）：`https://www.youtube.com/watch?v=WCUNPb-5EYI`。该视频是关于 ML 和 **人工神经网络 (ANNs)** 的更大课程的一部分。
- 你可以阅读原始发表论文，_DeepAR: Probabilistic Forecasting with Autoregressive Recurrent Networks_ 作者 David Salinas、Valentin Flunkert、Jan Gasthaus，链接如下：`https://arxiv.org/abs/1704.04110`。

