
Flair 的优秀 OOV 词性能可以在以下实验中观察到。我们将为一个正确拼写的词获取一个嵌入，例如词 `potato`。然后，我们将获取其拼写错误版本的嵌入，例如词 `potatoo`。然后我们将使用余弦相似度来计算这两个词在语义上的相似程度。我们的假设是，一个好的嵌入模型能够识别出第二个词与第一个词非常相似。余弦相似度大于 0.8 可以合理地被视为相似嵌入的指标：

```
from sklearn.metrics.pairwise import cosine_similarity as sim

s1 = Sentence("eating potato")
s2 = Sentence("eating potatoo")

embedding = FlairEmbeddings('news-forward')
embedding.embed(s1)
embedding.embed(s2)
e1 = s1[1].embedding.tolist()
e2 = s2[1].embedding.tolist()

print(sim([e1], [e2]))
```

前述代码打印出 `eating potato` 上下文中词 `potato` 与 `eating potatoo` 上下文中词 `potatoo` 之间的余弦相似度。打印出的相似度分数为 0.865，表明这两个词嵌入之间存在强关联。

## 池化 Flair 嵌入
为了获得更好的性能，您还可以使用 `PooledFlairEmbeddings`。它使用完全相同的语法接口并支持与 `FlairEmbeddings` 相同的模型集合，但通常能提供轻微的性能提升。请注意，`PooledFlairEmbeddings` 需要大量内存，这意味着性能的边际提升可能并不总是值得额外的内存消耗。

## 可用的 Flair 嵌入
框架中支持的 Flair 嵌入集合不断变化，到您阅读本书时，完整的列表很可能已经过时。但通常，您很可能在以下某一类别中找到所需的嵌入：
- 在 10 亿词语料库上训练的英语 Flair 嵌入：
  - ID: `news-DIRECTION`
- 在 Wikipedia、Opus 或其他大型语料库上训练的 Flair 嵌入：
  - ID: `LANGUAGE_CODE-DIRECTION`。
  - 例如，对于德语 Flair 嵌入的前向版本，使用 `de-forward`。
- 在生物医学 PubMed 摘要上训练的 Flair 嵌入：
  - ID: `pubmed-DIRECTION`
- 支持 300 多种语言的多语言 Flair 嵌入：
  - ID: `multi-DIRECTION`

请注意，使用嵌入 ID 时，术语 `DIRECTION` 需要替换为 `backward` 或 `forward`。

若要获取支持的 Flair 嵌入的完整且最新的列表，您应查阅 Flair 的官方文档 at https://github.com/flairNLP/flair/blob/master/resources/docs/embeddings/FLAIR_EMBEDDINGS.md。

在前面的练习中，我们介绍了 Flair 嵌入的基本用法。但您可能问过自己：如何使用 Flair 嵌入以利用出现在词前后完整上下文的方式？这些问题以及许多其他问题将在下一节中得到解答。

## 堆叠嵌入
**堆叠嵌入** 是一种元嵌入类型，允许我们通过组合两个或多个嵌入来形成新的嵌入。这些元嵌入，正如名称所示，只是将多个嵌入堆叠在一起。它们非常适合组合 Flair 嵌入的前向和后向版本。它们还可以用于添加其他嵌入类型，例如将上下文字符串嵌入与经典词嵌入混合。事实上，这正是 Flair 建议的组合类型，并且通常能产生最先进的结果。

要使用堆叠嵌入，只需实例化一个 StackedEmbeddings 类，并传入一个嵌入对象列表。然后您就可以以与在 Flair 中使用任何其他嵌入完全相同的方式使用此元嵌入。

让我们在 Flair 推荐的最佳性能嵌入集合上尝试一下。我们将堆叠 glove 词嵌入与 news-forward 和 news-backward Flair 嵌入：

```python
from flair.embeddings import FlairEmbeddings, WordEmbeddings
from flair.embeddings import StackedEmbeddings

glove = WordEmbeddings('glove')
news_fw = FlairEmbeddings('news-forward')
news_bw = FlairEmbeddings('news-backward')

combined_embeddings_list = [glove, news_fw, news_bw]

stack = StackedEmbeddings(combined_embeddings_list)
```

然后您可以在 Sentence 对象上运行 stack.embed(sentence)，就像使用 Flair 中的任何其他嵌入类型一样。

我们现在已经介绍了 Flair 中的主要词嵌入类型。现在是时候提升一个层次，学习如何计算的不仅是词的嵌入，还有整个文档的嵌入。

## 文档嵌入
文档嵌入与词嵌入类似，唯一的区别是，我们不是为每个词获取一个嵌入，而是为整个文档获取一个嵌入。

在 Flair 中，文档可以在 Sentence 对象内部定义。不是通过 sentence[n].embedding 访问嵌入（这将返回第 n 个词的嵌入），我们只需运行 sentence.embedding 或 sentence.get_embedding()。

Flair 目前支持以下嵌入类型：
- TransformerDocumentEmbeddings, 使用预训练的 Hugging Face transformers 的文档嵌入类。
- DocumentPoolEmbeddings, 一个元文档嵌入类，接收词嵌入对象，计算每个词的嵌入，并返回所有词嵌入的平均值。
- DocumentRNNEmbeddings, 一个元文档嵌入类，接收词嵌入对象，在整个文档上训练循环神经网络，并将最终状态作为该文档的嵌入返回。
- SentenceTransformerDocumentEmbeddings, 使用外部 sentence-transformers 库的文档嵌入类。该库需要单独安装。

Flair 建议在文档分类任务中使用 TransformerDocumentEmbeddings 类。让我们尝试一下：

```
from flair.data import Sentence
from flair.embeddings import TransformerDocumentEmbeddings

embedding = TransformerDocumentEmbeddings('bert-base-uncased')

sentence = Sentence('Example sentence .')
embedding.embed(sentence)

print(sentence.embedding)
```

前述代码将打印出代表我们句子的单个嵌入。

请注意，在前述代码示例中，我们将 bert-base-uncased 嵌入 ID 传递给了 TransformerDocumentEmbeddings 类。这是我们决定使用的变换器模型名称。您可以从 Hugging Face transformer 模型库中选择任何其他兼容模型。

## Flair 中的其他嵌入
在本章中，我们介绍了 Flair 最先进性能背后的大多数嵌入类型和技术，尽管本书中仍有许多其他嵌入类型未作解释。

包括以下内容：
- ELMoEmbeddings: 使用双向 LSTM 的上下文嵌入
- OneHotEmbeddings: 每个词都是 one-hot 向量的嵌入
- BytePairEmbeddings: 在子词级别预计算的嵌入
- `TransformerWordEmbeddings`: 使用基于变换器架构的嵌入
- `CharacterEmbeddings`: 每次初始化类时随机设置的字符级嵌入

虽然这些嵌入的内部工作原理超出了本书的范围，即使不完全理解它们的工作原理，您现在也应该能够像使用 Flair 中的任何其他嵌入对象一样使用它们，因为它们都共享相同的接口。

## 总结
在本章中，我们介绍了 Flair 中的嵌入。它们是所有 NLP 任务的核心概念。我们通过动手实践练习了解了嵌入的行为方式、其作用以及如何在 Flair 中使用它们。

我们实现了一个 NLP 类比求解器，可以使用算术运算和词嵌入之间的关系预测类比中的最后一个词。通过这样做，我们不仅复习了如何在实践中使用 Flair 的基本类型，还说明了词嵌入是什么以及如何从中推导出含义。然后我们还介绍了 Flair 中的所有嵌入类型。我们首先介绍了经典词嵌入，然后是 Flair 嵌入。

使用元嵌入方法，我们使用了堆叠嵌入并将多个嵌入组合成新的嵌入——这是一种在 Flair 中实现最先进结果的技术。然后我们获得了在 Flair 中形成文档嵌入的实践经验，并以超出本书范围的嵌入类型列表结束了本章。

在下一章中，我们将学习 Flair 如何利用这些嵌入来生成 NLP 中性能最佳的序列标注器之一。
