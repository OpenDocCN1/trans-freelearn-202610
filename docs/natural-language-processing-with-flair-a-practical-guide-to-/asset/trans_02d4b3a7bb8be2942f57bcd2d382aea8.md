
我们现在可以测试命名实体识别组件的性能如何：

-   我们首先需要实例化 FlairTrader 对象。我们将使用 amazon 作为我们想要投资的公司名称进行测试。这仅仅是因为 Amazon 可能与亚马逊雨林或亚马逊河等地理位置混淆。我们可以通过运行以下代码来定义 FlairTrader 对象：

```python
ft = FlairTrader("amazon")
```

-   这现在允许我们通过运行以下内容来测试 NER 组件：

```python
print(ft._references_company("amazon river is deadly"))
```

前面的代码块打印出 False。理所当然，它表明在给定上下文中，单词 amazon 与亚马逊公司无关。让我们再试一个例子：

```python
print(ft._references_company("amazon is underpaying staff"))
```

前面的代码块打印出 True。Flair 正确地判断出前一句确实与亚马逊公司相关。再试一个：

```python
print(ft._references_company("amazon deforestation is bad"))
```

这个前面的代码块不出所料打印出 False。

在前面的代码中，我们可以清楚地看到，我们的命名实体识别组件在区分引用亚马逊河或亚马逊雨林的文本与讨论亚马逊科技公司的文本方面表现出色。

-   我们现在可以实现情感分析组件，其职责正如名称所示，进行情感分析并判断我们是否应该买入、卖出或持有。该方法将被优化，以便只有当我们认为输入文本与我们想要交易的公司相关时才会执行情感分析。如果不相关，它将返回 hold（意味着我们既不买也不卖股票）：

```python
def buy_or_sell(self, text):
    if self._references_company(text):
        sentence = Sentence(text)
        self.classifier.predict(sentence)
```

```python
if sentence.labels[0].value == "POSITIVE":
    return "buy"
elif sentence.labels[0].value == "NEGATIVE":
    return "sell"
else:
    return "hold"
```

该组件是一个名为 buy_or_sell(self, text) 的类方法。它接收文本作为输入，如果文本与我们的公司相关，则执行情感分析。如果情感分析分类器认为文本为正面，我们就买入。如果文本为负面，我们就卖出。如果文本与我们的公司不相关，我们就持有。这里需要注意的是，我们完全忽略了分类器的概率。有时分类器对其结果有 100% 的把握，有时它只是在做 51% 的猜测。好的策略会利用这些信息并使用一定的概率阈值。

> **Important Note**
> Make sure to add the preceding method to the FlairTrader class definition and not define it as a standalone piece of code.

我们的交易策略类现在已完成！

我们现在可以通过使用另一个公司名称，例如 Apple 来尝试它：

```python
ft = FlairTrader("apple")
```

让我们看看我们的交易策略表现如何：

```python
print(ft.buy_or_sell("this year's apple harvest"))
```

前面的代码块打印出 hold，这是有道理的。文本谈论的是苹果作为水果，而不是 Apple 公司，这意味着我们没有获得关于做出什么交易决策的信息。

现在，让我们试一个正面情感的例子：

```python
print(ft.buy_or_sell("apple stock is doing great"))
```

前面的代码块不出所料打印出 buy。

我们现在可以尝试一个稍微不那么直接的正面例子：

```python
print(ft.buy_or_sell("apple shows steady growth"))
```

该代码块也打印出 buy。它表明 Flair 能够捕捉到文本中更微妙的正面信号。

下面这个代码块，值得庆幸的是，打印出 sell：

```python
print(ft.buy_or_sell("apple is being sued by the EU"))
```

前面的例子展示了我们的交易策略在买入、卖出或持有股票的决策方面有多好。它在输入文本上表现尤其出色，例如新闻文章标题，其中英语单词通常大写，无法从大写推断含义。

在合适的每日新闻文章源下，FlairTrader 类为我们提供了一个出色的交易机器人基础，可在现实世界中使用。但在把全部积蓄投入这个交易机器人之前，你应该意识到现实世界的股票交易通常比看起来更复杂。好的交易机器人通常依赖于多种不同的指标，很少依赖单一的事实来源，例如新闻文章情感。一种方便的测试交易策略而无需投入真钱的方法是使用回测，其中策略在数月或数年的历史交易数据上进行测试，这让我们在投入真实资金之前对交易策略在现实世界中的预期有一个大致的了解。如果你想深入研究使用 Python 等语言进行交易的领域，你应该尝试 Packt 出版的优秀技术书籍《Python for Finance》。

但无论你的机器人在现实世界中是盈利还是亏损，实现过程清楚地展示了使用 Flair 如何让我们仅用几行简单的代码就能构建智能且功能强大的应用程序。

## Flair 编程速查表

在结束之前，我们应该快速回顾一下本书中使用的最重要的 Flair 类：

| Class | Description |
|---|---|
| Sentence | 用于存储文本的类，可以是单个单词、句子或整个段落。也包含与该对象所属 token 关联的潜在标签。 |
| Token | 表示 token 及其对应标签和嵌入的类。 |
| SpaceTokenizer | 仅基于空格字符的 tokenizer。 |
| WordEmbeddings | 静态词级嵌入。 |
| FlairEmbeddings | 最先进的 Flair 嵌入。 |
| StackedEmbeddings | 用于生成一个或多个嵌入的堆叠的类。 |
| TransformerDocumentEmbeddings | 基于 Transformer 的文档嵌入。 |
| SequenceTagger | 用于加载序列标注器并执行序列标注的类。 |
| Corpus | 用于存储数据集并执行简单操作的类。 |
| ModelTrainer | 负责在 Flair 中训练模型的类。 |
| SequenceTaggerParamSelector | 超参数选择类。 |
| LanguageModel | 用于定义和存储语言模型的类。 |
| LanguageModelTrainer | 负责训练语言模型的类，语言模型最常用于生成嵌入。 |
| TextClassifier | 用于加载和使用文本分类模型的工具集。 |
| TARSClassifier | 用于零样本和少样本文本分类的类。 |

这个常用的 Flair 类和命令的简短概述希望能作为本书中使用的最重要的 Flair 命令的易于记忆的列表。

## 总结

这个简单有趣的动手练习结束了我们对最先进的 NLP 库 Flair 的探索之旅。本书开始时，我们从一些动机开始，随后介绍了 Flair 基础类型，这些类型奠定了 Flair 中不同对象通常如何相互交互的基础。然后我们介绍了词嵌入和文档嵌入，它们是 Flair 的重要组成部分，也是其标注器在序列标注任务上实现最先进性能的主要原因之一。这使我们能够进入序列标注本身，在那里我们了解了 Flair 中所有不同类型的序列标注器。然而，我们并不局限于仅重用预训练模型；我们在下一章中很快学会了如何训练自己的序列标注器。这让我们被漫长的训练超参数列表所淹没，我们必须找到一种利用机器学习帮助我们找到最佳参数集的方法。我们通过掌握超参数调优并学习如何在 Flair 中进行它来实现这一点。
