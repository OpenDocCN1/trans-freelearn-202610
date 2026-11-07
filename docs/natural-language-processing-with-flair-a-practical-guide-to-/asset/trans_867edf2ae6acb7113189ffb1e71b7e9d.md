
## 4
## 序列标注

**序列标注** (或 **序列标记**) 指的是一组 **自然语言处理** (**NLP**) 任务，它们为文本的词元或其他单元分配标签或标记。当标签为命名实体时，我们就是在处理 **命名实体识别** (**NER**) 。当标签为词性时，此任务称为 **词性** (**PoS**) 标注。与以无监督方式训练的嵌入不同，序列标注器使用监督训练技术进行训练，使其更容易评估和比较。

**序列标注** 是 **Flair** 真正大放异彩的领域。**Flair** 利用 **Flair** 嵌入的巧妙设计（在上一章中解释），在许多不同的序列标注任务和语言中实现最先进的结果。

在本章中，我们将简要解释 **Flair** 中序列标注器的工作原理。这将使我们在介绍 **NER**、**PoS** 标注、分块以及 **Flair** 中找到的其他序列标注技术时，能够充分理解其内部工作原理。这些部分获得的知识将帮助您了解 **Flair** 解决的序列标注任务范围以及可供您使用的预训练模型。我们随后将展示在 **NLP** 中通常用于衡量和比较序列标注器性能的指标。

我们将在以下部分介绍 Flair 中的序列标注：

- 理解 Flair 中序列标注的工作原理
- Flair 中的命名实体识别
- Flair 中的词性标注
- Flair 中的其他序列标注器
- 序列标注指标

### 技术要求

对于本章，您需要一个安装了 Flair 版本 0.11 的 **Python** 环境。

本章涵盖的代码示例可在本书官方 GitHub 仓库的以下 **Jupyter Notebook** 中找到：

```
https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter04.
```

### 在 Flair 中理解序列标注

在直接深入了解如何在 Flair 中使用序列标注器之前，让我们先简要解释序列标注（也称为序列标记）的工作原理。从工程角度来看，序列标注器是一个工具，它接收文本作为输入并返回一系列分配，其中每个分配包含一个标签名称和一个跨度，指示标注的文本单元（通常是单词）的开始和结束位置。

Flair 中序列标注的架构和设计可以用下图最好地解释：

![](img/c56c48b8fd18a11ab705352d618ed976_62_0.png)

图 4.1 - Flair 中序列标注的概述

上图显示了由两个组件组成的序列标注架构：语言模型（黄色）和序列标注（标记）模型（蓝色）。双向字符语言模型接收原始文本作为输入，并为每个单词计算上下文嵌入（r_George, r_Washington, r_was, r_born）。然后将它们传递给 BiLSTM-CRF 序列标注器。这种特殊的标注器是一种新颖的序列标注方法，它使用双向 LSTM（BiLSTM）循环神经网络模型与条件随机场（CRF）——一种组合算法相结合。它输出与各个单词对应的标签的概率分布。然后为每个单词选择最可能的标签（如前图所示的顶行所示）。例如，在前图中，单词 Washington 被标记为 E-PER，其中 PER 表示该词与人相关，E 标记了双词命名实体块的结尾。单词通常还会被标记为特殊标签 O（意为其他），用于不被视为命名实体或不属于任何定义类别的命名实体的单词。

上图用最常见的序列标注应用之一——NER 说明了序列标注器的设计。让我们在下一节学习如何在 Flair 中使用它。

### 在 Flair 中进行命名实体识别

NER 是一种特殊的序列标注任务，用于识别和标记命名实体。命名实体是现实世界中的对象，例如人、地点或组织。可能的不同命名实体的集合没有严格定义，可能因模型而异。Flair 附带丰富的预训练 NER 模型，以及训练自定义模型的工具。在本节中，我们将重点介绍预训练模型，以及如何解释和理解其输出。

为了很好地理解 Flair 中 NER 标注的工作原理，让我们作为练习的一部分来尝试一下。我们将从标记一个明显是命名实体的单个单词开始。

```
from flair.data import Sentence
from flair.models import SequenceTagger

tagger = SequenceTagger.load('ner')
sentence = Sentence('Berlin')
tagger.predict(sentence)

print(sentence.to_tagged_string())
```

在前面的脚本中，我们通过调用 SequenceTagger.load('ner') 将 Flair 的默认英文 NER 标注器加载到内存中。然后我们用单个单词 Berlin 标记了一句话。最后，我们打印出了标记后的句子。输出如下：

```
Sentence: "Berlin" → ["Berlin"/LOC]
```

第一次使用 ner 标注器时，您还会看到下载进度条以及模型托管在 HuggingFace ModelHub 上的通知。

前面的 NER 标注代码片段打印出："Berlin"/LOC。这表明标注器认为在给定上下文（或缺乏上下文）中，单词 Berlin 是一个地点，因为它被标记为 LOC。

NER 类别集会因模型而异，即使在 Flair 内部也是如此，但大多数 NER 标注器会使用以下类别对实体进行分类：

- 人
- 地点
- 组织
- 杂项

让我们看另一个例子，这是介绍 Flair 给 NLP 世界的论文中使用的例子。它展示了一个案例，其中 Flair 出色的词义消歧性能真正大放异彩：

```
tagger = SequenceTagger.load('ner')
text = 'George Washington was born in Washington.'
sentence = Sentence(text)
tagger.predict(sentence)

print(sentence.to_tagged_string())
```

前面的脚本将打印：

```
Sentence: "George Washington was born in Washington ." → ["George Washington"/PER, "Washington"/LOC]
```

上述输出清楚地显示了 Flair 如何区分作为地点的 Washington（标记为 LOC）和作为人的 Washington（标记为 PER）。

### Flair 中的 NER 标注器类型

Flair 的预训练 NER 模型分为单语或多语 NER 模型。

**单语 NER 模型** 是在单一语言上训练的。它们经过调优以实现该单一语言的最佳性能，同时不提供对任何其他语言的支持。

最值得注意的预训练单语 NER 标注器如下：

- 在 Conll-03 数据集上训练的英文（4 类）NER 标注器。
  ID: ner.
- 在 Conll-03 数据集上训练的英文（4 类）NER 标注器，尺寸减小，速度提高，但准确率降低。
  ID: ner-fast.
- 在 Conll-03 数据集上训练的英文（4 类）NER 标注器，准确率提高，但速度降低且尺寸增加。
  ID: ner-large.
- 在 Ontonotes 数据集上训练的英文（18 类）NER 标注器。
  ID: ner-ontonotes.
- 在 Conll-03 数据集上训练的德文（4 类）NER 标注器。
  ID: de-ner.
- 在 WikiNER 数据集上训练的法文（4 类）NER 标注器。
  ID: fr-ner.
- 在 Conll-03 数据集上训练的西班牙文（4 类）NER 标注器。
  ID: es-ner-large.

与前面的单语模型不同，Flair 还支持多语 NER 模型。它们在每种特定语言上的准确率确实明显较低，但带来的好处是可以让我们使用单一模型处理多种语言。Flair 的多语 NER 模型在英语、德语、荷兰语和西班牙语上训练。这些模型可以使用 ner-multi 和 ner-multi-fast ID 加载。

您现在应该很好地了解如何在 Flair 中使用 NER 标注器并解释其结果。我们现在可以继续介绍标注器，它们不是标记命名实体，而是标记词性。
