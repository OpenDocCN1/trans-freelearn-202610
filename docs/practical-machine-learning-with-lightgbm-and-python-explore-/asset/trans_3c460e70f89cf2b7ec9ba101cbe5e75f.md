

LightGBM 和 XGBoost 模型的超参数。对于 LightGBM，尝试了 DART 和 GBDT 模型，其中 DART 表现更好。与普查收入数据集不同，信用卡数据集未预先划分为训练集和测试集。因此，我们应用五折交叉验证来衡量对未见数据的性能。以下代码训练 LightGBM 模型，XGBoost 代码非常相似：

```
model = lgb.LGBMClassifier(force_row_wise=True, boosting_type="dart",
learning_rate=0.0023, max_bin=384, n_estimators=300, scale_pos_weight=scale_pos_weight, verbose=-1)
scores = cross_val_score(model, X, y, scoring="f1_macro")
print (f"Mean F1-score: {scores.mean()}")
```

LightGBM 和 XGBoost 的结果与 TabTransformer 结果一起显示在表 4.4 中。

## 训练 TabTransformer 模型

没有分类特征时，TabTransformer 架构可以大大简化。让我们看看图 4.2 所示的架构。可以看到，嵌入层和注意力层不再需要。事实上，模型简化为常规 MLP（将模型仍称为变换器是不诚实的，因为根本没有使用注意力层）。

除了移除不需要的层之外，其余架构和流程与普查收入问题保持相同。AdamW 再次用作优化器，我们对超参数和模型中的隐藏层数量进行网格搜索优化。与梯度提升模型一样，执行五折交叉验证来衡量性能。

## 结果

尽管接下来也报告了准确率，但必须注意的是，它在不平衡数据上不是一个良好的性能指标。在该数据集中，99.82% 的样本属于同一类别，而只预测该类别的模型将具有 99.82% 的准确率且完全毫无意义。F1 得分不受类别不平衡影响，仍然是不平衡数据集中分类性能的良好指标。以下表格显示了三种算法在五折交叉验证下的结果。

| 模型 | 训练时间 | 准确率 | F1 得分 |
| --- | --- | --- | --- |
| LightGBM GBDT | 113s | 99.88% | 0.80 |
| XGBoost GBDT | 351s | 98.41% | 0.82 |
| TabTransformer | 528.59s | 93.37% | 0.05 |

表 4.4 – 在信用卡欺诈数据集上训练三种模型的结果。训练时间包括五折交叉验证

XGBoost 和 LightGBM 在该数据集上的表现非常相似，分别获得 0.82 和 0.80 的 F1 得分。DNN 在该问题上困难重重，即使使用类别权重来补偿不平衡数据集，F1 得分也仅为 0.05。

调试 DNN 的性能问题众所周知非常棘手。由于构建和训练 DNN 模型的复杂性和不透明性，小的更改可能会产生显著影响。

性能不佳的可能原因包括以下几点：

- **模型架构不足：**这是最可能的原因。该架构不适合该问题。需要对架构、层大小甚至神经网络类型进行进一步实验。
- **训练不足：**模型可能训练时间不够长。增加训练轮数可以提高性能。然而，在我们的实验中，损失在 10 个轮数后停滞不前（而训练继续了 15 个轮数）。
- **损失函数不当：**我们应用了带有类别权重的 `BinaryCrossentropy` 损失函数。然而，可以尝试更高级的损失函数，例如焦点损失 [9]。

就训练和验证时间而言，情况与普查收入数据集类似。LightGBM 模型的训练和验证速度比其他方法快得多：比 XGBoost 快 3.1 倍，比 DNN 快 4.62 倍。

## 总结

在本章中，我们讨论了可用于解决表格学习问题的两种其他算法：XGBoost，另一种梯度提升框架，以及 TabTransformer，一种深度学习方法。

我们展示了如何在两个数据集上设置和训练 XGBoost 模型和 TabTransformer。我们还展示了如何为基于树的模型和神经网络模型编码分类特征。两个数据集也存在类别不平衡，我们在训练期间需要对此进行补偿。

我们发现 LightGBM 和 XGBoost 产生的模型准确率相似，但 LightGBM 训练模型的速度要快得多且更高效。我们也看到了训练 DNN 的复杂性以及在这些问题上表现平平的情况。深度学习是一种极其强大的技术，但在处理表格数据集时，基于树的方法往往更适用。

在下一章中，我们将重点介绍使用名为 Optuna 的框架对 LightGBM 进行更有效的参数优化。

## 参考文献

- [1] T. Chen 和 C. Guestrin，“XGBoost”，载于第 22 届 ACM SIGKDD 知识发现与数据挖掘国际会议论文集，2016 年。
- [2] A. Vaswani, N. Shazeer, N. Parmar, J. Uszkoreit, L. Jones, A. N. Gomez, L. Kaiser, 和 I. Polosukhin，“Attention Is All You Need”，2017 年。
- [3] X. Huang, A. Khetan, M. Cvitkovic, 和 Z. Karnin，TabTransformer：使用上下文嵌入进行表格数据建模，2020 年。
- [4] R. Becker，Adult，UCI 机器学习库，1996 年。
- [5] D. Hendrycks 和 K. Gimpel，高斯误差线性单元（GELUs），2020 年。
- [6] G. Klambauer, T. Unterthiner, A. Mayr 和 S. Hochreiter，自归一化神经网络，2017 年。
- [7] I. Loshchilov 和 F. Hutter，解耦权重衰减正则化，2019 年。
- [8] A. Dal Pozzolo, O. Caelen, R. Johnson 和 G. Bontempi，“用于不平衡分类的欠采样概率校准”，2015 年。
- [9] T.-Y. Lin, P. Goyal, R. Girshick, K. He 和 P. Dollár，用于密集物体检测的焦点损失，2018 年。

## 第二部分：使用 LightGBM 进行实用机器学习

第二部分深入探讨了支撑实用机器学习工程的复杂流程，首先介绍了通过名为 Optuna 的框架进行高效超参数优化。接着，我们将全面探索数据科学生命周期，阐述从问题定义和数据处理到实际数据科学建模应用的严谨步骤。本部分的最后，重点将转向自动机器学习，重点介绍 FLAML 库，该库旨在简化和优化模型选择和调优。贯穿本部分，案例研究和动手示例的结合将为利用这些高级工具的全部潜力提供清晰的路线图，强调效率和优化的主题。

本部分将包括以下章节：

- 第 5 章，使用 Optuna 进行 LightGBM 参数优化
- 第 6 章，使用 LightGBM 解决现实世界的数据科学问题
- 第 7 章，使用 LightGBM 和 FLAML 进行 AutoML

## 使用 Optuna 进行 LightGBM 参数优化

前几章讨论了 LightGBM 超参数及其对模型构建的影响。构建新模型时的一个基本问题是找到最佳超参数以实现最佳性能。

本章重点介绍使用名为 Optuna 的框架进行参数优化过程。除了超参数空间的剪枝，还讨论了不同的优化算法。一个实际示例展示了如何应用 Optuna 为 LightGBM 找到最佳参数。还展示了 Optuna 的高级用例。

本章的主要主题如下：

- Optuna 和优化算法
- 使用 Optuna 优化 LightGBM

## 技术要求

本章包含示例和代码片段，说明如何使用 Optuna 对 LightGBM 进行参数优化研究。有关设置本章适当环境的完整示例和说明，可在 https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-5 上找到。

## Optuna 和优化算法

前几章的示例表明，为问题选择最佳超参数在解决机器学习问题中至关重要。超参数对算法的性能和泛化能力有显著影响。最佳参数也特定于所使用的模型和正在解决的学习问题。

