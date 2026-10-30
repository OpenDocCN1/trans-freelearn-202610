

我们现在已经到达了关于数据准备和转换的这个漫长——但非常重要——章节的结尾。让我们借此机会快速回顾一下我们所学到的精彩内容。

## Summary

首先，你被介绍了你可能需要处理的不同类型的特征。识别你将要处理的变量类型对于定义可以应用于每种情况的转换类型和技术非常重要。

然后，我们学习了如何处理分类特征。我们看到，有时分类变量确实有顺序（例如有序变量），而有时则没有（例如名义变量）。你了解到独热编码（或哑变量）可能是名义特征最常见的转换类型；然而，根据唯一类别的数量，应用独热编码后，你的数据可能会出现稀疏性问题。关于有序特征，你不应该在其上创建哑变量，因为你会丢失已纳入变量中的顺序信息。在这些情况下，有序编码是最合适的转换。

我们继续旅程，查看数值特征，在那里我们学习了如何处理连续和离散数据。我们介绍了最重要的转换类型，例如归一化、标准化、分箱和离散化。你看到某些类型的转换依赖于底层数据来找到其参数，因此避免使用测试集从数据中学习任何东西非常重要（它必须严格用于测试）。

你还看到我们甚至可以对数据应用纯数学进行转换；例如，你了解到幂变换可用于减少特征的偏度，使其更接近正态。

接下来，我们研究了缺失数据，并对该任务的重要性有了认识。当你在建模时，你不能将缺失值视为一个简单的计算问题，即只需用 y 替换 x。这是一个更大的问题，你需要通过探索数据开始解决，然后检查你的缺失数据是否随机生成。

当你做出删除或替换缺失数据的决定时，你必须意识到你是在丢失信息还是在给数据添加偏差，分别而言。请记住回顾我们给你的所有重要说明，因为它们很可能在某种程度上出现在你的考试中。

接下来，你学习了异常值检测。你查看了寻找异常值的不同方法，例如 zscore 和箱线图方法。最重要的是，你学习了你可以对其进行标记或平滑处理。

在开始时，我告诉你这一章将是一次漫长但值得的数据准备之旅，这就是为什么我需要让你很好地了解如何处理罕见事件，因为这是 ML 上最具挑战性的问题之一。你了解到，有时你的数据可能不平衡，你必须欺骗你的算法（通过改变类别权重）或对数据进行重采样（应用欠采样和过采样）。

最后，你学习了如何处理 NLP 的文本数据。你现在应该能够手动计算词袋和 TF-IDF 矩阵！我们更深入地学习了词嵌入的工作原理。在这一小节中，我们了解到我们可以创建自己的嵌入空间（使用许多不同的方法）或重用预训练的嵌入，例如 BERT。

我们完成了！我很高兴你坚持到了这里，我相信这一章对你在考试中的成功至关重要。最后，我们为你准备了一些练习题；我希望你喜欢它们。

在下一章中，我们将深入数据可视化技术。

## Questions

1. 你作为一家医疗保健公司的数据科学家，正在创建机器学习模型来预测公司索赔中的欺诈、浪费和滥用。该模型的一个特征是特定药物在 2 年期间内对索赔的同一患者开出的次数。哪种类型的特征是这种特征？

- a) Discrete
- b) Continuous
- c) Nominal
- d) Ordinal

**Answer**
a, The feature is counting the number of times that a particular drug has been prescribed. Individual and countable items are classified as discrete data.

2. 你正在为一家在全球拥有学校和大学的教育集团构建 ML 模型。你的模型旨在预测特定学生辍学的可能性。许多因素可能导致辍学，但你的一个特征是每个学生的当前学术阶段：学前班，小学，初中或高中。哪种类型的特征是这种特征？

- a) Discrete
- b) Continuous
- c) Nominal
- d) Ordinal

Answer
d, The feature has an implicit order and should be considered a categorical/ ordinal variable.

3. 你正在为一家汽车保险公司构建机器学习模型。该公司希望创建一个二元分类模型，旨在预测其投保车辆被盗的可能性。你考虑了许多特征，包括车辆类型（经济型、紧凑型、高端型、豪华型、厢式车、运动型和敞篷车）。你将如何转换车辆类型以便在模型中使用？

- a) Applying ordinal encoding
- b) Applying one-hot encoding
- c) No transformation is needed
- d) Options A and B are valid transformations for this problem

Answer
b, In this case, we have a categorical/nominal variable (there is no order among each category). Additionally, the number of unique categories looks pretty manageable; furthermore, one-hot encoding would fit very well for this type of data.

4. 你作为一家金融公司的数据科学家。该公司希望创建一个模型来分类不当付款。你决定使用“交易类型”作为你的特征之一（本地、国际、预批准等）。在对该变量应用独热编码后，你意识到你的数据集有更多变量，你的模型训练时间很长。你如何可能解决这个问题？

- a) By applying ordinal encoding instead of one-hot encoding. In this case, we would be creating just one feature.
- b) By applying label encoding.
- c) By analyzing which types of transactions have the most impact on improper/proper payments. Only apply one-hot encoding to the reduced types of transactions.
- d) By porting your model to another programming language that can handle sparse data in a better way.

Answer
c, Your transformation resulted in more features due to the excessive number of categories in the original variable. Although the one-hot encoding approach looks right, since the variable is a nominal feature, the number of levels (unique values) for that feature is probably too high.

In this case, you could do exploratory data analysis to understand the most important types of transactions for your problem. Once you know that information, you can then restrict the transformation to just those specific types (reducing the sparsity of your data). It is worth adding that you would be missing some information during this process because now, your dummy variables would only be focusing only on a subset of categories, but it is a valid approach.

5. 你作为一家营销公司的数据科学家。你的公司正在构建一个聚类模型，用于细分客户。你决定对“年收入”变量进行归一化，其范围在 50,000 到 300,000 之间。

应用归一化后，收入为 50,000、125,000 和 300,000 的客户组的归一化值是多少？

- a) 1, 2, and 3
- b) 0, 0.5, and 1
- c) 0, 0.25, and 1
- d) 5, 12, and 30

Answer
b. Applying the normalization formula and assuming the expected range would be 0 and 1, the correct answer is b.

6. 考虑一个数据集，其中存储员工薪水的列。该列薪水的平均值是 $2,000，而标准差等于 $300。某人赚 $3,000 的标准化值是多少？

- a) 3.33
- b) 6.66
- c) 10
- d) 1

Answer
a, Remember the standard scale formula: (X - μ) / σ, which is (3,000 – 2,000) / 300.

7. 我们可以应用哪种类型的数据转换将连续变量转换为二元变量？

- a) Binning and one-hot encoding
- b) Standardization and binning
- c) Normalization and one-hot encoding
- d) Standardization and one-hot encoding

Answer
a, In this case, we could discretize a continuous variable by applying binning and then getting the dummy variables.

