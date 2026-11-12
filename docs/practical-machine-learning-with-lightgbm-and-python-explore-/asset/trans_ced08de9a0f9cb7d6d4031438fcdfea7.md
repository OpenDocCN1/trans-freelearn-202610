

nationalities = df.groupby(['Nacionality', 'Target']).size().reset_index().pivot(columns='Target', index='Nacionality', values=0)
nationalities_total = nationalities.sum(axis=1)
nationalities_total = nationalities_total.sort_values(ascending=True)
nationalities.loc[nationalities_total.index].plot(kind='barh', stacked=True)

```
nationalities = df.groupby(['Nacionality', 'Target']).size().reset_index().pivot(columns='Target', index='Nacionality', values=0)
nationalities_total = nationalities.sum(axis=1)
nationalities_total = nationalities_total.sort_values(ascending=True)
nationalities.loc[nationalities_total.index].plot(kind='barh', stacked=True)
```

以下截图显示了国籍的堆叠条形图：

![](img/31b70e4a77c3afbe11032f68ab874b02_79_0.png)

图 3.7 – 'Nacionality' 特征的分布，显示学术成功数据集中几乎所有行都只有一个值

'Nacionality' 和 'International' 的分布表明它们不太具有信息性（几乎所有行都有相同的值），因此我们可以从数据集中删除它们。

最后，我们注意到 'Gender' 特征。在处理性别时，检查偏见始终是个好习惯。我们可以使用直方图可视化 'Gender' 特征相对于目标类别的分布。结果显示在此代码片段之后的截图中：

```
sns.countplot(data=df, x='Gender', hue='Target', hue_order=['Dropout',
'Enrolled', 'Graduate'])
plt.xticks(ticks=[0, 1], labels=['Female', 'Male'])
```

![](img/31b70e4a77c3afbe11032f68ab874b02_80_0.png)

图 3.8 – 学术成功数据集中 'Gender' 特征的分布

存在轻微的女性学生偏向，但不足以引起担忧。

## 建模

现在我们可以为建模准备数据集了。我们必须将 Target 值映射为整数，并删除 Nacionality 和 International 特征。我们还需要删除特征名称中的空格。LightGBM 无法处理名称中带有空格的特征；我们可以将其替换为下划线：

```
df.columns = df.columns.str.strip().str.replace(' ', '_')
df = df.drop(columns=["Nacionality", "International"], axis=1)
df["Target"]=df["Target"].map({
    "Dropout":0,
    "Enrolled":1,
    "Graduate":2
    })
X = df.drop(columns=["Target"], axis=1)
y = df["Target"]
```

我们训练并比较四个模型：一个 LightGBM GBDT、一个 LightGBM DART 树、一个带有 GOSS 的 LightGBM DART 树，以及一个 scikit-learn 随机森林。

我们将使用 GridSearchCV 进行 5 折交叉验证参数优化，以确保模型性能良好。

以下代码设置了 GBDT 的参数优化。其他模型遵循类似的模式，可以在源代码中看到：

```
def gbdt_parameter_optimization():
    params = {
        "max_depth": [-1, 32, 128],
        "n_estimators": [50, 100, 150],
        "min_child_samples": [10, 20, 30],
        "learning_rate": [0.001, 0.01, 0.1],
        "num_leaves": [32, 64, 128]
    }

    model = lgb.LGBMClassifier(force_row_wise=True, boosting_type="gbdt", verbose=-1)
    grid_search = GridSearchCV(estimator=model, param_grid=params,
                               cv=5, verbose=10)
    grid_search.fit(X, y)
    return grid_search

results = gbdt_parameter_optimization()
print(results.best_params_)
print(results.best_score_)
```

运行上述代码需要一些时间，但完成后，它会打印找到的最佳参数以及最佳模型的得分。

所有模型训练完成后，我们可以使用找到的最佳参数，通过 5 折交叉验证的 F1 得分的平均值来评估每个模型。以下代码说明了如何对 GBDT 模型执行此操作：

```
model = lgb.LGBMClassifier(force_row_wise=True, boosting_type="gbdt",
    learning_rate=0.1, max_depth=-1, min_child_samples=10, n_estimators=100, num_leaves=32, verbose=-1)
scores = cross_val_score(model, X, y, scoring="f1_macro")
scores.mean()
```

每个模型的参数优化 Jupyter 笔记本可在 GitHub 仓库中找到：https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-3。

以下表格总结了每个模型找到的最佳参数值和交叉验证的 F1 得分：

| 模型 | 学习率 | 最大深度 | 最小子样本数 | N 估计器数 | 叶子数 | 最小叶样本数 | 最小分割样本数 | F1 得分 |
|---|---|---|---|---|---|---|---|---|
| GBDT | 0.1 | - | 10 | 100 | 32 | N/A | N/A | 0.716 |
| DART | 0.1 | 128 | 30 | 150 | 128 | N/A | N/A | 0.703 |
| DART (GOSS) | 0.1 | 128 | 30 | 150 | 128 | N/A | N/A | 0.703 |
| Random Forest | N/A | N/A | N/A | 150 | N/A | 10 | 20 | 0.665 |

表 3.1 – 每个模型找到的最佳参数及其对应的 F1 得分摘要

从表中可以看出，LightGBM 模型的表现远优于 scikit-learn 随机森林。两个 DART 模型获得了几乎相同的 F1 得分，GOSS 的 F1 得分略低（表格中的值保留 3 位小数）。

这标志着我们探索数据集并为该数据集构建优化模型（使用参数网格搜索）的端到端示例的结束。在即将到来的章节中，我们将研究更复杂的数据集，并深入分析模型性能。

## 摘要

本章介绍了 LightGBM 作为高效训练提升机器的库。我们探讨了构建 GBDT 的复杂性来源以及 LightGBM 中解决这些问题的特性，例如基于直方图的采样、特性捆绑和 GOSS。我们还回顾了 LightGBM 最重要的超参数。

我们还详细概述了在 Python 中使用 LightGBM，涵盖了 LightGBM Python API 和 scikit-learn API。随后，我们使用 LightGBM 构建了第一个调优模型来预测学生学业表现，利用交叉验证和基于网格搜索的参数优化。

在下一章中，我们将 LightGBM 与另一个流行的梯度提升库 XGBoost 以及用于表格数据的 DL 技术进行比较。

## 参考文献

- [1] G. Ke, Q. Meng, T. Finley, T. Wang. W. Chen, W. Ma, Q. Ye 和 T.-Y. Liu，“LightGBM：一种高效梯度提升决策树”，载于《神经信息处理系统进展》，2017。

- [2] M. Mehta, R. Agrawal 和 J. Rissanen，“SLIQ：一种用于数据挖掘的快速可扩展分类器”，载于《数据库技术进展—EDBT’96：第 5 届国际扩展数据库技术会议，法国亚维农，1996 年 3 月 25-29 日 论文集 5》，1996。

- [3] J. Shafer, R. Agrawal, M. Mehta 等人，“SPRINT：一种用于数据挖掘的可扩展并行分类器”，载于《Vldb》，1996。

- [4] S. Ranka 和 V. Singh，“CLOUDS：一种用于大数据集的决策树分类器”，载于《第 4 届知识发现与数据挖掘会议论文集》，1998。

- [5] H. Shi，“最佳优先决策树学习”，2007。

- [6] R. Kleiman 和 D. Page，“Aucu：一种用于多类机器学习模型的性能指标”，载于《国际机器学习会议》，2019。

- [7] V. Realinho, J. Machado, L. Baptista 和 M. V. Martins，预测学生辍学和学术成功，Zenodo，2021。

## 比较 LightGBM、XGBoost 和深度学习

上一章介绍了用于构建梯度提升决策树 (GBDT) 的 LightGBM。在本章中，我们将 LightGBM 与另外两种用于表格数据建模的方法进行比较：XGBoost，另一个用于构建梯度提升树的库；以及深度神经网络 (DNN)，一种最先进的机器学习技术。

我们在两个数据集上比较 LightGBM、XGBoost 和 DNN，重点关注复杂性、数据集准备、模型性能和训练时间。

本章面向高级读者，需要一定的深度学习基础。然而，本章的主要目的不是详细理解 XGBoost 或 DNN（这两种技术在后续章节中都不会使用）。相反，在本章结束时，你应该了解 LightGBM 在机器学习领域有多具竞争力。

本章的主要主题如下：

- XGBoost 概述
- 深度学习与 TabTransformers
- 比较 LightGBM、XGBoost 和 TabTransformers

## 技术要求

本章包含示例和代码片段，说明如何在 Python 中训练 LightGBM、XGBoost 和 TabTransformer 模型。本章完整示例和设置合适环境的说明可在以下 GitHub 仓库中找到：https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-4。

