

|type|air_temperature_k|process_temperature_k|rotational_speed_rpm|torque_nm|tool_wear_min|machine_failure|type_index|
|---|---|---|---|---|---|---|---|
| M | 298.1| 308.6| 1551.0| 42.8| 0.0| 0.0| 1.0|
| L | 298.2| 308.7| 1408.0| 46.3| 3.0| 0.0| 0.0|
| L | 298.1| 308.5| 1498.0| 49.4| 5.0| 0.0| 0.0|

图 5.16 – DataFrame 输出

以下代码片段通过向量函数生成特征并对列进行归一化：

```
from pyspark.ml.feature import VectorAssembler
assembler = VectorAssembler(inputCols=["air_temperature_k",
 "process_temperature_k", "rotational_speed_rpm",
 "torque_nm", "tool_wear_min","type_index"],
 outputCol="features")
train_df = assembler.transform(train_df)
# apply standard scaler
from pyspark.ml.feature import StandardScaler
scaler = StandardScaler(inputCol="features", outputCol="scaledFeatures")
scalerModel = scaler.fit(train_df)
train_df = scalerModel.transform(train_df)
# show the scaled features
train_df.select("scaledFeatures", "machine_failure").show(3)
```

将看到以下输出：

| scaledFeatures|machine_failure|
|---|---|
|[149.030724148837...| 0.0|
|[149.080717682600...| 0.0|
|[149.030724148837...| 0.0|

仅显示前 3 行

图 5.17 – 缩放特征

以下代码片段将数据集拆分为训练集和测试集。然后使用 LogisticRegression 训练模型并评估模型性能。

```
# split the data
train_df, test_df = train_df.randomSplit([0.7, 0.3])
# select the features and label
train_df = train_df.select("scaledFeatures", "machine_failure")
test_df = test_df.select("scaledFeatures", "machine_failure")
# train the model
from pyspark.ml.classification import LogisticRegression
lr = LogisticRegression(
    featuresCol="scaledFeatures", labelCol="machine_failure")
lrModel = lr.fit(train_df)
# evaluate the model
from pyspark.ml.evaluation import BinaryClassificationEvaluator
evaluator = BinaryClassificationEvaluator(
    labelCol="machine_failure")
evaluator.evaluate(lrModel.transform(test_df))
```

输出结果为 0.918577395158542。

至此，代码示例的讲解结束。

## 总结

本章在监督学习领域探讨了分类的基本概念和技术。分类是机器学习中的一项关键任务，可将数据分类到预定义的类别中。这对于电子邮件过滤、医疗诊断和客户细分等广泛应用至关重要。

我们深入探讨了各种分类算法，包括决策树、SVM、KNN、逻辑回归、朴素贝叶斯和 FMs。本文讨论了每种算法的独特优势、应用、优点和局限性，从而全面了解其实际用例。

通过实际案例研究和现实世界示例，我们展示了分类在金融、医疗保健和网络安全等不同领域中的变革性影响。我们还介绍了使用准确率、精确率、召回率、F1 分数和混淆矩阵等指标评估分类器性能的关键方面，以确保对模型进行严格评估和改进。

此外，我们还探讨了提高模型性能的高级技术，例如数据预处理、特征工程、处理数据不平衡、超参数调优和正则化。通过 Apache Spark 中的代码示例进行的实践练习，为您提供了实现和优化分类模型所需的实用技能。

到此，您应该已对分类有扎实的理解，并准备好运用这些技术解决复杂问题，标志着您在机器学习这个迷人世界中的旅程迈出了重要一步。

在下一章中，我们将学习聚类。

## 第 3 部分：无监督学习

在本部分中，您将深入了解无监督学习的世界——这是机器学习的一个强大分支，其中模型从未标记的数据中学习。与监督学习不同，监督学习的目标是预测目标变量，而无监督学习则专注于挖掘数据中的隐藏模式、关系和结构。本部分旨在为您提供对无监督学习中使用的关键概念、技术和算法的深入理解。

本部分中的章节将指导您了解聚类技术、推荐技术和关联规则挖掘。您将探索这些方法在现实生活场景中的应用，从而能够利用无监督学习解决复杂问题。

本部分包含以下章节：

- 第 6 章，构建聚类系统
- 第 7 章，构建推荐系统
- 第 8 章，挖掘频繁模式

## 构建聚类系统

在不断发展的机器学习领域，无监督学习犹如探索的灯塔，算法在其中深入原始、未标记的数据，在没有明确标签或指令的情况下寻找模式、结构和洞察。本章专门探讨聚类的艺术与科学，聚类是一种无监督学习类型，根据相似性对数据点进行分组，揭示数据集的底层结构。

聚类算法是数据的制图师，绘制数据集内隐藏的领域。它们基于一个简单而深刻的前提运作：将相似的数据点聚合在一起，并将显著不同的数据点分开。这项看似简单直接的任务充满复杂性和细微差别，因为“相似性”的定义会根据上下文、数据类型和预期结果而有很大差异。

本章通过考察多种算法来探索聚类技术，以揭示数据景观中独特的模式。我们深入探讨其机制、假设、优势和局限性。挑战包括确定最佳聚类数量、处理高维数据以及评估聚类质量。指标用于指导评估，而实际应用涵盖市场细分、异常检测、生物信息学和社交网络分析。聚类提取价值、为决策提供信息并揭示隐藏模式。

本章将涵盖以下主题：

- 了解聚类
- 学习聚类算法
- 评估模型性能
- 提高模型性能

通过理解聚类的原理、挑战和应用，您将能够在自己的工作中发挥其潜力，拓展在没有预定义标签或结构的数据集中可以发现的边界。

## 技术要求

本章的代码文件可在 GitHub 上找到：https://github.com/PacktPublishing/Apache-Spark-for-Machine-Learning/tree/main/Chapter06。

## 了解聚类

聚类是无监督机器学习和数据挖掘中的一项基本技术，旨在将一组对象分组，使同一组（或簇）中的对象比其他簇中的对象更相似。它是一种探索性数据分析方法，广泛应用于各个领域和应用中，用于发现底层模式、对数据进行分类，并通过将相似项目分组来总结数据集。

## 理解聚类

与监督学习不同，监督学习中模型从标记数据中学习，而聚类使用未标记数据。其目标不是预测结果，而是通过基于数据相似性识别组或簇来理解数据结构。相似性通常使用欧氏距离、曼哈顿距离或针对数据类型量身定制的更复杂的度量来计算。

让我们参考下图了解聚类的工作机制：

![](img/b40816015a9835648eac2b186dac82d1_186_0.png)

图 6.1 – 聚类的工作机制

在了解聚类的工作机制后，让我们了解可以应用聚类的各种场景。

