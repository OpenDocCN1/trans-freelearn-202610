
# 预测性指标：Kaboudan 指标与修正 Kaboudan 指标

## 原始 Kaboudan 指标

1999 年，Kaboudan 定义了一个时间序列可预测性指标，称之为 ![](media/file126.png)\-指标。其背后的思想非常简单。如果我们对时间序列进行**分块打乱**，本质上就是在破坏时间序列中的信息。**分块打乱**是将时间序列分成块，然后打乱这些块的过程。因此，如果我们计算在时间序列上训练的预测的**平方误差和**（**SSE**），然后将其与在打乱后的时间序列上训练的预测的 SSE 进行对比，就可以推断该时间序列的可预测性。计算公式如下：![](media/file127.jpg)
其中，![](media/file128.png) 是基于原始时间序列生成的预测的 SSE，而 ![](media/file129.png) 是基于分块打乱序列生成的预测的 SSE。如果时间序列包含一些可预测的信号，![](media/file130.png) 会低于 ![](media/file131.png)，且 ![](media/file132.png) 会接近 1。这是因为分块打乱破坏了一些信息或模式。另一方面，如果序列只是白噪声（根据定义不可预测），![](media/file133.png) 和 ![](media/file134.png) 之间几乎不会有差异，且 ![](media/file135.png) 会接近零。

> **参考文献检查**
>
> 提出 Kaboudan 指标的研究论文在*参考文献*部分被引用为参考文献 *4*。Duan 提出的后续修正被引用为参考文献 *5*。

## 修正 Kaboudan 指标

2002 年，Duan 在其论文中研究了该指标并提出了一些修正。他发现的问题之一，特别是在长时间序列中，是 ![](media/file136.png) 值分布在 1 附近的一个窄带内，因此建议对公式进行轻微修改。我们称之为**修正 Kaboudan 指标**。下限也被截断为零。有时指标可能低于零，因为 ![](media/file137.png) 低于 ![](media/file138.png)，这是由于序列不可预测，且纯粹由于偶然，分块打乱导致 SSE 降低：![](media/file139.jpg)

该修正版本以及原始版本均已在本书的 GitHub 仓库中实现。

对用于生成预测的预测模型没有限制，这使其更加灵活。理想情况下，我们可以选择一种足够快、可应用于整个数据集的经典统计方法。但这也使得 Kaboudan 指标依赖于模型，模型的局限性内在于该指标中。该指标衡量的是序列预测难度与模型预测该序列难度的组合。

同样，这两个指标都已在本书的 GitHub 仓库中实现。让我们看看如何使用它们：

## Python 实现

```
from src.forecastability.kaboudan import kaboudan_metric, modified_kaboudan_metric
model = Theta(theta=3, seasonality_period=48*7, season_mode=SeasonalityMode.ADDITIVE)
block_df["kaboudan_metric"] = [kaboudan_metric(r[0], model=model, block_size=5, backtesting_start=0.5, n_folds=1) for r in tqdm(zip(*block_df[["energy_consumption"]].to_dict("list").values()), total=len(block_df))]
block_df["modified_kaboudan_metric"] = [modified_kaboudan_metric(r[0], model=model, block_size=5, backtesting_start=0.5, n_folds=1) for r in tqdm(zip(*block_df[["energy_consumption"]].to_dict("list").values()), total=len(block_df))]
```

虽然还有更多指标可用于此目的，但我们刚才回顾的用于评估预测性的指标涵盖了许多常见用例，足以衡量任何时间序列数据集的预测难度。我们可以使用这些指标来比较一个时间序列与另一个时间序列，或者对数据集中的一整组相关时间序列进行画像，并与另一个数据集进行基准测试。

> **延伸阅读**
>
> 如果您想深入分析这些指标的行为、它们之间的相似性以及它们在衡量预测性方面的有效性，请前往 `03-Forecastability.ipynb` 笔记本的末尾。我们计算这些指标之间的秩相关性，以了解它们的相似程度。我们还可以计算这些指标与表现最好的基线方法计算出的指标之间的秩相关性，以了解这些指标在估计时间序列预测性方面的表现如何。我强烈建议您动手操作该笔记本，理解不同指标之间的差异。挑选几个时间序列，检查不同指标如何给出略微不同的解读。

恭喜您生成了基线预测——这是我们使用本书生成的第一组预测！请随意前往笔记本，调整方法的参数，观察预测如何变化。这将帮助您建立对基线方法工作原理的直觉。如果您有兴趣了解如何改进这些基线方法，请前往*延伸阅读*部分，我们在那里提供了 F. Petropoulos 和 E. Spiliotis 的论文《The Wisdom of the Data: Getting the Most Out of Univariate Time Series Forecasting》的链接。

## 总结

至此，我们已完成*第 1 部分* *熟悉时间序列*的学习。我们从仅仅理解什么是时间序列，一路走到了生成具有竞争力的基线预测。在此过程中，我们学习了如何处理缺失值和异常值，以及如何使用 `pandas` 操作时间序列数据。我们将所有这些技能应用于一个真实的能源消耗数据集。我们还探讨了可视化和分解时间序列的方法。在本章中，我们搭建了测试框架，学习了如何使用 `NIXTLA` 库生成基线预测，并介绍了几个可用于理解时间序列预测性的指标。对某些读者而言，这可能是复习，我们希望本章在细节和实践考量方面能带来一些价值。对其他读者而言，我们希望你们在基础上已有良好掌握，准备好在本书的下一部分探索使用机器学习的现代技术。

在下一章中，我们将讨论机器学习的基础知识，并深入探讨时间序列预测。

## 参考文献

本章提供了以下参考文献：

*   Assimakopoulos, Vassilis and Nikolopoulos, K.. (2000). *Theta 模型：一种用于预测的分解方法*. International Journal of Forecasting. 16. 521-530. https://www.researchgate.net/publication/223049702_The_theta_model_A_decomposition_approach_to_forecasting.

*   Rob J. Hyndman, Baki Billah. (2003). *揭开 Theta 方法的面纱*. International Journal of Forecasting. 19. 287-290. https://robjhyndman.com/papers/Theta.pdf.

*   Shannon, C.E. (1948), *通信的数学理论*. Bell System Technical Journal, 27: 379-423. https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf.

*   Kaboudan, M. (1999). *一种利用遗传编程应用于股票收益的时间序列可预测性度量*. Journal of Forecasting, 18, 345-357: http://www.aiecon.org/conference/efmaci2004/pdf/GP_Basics_paper.pdf.

*   Duan, M. (2002). *时间序列可预测性*: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.68.1898&rep=rep1&type=pdf.

*   De Livera, A. M., & Hyndman, R. J. (2009). *使用指数平滑法预测具有复杂季节模式的时间序列* (计量经济学与商业统计系工作论文系列 15/09)

*   Hyndman, Rob. "Rob J Hyndman - 带回归变量的 TBATS." Rob J Hyndman, 2014 年 10 月 6 日, http://robjhyndman.com/hyndsight/tbats-with-regressors

## 延伸阅读
