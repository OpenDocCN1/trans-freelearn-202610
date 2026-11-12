

![](img/672c0cf8ba3c000a6529b117ed96945e_185_0.png)

要选择最优模型，请关注变量在 y 轴范围顶部附近带有黑框的点。

以本示例为例，这意味着使用四变量模型 Ozone、Month、Solar.R 和 Day。不过，如前所述，两变量模型和四变量模型之间的差异似乎很小。不过，你可以看到如何使用此方法将变量从五个减少到两个，同时对回归模型造成最小的降级。

### 变量重要性

对于分类目标，可以使用随机森林算法来确定变量重要性。

在本示例中，生成了一个模拟样本，吸烟和家族病史是确定男性心脏病的关键因素：

```
set.seed(1020)
#construct a 50/50 sample of Males, and Females
gender <- sample(c("M","F"), 100, replace=T,prob=c(0.50,0.50))

#assign a higher probability of smoking to the Males (95%, WAY too
smokes <- ifelse(gender=="M",
                 sample(c("N","Y"), 100, replace=T,prob=c(0.05,0.95)),
                 sample(c("N","Y"), 100, replace=T,prob=c(0.45,0.55)))
```

```
#assume they also have a 60% chance of family history of heart disease
familyhistory <- ifelse(gender=="M",
                        sample(c("N","Y"), 100, replace=T,prob=c(0.40,0.60)),
                        sample(c("N","Y"), 100, replace=T,prob=c(0.50,0.50)))
```

```
HighChol <- sample(c("Y","N"), 100, replace=T,prob=c(0.50,0.50))
heartdisease <- sample(c("Y","N"), 100, replace=T,prob=c(0.05,0.95))

#bind all of the variables together
heart <- as.data.frame(cbind(smokes,HighChol,familyhistory,gender,heartdisease))
```

使用一些交叉表验证生成的表的一部分：

```
#row percentages for smoking by gender. Males who smoke came out
prop.table(table(heart$gender,heart$smokes),1)
```

以下是输出：

```
> prop.table(table(heart$gender,heart$smokes),1)
     N         Y
F 0.42592593 0.57407407
M 0.04347826 0.95652174
```

请参见以下代码：

```
#row percentage for family history by gender. Males with family history
prop.table(table(heart$gender,heart$familyhistory),1)
```

以下是输出：

```
> prop.table(table(heart$gender,heart$familyhistory),1)
```

| | N | Y |
|---|---|---|
| F | 0.5370370 | 0.4629630 |
| M | 0.3913043 | 0.6086957 |

最后，还可以查看按性别划分的心脏病发生情况：

```
> prop.table(table(heart$gender,heart$heartdisease),1)
```

| | N | Y |
|---|---|---|
| F | 0.96296296 | 0.03703704 |
| M | 0.93478261 | 0.06521739 |

### 变量影响图

现在我们可以运行变量影响图。下图显示高胆固醇和家族病史是最重要的变量：

```
require(randomForest)
fit <- randomForest(factor(heartdisease) ~., data=heart,ntree=1000)
(VI_F <- importance(fit))
varImpPlot(fit,type=2,main="Random Forest Variable Importance Plot")
```

## 随机森林变量重要性图 - 心脏病模拟

![](img/672c0cf8ba3c000a6529b117ed96945e_188_0.png)

这种方法的一个缺点是它单独处理每个变量，而不考虑两个变量之间的任何相关性。

## 参考资料

Hospital Inpatient Discharges (SPARCS De-Identified): 2012. (n.d.). 检索自 https://health.data.ny.gov/Health/Hospital-Inpatient-Discharges-SPARCS-De-Identified/u4ud-w55t.

## 摘要

在本章中，我们学习了关于为分析准备数据的所有内容，以便您可以开始运行模型。它始于以原始形式输入外部数据，我们了解到有几种方法可以完成这些可用方法。您还学习了如何生成自己的数据，以及两种不同的方法可以用来连接或整理数据，一种使用 SQL，另一种使用 `dplyr` 函数。

随后，我们介绍了在数据输入后有时需要的一些基本数据清洗和数据探索技术，例如标准化和转置数据、更改变量类型、创建哑变量、分箱以及消除冗余数据。您现在了解了用于初步查看数据内容及其结构的关键 R 函数。

接着，我们介绍了分析缺失值和异常值的重要概念，以及如何处理它们。

我们了解了一些将变量数量减少到可管理水平的方法，例如使用主成分分析和全子集回归等技术。

最后，我们学习了如何使用随机森林算法来识别建模中最重要的变量。

数据准备部分到此结束，在为建模准备数据的方法方面，我们仅仅是初涉入门。

在下一章中，我们将更详细地介绍随机森林，以及回归、决策树、SVM 和一些其他内容，这些内容将使我们能够为构建核心模型打下基本算法的坚实基础。

## 第 4 章。回归算法简介

> "不是无知造成了那么多麻烦；是人们明明不知道的事情却自以为知道。" - Billings

每天，预测分析技术的数量似乎都在增加。关于我们是否需要更好的算法，还是需要更多或更好的数据来解决预测分析问题，一直存在持续的争论。

无论哪种情况，随时了解可用的所有算法总是有益的。同样重要的是，磨练您对前三四个可解决 90% 将面临问题的算法的技能，并能够理解在什么情况下使用它们是有意义的。

本章是两章中的第一章，将介绍四种基本算法，涵盖您将遇到的许多典型业务场景。事实上，许多对分析从业者进行调查的数据科学调查通常会将这些技术列为所有数据科学家都应该掌握的顶级技术。在这些技术中，我们将在本章详细讨论回归，并在后续章节中讨论其余三种核心算法。但让我们先介绍所有四种：

- 回归技术代表了一种标准参数化问题求解方法，为每个自变量分配一个系数或权重。变量通过 `linear` 函数与结果变量相关。
- 决策树是一种重要的算法类别，当变量之间的关系不一定是线性时，可以将其用作回归的替代方案。
- 聚类分析也被讨论为一种无监督学习形式，其性质更具探索性，但不一定是预测性的。
- 支持向量机在处理高维数据的场景中非常有用。当数据中出现大量变量，并且您选择在模型中使用其中全部或大部分变量，而未首先应用某种数据缩减技术时，高维数据就会发挥作用。在以原始形式对某些实时数据进行分析时，这种情况经常发生，此时模型需要在瞬间内应用。

## 受监督与无监督学习模型

我们已经讨论了目标（因变量）和自变量（或特征）的概念。特征（或自变量）用于描述与目标变量的关系，或预测目标变量的值。定义自变量和因变量后，您将制定模型。表征模型从数据中学习方式的一种方法是将其分类为受监督或无监督学习模型。

### 受监督学习模型

当目标变量的可能值被指定并标记时，模型被视为受监督的，也就是说，我们知道要预测什么，目标是找到最合适的预测模型来预测结果。

例如，如果我们要预测某产品的批准率，我们知道我们要预测什么（产品的批准率），而且我们通常也知道可能结果的范围。它可能是 0-100 的百分比，也可能是诸如高批准率或低批准率之类的类别。

当问题是受监督的时，使用哪些模型的选择通常取决于目标变量的类型，即它是连续型还是分类型。它还可能取决于偏差/方差（在第 2 章“建模过程中”讨论）等因素，以及我们愿意对模型做出的各种假设。回归和决策树算法被视为受监督算法，因为目标变量或目标类别（在决策树的情况下）在预测建模开始之前就已指定。

