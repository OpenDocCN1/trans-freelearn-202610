
其中：

- **真正例 (TP)**：被预测为正例且实际为正例的样本数量
- **假阳性 (FP)**：被预测为正例但实际为负例的样本数量
- **假阴性 (FN)**：被预测为负例但实际为正例的样本数量

F1 分数随后被定义为精确率和召回率的调和平均数：

$F1 = 2 \cdot \frac{precision \cdot recall}{precision + recall}$

计算 F1 分数的过程与序列标注中使用的方法相同，涉及以下步骤：

- 1. 统计真正例、假阳性和假阴性
- 2. 使用上述公式计算精确率和召回率
- 3. 将 F1 分数计算为精确率和召回率的调和平均数

如果我们分别对每个类别标签计算 F1 分数，然后对所有 F1 分数取平均，就得到 **宏 F1 分数**。该指标考虑了我们数据集中类别分布不平衡的情况。

如果我们对所有类别一起计算 F1 分数，将总的真正例、假阴性和假阳性合并计算，就得到 **微 F1 分数**。在单标签分类中，该指标的值与准确率相同。

让我们看看这一理论在实践中如何表现，使用最简单但功能强大的 NLP 库之一。

## Flair 中的文本分类

Flair 提供了使用预训练模型以及训练自定义文本分类器的简单接口。虽然 Flair 的秘诀——前向和后向 Flair 嵌入——并不是文本分类任务的最佳工具，但 Flair 可以利用其他第三方方法来实现文本分类的出色性能。

让我们先学习如何训练和使用 Flair 的预训练模型。

### 使用预训练的 Flair 文本分类模型

Flair 中可用的预训练文本分类模型集合相当小，很可能你想要的模型并不在其中。即使如此，以下语法仍然很有价值，因为加载和使用 Flair 预训练模型的 API 与加载和使用自定义训练模型的 API 相同。我们将在下一节中训练这些模型。

Flair 目前提供以下稳定的预训练模型：

- sentiment – 用于将英文文本分类为正面和负面的情感分析模型
- sentiment-fast – sentiment 文本分类模型的一个略小版本，推理速度更快
- de-offensive-language – 用于将德语文本标记为冒犯性或非冒犯性的文本分类模型——例如，模型会将包含粗俗用语的句子标记为冒犯性

了解 Flair 中当前可用的文本分类模型名称后，我们现在可以使用以下语法加载和使用模型：

```
from flair.models import TextClassifier
from flair.data import Sentence

classifier = TextClassifier.load("sentiment")
sentence = Sentence("Flair is pretty cool!")
classifier.predict(sentence)

print(sentence.labels)
```

前面的代码片段首先使用 TextClassifier.load() 函数通过传入 sentiment 模型名称来加载分类器。第一次加载此模型时，Flair 会从其 Web 服务器将其下载到我们的本地机器上。这些模型随后将被缓存到磁盘以供将来使用。

然后我们使用一个明确传达积极信息的句子初始化 Sentence 类。接下来，我们将使用 TextClassifier.predict() 方法生成预测并将其存储为 Sentence 对象的部分内容。

最后，我们只需打印出对象即可预测类别标签概率。它产生以下输出：

```
['Sentence: "Flair is pretty cool !"/'POSITIVE' (0.9991)]
```

前面的消息显示，我们的 Flair is pretty cool! 句子被分配了单个 POSITIVE 类别标签。该消息还表明我们的模型对其决策有 99.9% 的信心。

我们可以用一个极其负面的句子尝试完全相反的情况：

```
sentence = Sentence("I don't get enough sleep.")
classifier.predict(sentence)
print(sentence.labels)
```

这会打印出以下内容：

```
['Sentence: "I don't get enough sleep ."/'NEGATIVE' (0.9911)]
```

这表明我们的模型有 99.1% 的把握认为前一条消息具有负面含义。

就是这样！几行代码我们就可以进行文本分类。Flair 预训练文本分类模型的缺点是可供选择的模型实在少得可怜。幸运的是，Flair 提供了易于使用的接口来训练新的自定义模型，我们可以训练任意多个！让我们先学习如何利用 Flair 中执行文本分类所需的底层技术——文档嵌入——来学习如何做到这一点。

## Flair 中的文档嵌入

与词嵌入是 Flair 中生成序列标注模型的关键部分的方式相同，许多文本分类模型也利用了类似的概念——文档嵌入。

文档嵌入本质上是文档的向量表示。术语文档 simply 指的是一串词。这些文档的长度可以从单个词到整本书或语料库不等。

Flair 提供了一套不同类型的文档嵌入，如以下小节所述。

### DocumentPoolEmbeddings

这是最简单的文档嵌入类型。它是一段代码，为文档中的每个词计算词嵌入并返回平均值。例如，如果我们有一个包含两个词的文档，第一个词的嵌入为 [1, 2]，第二个词的嵌入为 [3, 0]，那么 DocumentPoolEmbeddings 嵌入将为 [2, 1]。除了计算平均嵌入（称为平均池化）之外，这种类型的文档嵌入还可以使用其他池化方式，例如最小池化（意味着对每个嵌入维度取最小值）和最大池化（意味着对每个嵌入维度取最大值）。要使用 DocumentPoolEmbeddings，你只需要训练好的词嵌入。文档嵌入本身不需要训练，这是一个巨大的优势！

```python
from flair.data import Sentence
from flair.embeddings import (DocumentPoolEmbeddings,
                               WordEmbeddings)

glove = WordEmbeddings('glove')
document_embeddings = DocumentPoolEmbeddings([glove])
sentence = Sentence('two words')
glove.embed(sentence)

print((sentence[0].embedding + sentence[1].embedding) / 2)
```

在前面的代码片段中，我们自己实现了使用平均池化的文档嵌入。我们只需加载一个词嵌入对象并为两个词（词 two 和词 words）计算词嵌入。然后我们将两个词嵌入相加并将结果嵌入除以二——从而对两个嵌入取平均或执行平均池化。我们可以使用 DocumentPoolEmbeddings 类完成完全相同的事情：

```python
document_embeddings = DocumentPoolEmbeddings([glove])
document_embeddings.embed(sentence)

print(sentence.embedding)
```

前面的代码片段使用 DocumentPoolEmbeddings 类为我们的文档计算嵌入。打印出嵌入的输出显示，结果与我们之前通过对两个词嵌入向量取平均计算出的嵌入相同。

但是使用 DocumentPoolEmbeddings 的便利性是有代价的——它们通常不能像以下嵌入方法那样提供良好的性能。

### DocumentRNNEmbeddings

DocumentRNNEmbeddings 是一种特殊的文档嵌入类型，由 Flair 实现，使用循环神经网络形成文档向量表示。

它通常比 DocumentPoolEmbeddings 产生更好的性能，并且以相同的方式接收词嵌入对象作为输入。然而，开箱即用的 DocumentRNNEmbeddings 是无用的——新初始化的嵌入对象本质上是一个具有随机权重的未训练神经网络模型。该模型需要训练后才有用。

但与其被训练另一种嵌入类型分散注意力，不如探索 Flair 中另一种文档嵌入，它既是预训练的，又能产生出色的性能。

### TransformerDocumentEmbeddings

解决任何问题的良好方法是不要重新发明轮子，而是利用已经有效的工作技术。近年来取得巨大进展的一种特殊 NLP 模型是 transformers。Flair 提供了一个包装类，能够直接从预训练的 Transformer 为整个文档计算嵌入。

TransformerDocumentEmbeddings 的使用方式与 Flair 中任何其他嵌入类相同——它可用于使用 .embed(sentence) 方法嵌入句子。让我们使用 roberta-base Transformer 模型尝试此嵌入：

```
from flair.embeddings import TransformerDocumentEmbeddings

embedding = TransformerDocumentEmbeddings('roberta-base')
sentence = Sentence('two words')

embedding.embed(sentence)

print(sentence.embedding)
```
