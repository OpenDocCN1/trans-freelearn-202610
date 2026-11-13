

Pipeline lrPipeline = new Pipeline().setStages(new PipelineStage[]
{vectorAssembler, lr});
// Train model.
PipelineModel lrModel = lrPipeline.fit(data[0]);
// Make predictions.
Dataset<Row> lrPredictions = lrModel.transform(data[1]);
// Select example rows to display.
lrPredictions.show(20);

在继续之前，你应该注意到预测结果并不十分准确。这是因为：

- 我们的机器学习模型非常简单，即使线性回归的定义也只考虑了最简单的可能性 Linear Regression，且该模型是一个线性方程
- 样本量非常有限，总共只有约 500 条记录，其中 350 条作为训练数据
- 我们没有将详尽的参数列表作为特征向量包含在内
- 我们也没有专注于模型选择或参数调优

现在让我们来讨论上述每个原因的解决方案：

- 1. 我们对使用机器学习算法的理解应当非常精确，例如，在 Spark 2.0 中，线性回归有两种变体，一种称为 **线性回归**，另一种称为 **广义线性回归**。不仅如此，在广义线性回归下，还有各种实现选项，例如高斯、二项分布、泊松、伽马和 Tweedie。因此，至关重要的是正在使用哪种类型的机器学习算法，以及该算法是否存在相关的子类型。如果存在，那么哪种变体最适合我们的用例。这个问题没有简单直接的答案；然而，数据科学的领域知识无疑将有助于在决定选择哪种模型时得出更好的结论。
- 2. 样本的选择应当足够详尽，以涵盖功能的大部分，如果不是全部可能场景的话。
- 3. 机器学习算法的选择起着重要作用；同样重要的是代表特征向量的参数的选择。例如，在前面的用例中，我们没有涵盖发布消息的用户的性别，但有些研究表明，用户的性别在生成展示量（即评论、点赞或分享）方面也起着重要作用。另一个重要方面是在特征向量中添加或删除人为偏差因素。微软推出了两款自学习聊天机器人 Tay 和 Zo，但它们惨败了，因为本应让它们从无法由公司控制的交互中学习。这对他们来说一直是个令人担忧的问题，整个场景也应引起任何机器学习算法开发者的注意，开发者需要有意识地决定是否要在特征向量中添加人为偏差。
- 4. 每个机器学习工作流都有自己的一组可调优参数，可以对其进行微调以生成一组或另一组结果。哪种参数组合适合任何用例的需求，既取决于经验，也取决于我们围绕各种参数进行调整并评估结果的能力。

在本节中，我们讨论了使用管道和链接各种组件来实现机器学习工作流。在下一节中，我们将讨论各种提取、转换和选择特征的技术。

## 特征向量上的操作

虽然 spark.ml 包将数据帧用于机器学习工作流，但取决于用例，可能需要从原始数据帧中提取数据，或将数据帧转换为机器学习算法所需的格式，有时可能只需要少量选定的参数作为特征向量。所有这些不同类型的操作都需要使用专门开发的 API，这些 API 可以分为以下几类。

## 特征提取器

当原始数据帧中的数据不以机器学习算法期望的形式明确存在时，我们使用特征提取器来提取这些特征。常见的特征提取器有：

- **计数向量器**：CountVectorizer 将文档集合转换为表示文档中单词计数的向量。CountVectorizer 的工作方式有两种，取决于词典值的填充方式。首先假设用户没有关于将填充文本数据集的数据类型的先验信息；在这种情况下，词典是根据数据集中的术语频率出现情况来准备的。VocabSize 定义词典可以容纳的单词数量，而可选参数 minDF 则限制词典中单词的加入，如果该单词在文档中的出现次数少于 minDF 的值：

```
List<Row> data = Arrays.asList(
RowFactory.create(Arrays.asList("w1", "w2", "w3")),
RowFactory.create(Arrays.asList("w1", "w2", "w4", "w5", "w2"))
);
StructType schema = new StructType(new StructField[]{
new StructField("text", new ArrayType(DataTypes.StringType, true), false)
});
Dataset<Row> df = spark.createDataFrame(data, schema);

CountVectorizerModel cvModel = new CountVectorizer()
.setInputCol("text")
.setOutputCol("feature")
.setVocabSize(5)
.setMinDF(2)
.fit(df);

System.out.println("The words in the Vocabulary are :: " +
cvModel.transform(df).show(false));
```

上述代码的输出如下：

| text | feature |
| --- | --- |
| [w1, w2, w3] | (2,[0,1],[1.0,1.0]) |
| [w1, w2, w4, w5, w2] | (2,[0,1],[2.0,1.0]) |

当未定义词典时，CountVectorizer 会遍历数据集两次以根据频率和大小准备词典。如果知道构成词典的单词值，则可以优化准备特征向量的过程，从而节省一次遍历。在此场景中，在实例化 CountVectorizer 时会构建一个 _先验_ 词典，因此如果在与之前示例相同的数据集上运行以下转换：

```
CountVectorizerModel cvMod = new CountVectorizerModel(new String[]{"w1", "w2"})
.setInputCol("text")
.setOutputCol("feature");
System.out.println("The words in the Vocabulary are :: " +
cvMod.transform(df).show(false));
```

我们得到以下输出：

词汇表中的单词为：[w1, w2, w3]

| text | feature |
| --- | --- |
| [w1, w2, w3] | (3,[0,1,2],[1.0,1.0,1.0]) |
| [w1, w2, w4, w5, w2] | (3,[0,1],[1.0,2.0]) |

- **词频-逆文档频率（TF-IDF）**：它被用作一种特征提取技术。TF-IDF 用于计算文档中单个单词的权重，而不是整个数据集。这里的权重是指统计评估，用于衡量集合或数据集中文档内单词的重要性。随着文档中单词频率的增加，其重要性也随之提高，但在 TF-IDF 中，这会被该单词在整个集合或数据集中的频率所抵消。TF-IDF 由两部分组成，即：

- **术语频率（TF）**：它是衡量术语在文档中出现频率的统计量。例如，如果在一个文档中 spark 出现五次，而该文档中的单词总数为 20，则：

TF= 文档中单词出现的频率 / 文档中的单词总数
=5/20=0.25

在 Spark ML 库中，可以使用 `HashingTF` 或 `CountVectorizer` 来计算 TF：

- **逆文档频率（IDF）** 衡量术语在集合或整个数据集中的重要性。它考虑了这样一个事实：某些术语，如 *is, are, the* 等，尽管出现频率较高，但就数据集而言，其重要性很小。因此，如果单词 spark 出现在考虑数据集的 100,000 个文档中的 100 个文档中，则：

IDF= loge(文档总数/包含特定术语或单词的文档数)
= loge(10000/100) = 4.6
因此 TF-IDF 的值 = TF*IDF
= 0.25*4.6=1.15

在 Spark 中，IDF 是一个估计器，通过在数据集上使用 `fit()` 方法生成 IDFModel。然后使用 `transform()` 方法将由 `HashingTF` 或 `CountVectorizer` 创建的特征向量传递给它，该方法会重新缩放或偏移原始向量值：

```
List<Row> data = Arrays.asList(
RowFactory.create(0.0, "Spark"),
RowFactory.create(0.0, "Spark"),
RowFactory.create(0.5, "MLIB"),
RowFactory.create(0.6, "MLIB"),
RowFactory.create(0.7, "MLIB"),
RowFactory.create(1.0, "ML")
);
StructType schema = new StructType(new StructField[]{
new StructField("label", DataTypes.DoubleType, false, Metadata.empty),
new StructField("words", DataTypes.StringType, false, Metadata.empty)
});
Dataset<Row> rowData = spark.createDataFrame(data, schema);
Tokenizer tokenizer = new Tokenizer().setInputCol("words").setOutputCol("tokenizedWords");
Dataset<Row> tokenizedData =
tokenizer.transform(rowData);
int numFeatures = 20;
HashingTF hashingTF = new HashingTF()
.setInputCol("tokenizedWords")
.setOutputCol("rawFeatures")
.setNumFeatures(numFeatures);
Dataset<Row> featurizedData = hashingTF.transform(tokenizedData);
IDF idf = new IDF().setInputCol("rawFeatures").setOutputCol("features");
IDFModel idfModel = idf.fit(featurizedData);

