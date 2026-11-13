

```java
Dataset<Row> rescaledData = idfModel.transform(featurizedData);
rescaledData.select("label", "features").show(false);
```

Word2Vec 作为一个估计器，通过接受由句子或词序列组成的数据集值（这些值共同表示为文档）来生成 Word2VecModel。该模型通过使用文档中所有单词的平均值将文档转换为向量，从而生成一个固定大小的向量。Word2Vec 广泛用于预测、文档相似度计算等：

```java
List<Row> data = Arrays.asList(
    RowFactory.create(Arrays.asList("Learning Apache Spark")),
    RowFactory.create(Arrays.asList("Spark has API for Java")),
    RowFactory.create(Arrays.asList("API in above language"))
);
StructType schema = new StructType(new StructField[]{
    new StructField("text", new ArrayType(DataTypes.StringType, true))
});
Dataset<Row> documentDF = spark.createDataFrame(data, schema);
Word2Vec word2Vec = new Word2Vec()
    .setInputCol("text")
    .setOutputCol("result")
    .setVectorSize(10)
    .setMinCount(0);
Word2VecModel model = word2Vec.fit(documentDF);
Dataset<Row> result = model.transform(documentDF);
result.show(false);
```

| 特征提取 | CountVectorizer |
|---|---|
| | TF-IDF |
| | Word2Vec |
| 转换 | Tokenizer |
| | StopWordsRemover |
| | n-gram |
| | VectorAssembler 等等。 |
| 选择 | ChiSqSelector |
| | RFormula |
| | VectorSlicer |

## 特征转换器

特征转换器是通过缩放、转换或修改父数据框的特征来转换数据的转换器。一些常见的特征转换器如下：

- **Tokenizer**: Tokenizer 通过将句子或文档拆分为单个单词来转换它们。如下示例所示，数据框中的句子将被拆分为单词：

```java
List<Row> data = Arrays.asList(
    RowFactory.create(0, "Learning Apache Spark"),
    RowFactory.create(1, "Spark has API for Java, Scala, and R"),
    RowFactory.create(2, "API in above language are very rich")
);

StructType schema = new StructType(new StructField[]{
    new StructField("id", DataTypes.IntegerType, false, Metadata),
    new StructField("sentence", DataTypes.StringType, false)
});

Dataset<Row> sentenceDataFrame = spark.createDataFrame(data, schema);
Tokenizer tokenizer = new Tokenizer().setInputCol("sentence");
Dataset<Row> tokenized = tokenizer.transform(sentenceDataFrame);
tokenized.show(false);
```

Tokenizer 提供了一个非常直接的用途，即基于空格拆分句子，因此由于其简单性，有时其可用性会受到限制。RegexTokenizer 可用于克服基于正则表达式拆分单词的这一局限性。

- **StopWordsRemover**: 这是一个转换器，用于从文档中删除常用词，因为这些词对文档内容的贡献不大，而是对上下文有贡献，例如 is、a、the 等词。默认情况下，停用词删除的语言为英语，但可以使用 loadDefaultStopWords (languages) 方法进行更改，支持的语言包括英语、法语、意大利语、俄语等。还可以使用 stopWords 参数指定自定义停用词列表：

```java
List<Row> data = Arrays.asList(
    RowFactory.create(Arrays.asList("This", "is", "a", "book")),
    RowFactory.create(Arrays.asList("The", "language", "used"))
);

StructType schema = new StructType(new StructField[]{
    new StructField("raw", DataTypes.createArrayType(DataTypes.StringType, true))
});

StopWordsRemover remover = new StopWordsRemover()
    .setInputCol("raw")
    .setOutputCol("filtered");

Dataset<Row> dataset = spark.createDataFrame(data, schema);
remover.transform(dataset).show(false);
```

示例代码的输出为

| 原始 | 过滤后 |
|---|---|
| [This, is, a, book, on, Spark] | [book, Spark] |
| [The, language, used, is, Java] | [language, used, Java] |

- **VectorAssembler**: 这被用作一个转换器，用于将数据框中的多个列合并为单个向量列。在某些算法（如线性回归、逻辑回归或决策树）中，输入数据集预期具有一个单一特征向量，VectorAssembler 就在此类场景中发挥作用。但是，VectorAssembler 有一些限制，取决于输入列的数据类型，它接受所有数值、布尔型和向量数据类型：

```java
Dataset<Row> featSet = emp_ds.select(col("empId"), col("salary"), col("deptno"));
String[] featuresCols = featSet.columns();
VectorAssembler vectorAssembler = new VectorAssembler().setInputCols(featuresCols);
Dataset<Row> output = vectorAssembler.transform(featSet);
output.show(false);
```

## 特征选择器

数据框可能包含不需要的列，或者有时不够重要，无法以任何方式为标签计算做出贡献。特征选择器是帮助从更大的特征集中选择特征子集的转换器。常见的特征选择器如下：

- **ChiSqSelector**: ChiSqSelector 使用卡方独立性检验从特征向量中选择特征。卡方独立性检验的详细内容超出了本书的范围，但简单来说，选择器会根据 NumTopFeatures 参数的值动态选择最佳的特征数量，这将影响 ML 算法的预测输出：

```java
List<Row> data = Arrays.asList(
    RowFactory.create(1, Vectors.dense(0.1, 0.0, 1.0, 20.0)),
    RowFactory.create(2, Vectors.dense(0.2, 1.0, 13.0, 40.6)),
    RowFactory.create(3, Vectors.dense(0.3, 2.0, 21.0, 1.1))
);
StructType schema = new StructType(new StructField[]{
    new StructField("id", DataTypes.IntegerType, false, Metadata),
    new StructField("rawfeatures", new VectorUDT(), false),
    new StructField("label", DataTypes.DoubleType, false, Metadata)
});

Dataset<Row> df = spark.createDataFrame(data, schema);

ChiSqSelector selector = new ChiSqSelector()
    .setNumTopFeatures(1)
    .setFeaturesCol("rawfeatures")
    .setLabelCol("label")
    .setOutputCol("features");

Dataset<Row> result = selector.fit(df).transform(df);
result.show();
```

- **VectorSlicer**: 这是一个接受特征向量并返回原始特征向量子集的特征向量的转换器。当原始特征向量中包含对数据集上应用的算法无关的特征时，需要此类转换。可以将列索引或名称传递给 VectorSlicer 以选择子集特征向量，但是，为了使用名称过滤特征，需要先使用 setNames() 方法设置特征名称：

```java
Dataset<Row> featSet = emp_ds.select(col("empId"), col("salary"), col("deptno"));
String[] featuresCols = featSet.columns();
Dataset<Row> vecAssembler = new VectorAssembler().setInputCols(featuresCols);
VectorSlicer vectorSlicer = new VectorSlicer().setInputCol(featuresCols);
vectorSlicer.setIndices(new int[]{1, 2});
Dataset<Row> output = vectorSlicer.transform(vecAssembler);
output.show(false);
```

## 摘要

本章讨论了机器学习作为一门研究领域及其各种子分类。接着，我们进一步讨论了与机器学习相关的各种术语，以及机器学习在 Apache Spark 中的实现方式。我们还介绍了 spark.mllib 包中使用的数据类型，然后讨论了使用管道进行作业链。管道相关的术语也详细讨论了，并结合了用例。最后，讨论了特征上的操作。

在下一章中，我们将深入探讨 Spark 的另一个模块，即 GraphX，并了解 GraphX RDD 的类型及其相关的各种操作。我们还将讨论围绕 GraphX 实现的用例。

