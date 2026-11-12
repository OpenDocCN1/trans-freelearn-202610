

$$F_1(x) = F_0(x) + \nu \gamma_{2,1} = 125 + 0.1(45) = 129.5$$

正如预期，我们的预测已朝着目标值的方向改进。对其他样本执行相同操作，我们得到以下结果：

| 性别 | 空腹血糖 | 腰围 | BMI | LDL胆固醇 | 预测 F 0(x) | F 0(x) 的残差 | 预测 F 1(x) |
|---|---|---|---|---|---|---|---|
| 男性 | 105 | 110 | 29.3 | 170 | 125 | 45 | 129.5 |
| 女性 | 85 | 80 | 21 | 90 | 125 | -35 | 122.65 |
| 男性 | 95 | 93 | 26 | 113 | 125 | -12 | 122.65 |

表 2.5 – 按照步骤 2.1 至 2.4，基于初始预测和残差，我们计算出新的预测 $\mathbf{F}_1(\mathbf{x})$

学习率的目的是限制每棵树对整体预测可能产生的影响：通过小步改进我们的预测，最终得到整体上更准确的模型。

然后重复步骤 2，直到得到最终预测 $F_M(x)$。

总之，我们的梯度增强集成由一系列回归树做出的预测的加权（由学习率加权）求和组成，每棵树都预测前一次预测的伪残差（相对于前一次预测的误差梯度），从而最小化前一次预测的误差以产生准确的最终预测。

## 用于分类的梯度增强

此前对梯度增强的解释使用回归问题作为示例。由于算法相同，我们不会对分类进行详细示例说明。不过，我们不是使用连续预测值，而是采用与逻辑回归（https://en.wikipedia.org/wiki/Logistic_regression）相同的技术。因此，各棵树预测样本属于某个类别的概率。概率的计算方法是取样本的对数几率，并使用逻辑函数将其转换为概率，如下所示：

$$p(x) = \frac{1}{1 + e^{-(x-\mu)/s}}$$

伪残差计算为观察值（类别为 1 或 0）与预测值（逻辑函数得出的概率）之差。另一个不同之处是损失函数。我们可以使用交叉熵作为损失，而不是 MSE 之类的函数，如下所示：

$$H_p(q) = -\frac{1}{N} \sum_{i=1}^N y_i \log(p(y_i)) + (1 - y_i) \log(1 - p(y_i))$$

### 梯度增强决策树超参数

除标准决策树训练的参数外，scikit-learn 中还提供了以下专用于梯度增强树的新超参数：

- n_estimators: 控制集成中树的数量。通常，树越多越好。然而，通常会达到边际效益递减点，当树太多时会发生过拟合。

- learning_rate: 控制每棵树对集成的贡献。较低的学习率会导致更长的训练时间，并可能需要构建更多树（n_estimators 的较大值）。将 learning_rate 设置为非常大的值可能会导致优化遗漏最优点，并且必须与较少的树结合使用。

scikit-learn 梯度增强超参数的完整列表可在 https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html 找到。

### scikit-learn 中的梯度增强

梯度增强的细节涉及数学且复杂；幸运的是，通过 scikit-learn，该算法与其他算法一样易于使用。以下是 scikit-learn 中 GradientBoostingClassifier 类的示例，再次使用本章前面用于训练随机森林分类器的 Forest CoverType 数据集。

该分类器也从 ensemble 包导入，如下所示：

```
from sklearn.ensemble import GradientBoostingClassifier
```

我们像之前一样获取并分割数据，然后拟合模型，如下所示：

```
dataset = datasets.fetch_covtype()

X_train, X_test, y_train, y_test = train_test_split(dataset.data,
dataset.target, random_state=179)
booster = GradientBoostingClassifier(random_state=179, min_samples_leaf=3, min_samples_split=3, learning_rate=0.13, n_estimators=180)
booster = booster.fit(X_train, y_train)
print(f1_score(y_test, booster.predict(X_test), average="macro"))
```

运行上述代码应产生 0.7119 的 F1 分数，这个分数甚至比标准决策树还要差得多。我们可以花时间优化超参数以提高性能。然而，有一个更严重的问题。之前的代码执行时间非常长——在我们的硬件上大约需要 45 分钟——而 ExtraTrees 大约只需 3 分钟。

LightGBM 解决了我们对梯度增强树的两个问题，并在极短的时间内构建了性能显著更好的梯度增强树。

在接下来的章节中，我们将简要介绍与梯度增强相关的一种高级算法：DART。

### 高级增强算法 – DART

**DART** 是前一节讨论的标准 GBDT 算法的扩展 [4]。DART 采用 **脱落**，一种源自 **深度学习（DL）** 的技术，以避免决策树集成的过拟合。该扩展简单明了，包含两部分。首先，在拟合下一棵预测树 \( M_{n+1}(x)（由所有先前树 \( M_n...M_1 \) 的缩放求和组成）时，而是使用先前树的一个随机子集，将其他树从求和中舍弃。\( p_{drop} \) 参数控制先前树被包含的概率。DART 算法的第二部分是对新树贡献应用额外缩放。设 \( k \) 为计算新树 \( M_{n+1} \) 时被舍弃的树的数量。由于 \( M_{n+1} \) 在更新我们的预测 \( F_{n+1}（包含所有树）时未包含这 \( k \) 棵树的贡献，因此预测会过冲。因此，新树按 \( \frac{1}{k} \) 的因子进行缩放以进行补偿。

研究表明，DART 在性能上优于标准 GBDT，同时也显著改善了过拟合问题。

Scikit-learn 未为 GBDT 实现 DART，但 LightGBM 中已集成 DART。

## 总结

总之，本章探讨了决策树集成学习的两种最常用方法：袋装法（bagging）和提升法（boosting）。我们研究了使用袋装法构建决策树集成的随机森林和 ExtraTrees 算法。

本章还通过逐步讲解 GBDT 算法，详细概述了决策树中的提升法，说明了如何应用梯度增强。我们介绍了 scikit-learn 中随机森林、ExtraTrees 和 GBDT 的实际示例。

最后，我们探讨了如何使用 DART 算法将脱落法应用于 GBDT。我们现在已彻底理解决策树集成技术，并准备深入研究 LightGBM。

下一章将详细介绍 LightGBM 库，包括该库所取得的理论进步及其实际应用。我们还将探讨如何使用 LightGBM 与 Python 解决机器学习问题。

## 参考文献

- [1] L. Breiman, “Random forests,” Machine learning, vol. 45, p. 5-32, 2001.
- [2] P. Geurts, D. Ernst and L. Wehenkel, “Extremely randomized trees,” Machine learning, vol. 63, p. 3-42, 2006.
- [3] J. H. Friedman, “Greedy function approximation: a gradient boosting machine,” The Annals of Statistics, p. 1189-1232, 2001.
- [4] R. K. Vinayak and R. Gilad-Bachrach, “Dart: Dropouts meet multiple additive regression trees,” in Artificial Intelligence and Statistics, 2015.

## Python 中 LightGBM 概览

在上一章中，我们探讨了决策树的集成学习方法。**自助聚合（bagging）**和梯度增强都进行了详细讨论，并附有在 scikit-learn 中应用这些技术的实际示例。我们还展示了**梯度增强决策树（GBDT）**训练缓慢，并且在某些问题上可能表现不佳。

本章介绍 LightGBM，一种使用基于树的学习器的梯度增强框架。我们探讨 LightGBM 对集成学习方法所做的创新和优化。还给出了通过 Python 实际使用 LightGBM 的更多详细信息和示例。最后，本章包含一个使用 LightGBM 的建模示例，融合了用于模型验证和参数优化的更高级技术。

本章结束时，您将彻底理解 LightGBM 的理论和实际特性，从而能够深入研究在数据科学和生产系统中使用 LightGBM。

本章的主要主题如下：

- 介绍 LightGBM
- 在 Python 中开始使用 LightGBM
- 构建 LightGBM 模型

### 技术要求

本章包含示例和代码片段，说明如何在 Python 中使用 LightGBM。本章完整示例以及设置合适环境的说明可在 https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-3 获取。

