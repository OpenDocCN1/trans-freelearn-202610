

# Prediction negpos
#       neg  89  21
#       pos  11  32
#
# Accuracy : 0.7908
# 95% CI : (0.7178, 0.8523)
# No Information Rate : 0.6536
# P-Value [Acc > NIR] : 0.0001499
#
# Kappa : 0.5167
# Mcnemar's Test P-Value : 0.1116118
#
# Sensitivity : 0.8900
# Specificity : 0.6038
# PosPredValue : 0.8091
# NegPredValue : 0.7442
# Prevalence : 0.6536
# Detection Rate : 0.5817
# Detection Prevalence : 0.7190
# Balanced Accuracy : 0.7469
#
# 'Positive' Class : neg

让我们看看混淆矩阵告诉我们什么：

# 这表明在被标记为负例（neg）的记录中
# 我们预测其中 89 条为负例，11 条为正例（即，
# 它们为负例但我们错误地将其分类为正例

# 我们正确识别了 32 个正例，但错误地将
# 21 个正例分类为负例

| Prediction | neg | pos |
|---|---|---:|---:|
| neg | 89 | 21 |
| pos | 11 | 32 |

总体准确率为 79%
这可以通过使用更多的方式得到（显著）改善
Accuracy : 0.7908

```
plot(rf_model)
```

图表如下：

![](img/8ededb1e0b69e85687f6e488d1044b79_272_0.png)

# 最后我们还可以使用 R 中内置的 fourfoldplot 函数来
# 可视化我们的混淆矩阵

```
fourfoldplot(cf$table)
```

我们得到的图表如下：

![](img/8ededb1e0b69e85687f6e488d1044b79_272_1.png)

```
diab<- knnImputation(diab)

# Create the train-test set split
training_index<- createDataPartition(diab$diabetes, p = .8, list = FALSE, times = 1)

# Create the training and test dataset
diab_train<- diab[training_index,]
diab_test<- diab[-training_index,]

# We will use 10-Fold Cross Validations
diab_control<- trainControl("repeatedcv", number = 10, repeats = 3, search = "random", classProbs = TRUE)

# Create the model using methodnnet (a Neural Network package in R)
# Note that we have changed the metric here to "Accuracy" instead of # ROC
nn_model<- train(diabetes ~ ., data = diab_train, method = "nnet", preProc = c("center", "scale"), trControl = diab_control, tuneLength = 10, metric = "Accuracy")

predictions<- predict(nn_model, diab_test[,-ncol(diab_test)])
cf<- confusionMatrix(predictions, diab_test$diabetes)
cf

# >cf
# Confusion Matrix and Statistics
# #
# Reference
# Prediction neg pos
# neg 89 19
# pos 11 34
#
# Accuracy : 0.8039
# 95% CI : (0.7321, 0.8636)
# No Information Rate : 0.6536
# P-Value [Acc> NIR] : 3.3e-05
#

即使有 650+ 缺失值，我们的模型仍能达到 80%+ 的准确率。

当然可以进一步改进，但作为基准，它展示了机器学习模型可以期望的性能类型。

在二分结果变量的情况下，随机猜测的准确率只有 50%。80% 的准确率明显高于仅靠猜测所能达到的准确率：

```
plot(nn_model)
```

生成的图表如下：

![](img/8ededb1e0b69e85687f6e488d1044b79_275_0.png)

```
fourfoldplot(cf$table)
```

结果在以下图表中显示：

![](img/8ededb1e0b69e85687f6e488d1044b79_276_0.png)

## Summary

在本章中，我们学习了机器学习的基本基础，不同的类型如监督学习和无监督学习，以及数据预处理、数据插补、管理不平衡类别等主要概念。

我们还学习了今天被互换使用的术语之间的关键区别，特别是人工智能和机器学习这两个术语。我们了解到人工智能涉及广泛的主题，如博弈论、社会学、约束优化和机器学习；人工智能相对于机器学习而言范围更广。

机器学习促进了人工智能；也就是说，机器学习算法用于创建人工智能系统，但它们的范围不同。回归问题（给定一组点找到最佳拟合线）可以被视为机器学习算法，但它不太可能被视为人工智能算法（概念上，尽管技术上可以）。

在下一章中，我们将研究机器学习中的一些其他概念，如偏差、方差和正则化。我们还将阅读一些重要算法并学习如何使用 R 中的机器学习包应用它们。

## 8

## Machine Learning Deep Dive

上一章关于机器学习的介绍提供了该主题的初步概述，包括该领域的不同类别和核心概念。本章将更深入地探讨机器学习的理论方面，例如算法的局限性以及不同算法的工作原理。

**机器学习** 是一个庞大而复杂的主题，为此，本章侧重于不同主题的广度，而非深度。概念以高层次的方式介绍，读者可以参考其他资料以进一步理解这些主题。

我们将首先讨论机器学习中的几个基本理论，如梯度下降和 VC 维。接下来，我们将研究偏差和方差，这是任何建模过程中最重要的两个因素，以及偏差-方差权衡的概念。

我们随后将讨论各种机器学习算法、它们的优势和应用领域。

我们将以利用真实世界数据集使用 R 执行机器学习操作的练习作为结尾。

本章将涵盖以下主题：

- 偏差、方差和正则化特性
- 梯度下降和 VC 维理论
- 机器学习算法
- 教程：使用 R 进行机器学习

|  |  |
|---|---|
|1757|275,921|
|1630|274,713|
|1236|201,428|
|1002|193,128|
|1118|187,073|
|1339|202,422|
|1753|283,989|
|1239|228,170|
|1364|230,662|
|995|169,369|
|1000|157,305|

如果我们使用线性趋势线绘制最佳拟合线，图表看起来会是这样：

![](img/8ededb1e0b69e85687f6e488d1044b79_279_0.png)

Excel 提供了一个有用的附加功能，允许用户绘制趋势线的延伸，这可以提供未知变量的估计，或 *预测*。在这种情况下，延伸趋势线将根据函数显示 1,800-2,000 平方英尺范围内的房屋价格可能为多少。

描述数据的线性函数如下：

```
$y=126.13x + 54,466.81$
```

以下带有延伸趋势线的图表显示价格很可能在 275,000 美元到 300,000 美元之间：

![](img/8ededb1e0b69e85687f6e488d1044b79_280_0.png)

然而，有人可能会认为该线并不是最佳近似，并且可能可以提高 R2 的值，在本例中为 0.87。一般来说，R^2 越高，描述数据的模型越好。R² 有各种不同的类型，但为本节目的，我们假设 R² 越高，模型越好。

在下一节中，我们将绘制一条新的趋势线，其 R^2 明显更高，但使用多项式函数。该函数具有更高的 R^2（0.91 对比 0.87），并且在视觉上平均更接近点。

本例中的函数是 6 阶多项式：

```
y = -0.00x^6 + 0.00x^5 - 0.00x^4 + 2.50x^3 - 2,313.40x^2 + 1,125,401.77x - 224,923,813.17
```

![](img/8ededb1e0b69e85687f6e488d1044b79_281_0.png)

但是，即使该线的 R^2 更高，如果我们延伸趋势线，试图找出 1,800-2,000 平方英尺范围内的房屋价格可能为多少，我们会得到以下结果。

1,800-2,000 平方英尺范围内的房屋价格从约 280,000 美元到 2,000 平方英尺时负的 200 万美元不等。换句话说，预计购买 1800 平方英尺房屋的人将花费 280,000 美元，而根据此函数，具有“更高 R^2”的函数，预计购买 2,000 平方英尺房屋的人应获得 200 万美元！这当然不准确，但我们刚刚见证的就是所谓的过拟合。下面的图像说明了这一现象。

![](img/8ededb1e0b69e85687f6e488d1044b79_282_0.png)

在另一端是欠拟合。当构建的模型不能描述数据时就会发生这种情况。在下面的图表中，函数 y = 0.25x - 200 就是一个这样的例子：

