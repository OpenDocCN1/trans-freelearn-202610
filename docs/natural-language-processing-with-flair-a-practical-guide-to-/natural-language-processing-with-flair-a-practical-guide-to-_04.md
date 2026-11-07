## 5 训练序列标注模型

在本书中，迄今为止我们已经介绍了一些 Flair 开箱即用的功能和模型。预训练序列标注器、嵌入和其他模型的集合乍一看似乎足够大，以至于我们永远不需要 Flair 中已有的东西之外的任何东西。但如果您从事 **自然语言处理 (NLP)** 工作足够长的时间，您很可能会遇到一个问题，其通用性不足以用预训练模型解决。面对此类问题，我们通常需要自行训练模型。此过程涉及获取训练数据、预处理它，可能手动标注它，最后使用 NLP 框架训练模型。但由于 Flair 提供了大量标注语料库，您可能只需要执行最后一步，即模型训练——本章的重点。

我们将从解释何时以及为何需要训练自定义模型开始本章。接下来，我们将介绍训练序列标注模型的硬件要求，并展示在云中训练模型的免费和付费方法。

然后我们将简要介绍神经模型训练的过程。之后，我们将查看控制训练的指标和参数。我们将在一个动手练习中介绍实际的模型训练语法，在该练习中我们训练一个英语词性标注器。最后，我们将学习一些评估模型质量和运行可视化的基本方法，这将帮助我们判断模型是否足够好，或者是否应该继续训练或从头开始。

在本章中，我们将作为以下部分的一部分介绍上述主题：

- 训练自定义模型的动机
- 了解训练模型的硬件要求
- 处理训练和评估参数
- 训练自定义序列标注模型
- 知道何时停止并重试

## 技术要求

本章使用的所有 Python 库都是 Flair 0.11 的直接依赖项，在假设 Flair 已安装在您的机器上的情况下无需特殊设置。本章介绍的代码示例可在本书的官方 GitHub 仓库中的以下 Jupyter notebook 找到：https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter05。

在启用 GPU 的机器上运行本章介绍的代码不是强制性的，但建议这样做，因为它将加快训练过程并允许您训练更高质量的模型。

## 训练自定义模型的动机

在本书中，我们迄今为止讨论了 Flair 开箱即用的功能和模型。然而，如果您从事 NLP 工作足够长的时间，您很可能会遇到一个序列标注问题，其复杂性或特殊性足以导致没有可用的预训练模型。这可能发生在以下任一情况下：

- *您解决的问题是领域特定的*：诸如 **命名实体识别 (NER)** 或 **词性 (PoS)** 标注器之类的序列标注器通常是在大型通用语料库上训练的，这些语料库旨在代表语言的一般用法。但如果我们的问题是领域特定的，那么我们很可能需要一个使用领域特定语料库训练的、具有领域特定标签的自定义标注器。
- *存在预训练模型但性能不足*：Flair 中提供的每个模型以及用于训练的方法通常都会由其他贡献者审查，但完全有可能您自己构建更好的模型。机会随着您转向不太流行的语言而增加，因为它们的性能不会得到同等优化。也可能发生预训练模型在 F1 分数或准确率方面总体表现更好，但在您真正关心的那个特定标签的标注上却有所不足。




- *你正在使用的语言使用范围较窄且不被框架支持：* 当谈到 NLP 的进步时，最新的、最先进的解决方案通常是为英语或流行的西方语言构建的。 只有当一种技术获得流行和认可后，模型才会开始为代表性不足的语言构建。 如果你正在使用一种罕见的语言，很可能你需要训练自己的标注器。
- *你想为 Flair 和开源社区做出贡献：* 最后，你可能并不真的需要为特定语言构建特定标注器。 你可能只是想为 Flair 做出贡献。 鉴于目前仍有大量语言未被 Flair 的预训练模型支持，你只需挑选一种代表性不足的语言并亲自动手即可。

无论你训练自定义序列标注器的动机是什么，达到目标的过程都是一样的。 首先，让我们确保我们拥有合适的模型训练工具，通过在下一节中了解硬件要求来实现。

## 了解训练模型的硬件要求

Flair 序列标注模型本质上是一种特殊的神经网络类型。 你可能听说过，为了对神经网络进行推理（即使用）或训练，你需要一台配备高性能 **图形处理单元 (GPU)** 的机器。 训练神经网络需要计算大量数学运算（主要是矩阵乘法）。 与 CPU 相比，这些运算在 GPU 上可以更好地并行化，从而显著加快训练过程。 但这并不意味着没有 GPU 就无法进行任何训练或推理。 你是否真正需要 GPU，完全取决于你正在训练的神经网络的大小以及你愿意等待训练完成的时间（小时、天甚至数十年）。 如果你只是刚开始接触神经网络，并且正在尝试训练只有少数层和几个节点的微小网络，那么即使使用一台没有专用 GPU 的稍旧 PC 也应该完全没问题。

不幸的是，Flair 中的大多数模型在合理的时间内用 CPU 训练起来会太庞大。 在 CPU 上进行推理是完全可行的，单个样本可以在几秒钟内完成标注，但以生产出良好模型为目标在 CPU 上进行训练可能需要数周甚至数月。 事实上，即使是普通的 GPU 训练一个序列标注模型也可能需要从一天到数天不等。

现在很清楚，要训练出效果良好的 Flair 模型需要一台配备 GPU 的机器。 但购买 GPU 并安装到 PC 上并不是你唯一的选项。 你可以从以下任一选项中选择：

- *免费的 Jupyter notebook 环境，例如 Google Colaboratory：* **Google Colaboratory**（也称为 **Colab**）是一个免费的 Jupyter notebook 环境，你可以在其中运行配备 GPU 的 Python 会话。 该环境还提供相当数量的 RAM 和良好的 CPU 性能。 但它也有其局限性。 你只能运行最长 12 小时的会话。 这意味着如果你训练模型超过 12 小时且在此之前未能保存它，你的工作将会丢失。 幸运的是，Flair 提供了一种名为 **检查点** 的特殊解决方案，可使模型训练器在每个 epoch 后存储模型快照。 即使会话在 12 小时后过期，训练也可以从上一个成功的 epoch 恢复。 我们建议将 Colab 会话连接到 Google Drive，以便你的模型自动保存到云端。
- *免费的云计算积分：* 当你首次注册云服务提供商（如 Microsoft Azure）时，你将获得一定数量的积分。 Azure 允许你使用这些积分启动配备 GPU 的 **虚拟机 (VMs)**，在这些虚拟机上你可以轻松运行 Flair。 缺点是最终你会用完免费积分，这将使你转向下一个选项。
- *付费云服务：* 有大量付费云服务提供预设的 Jupyter notebook 环境或非常基础的配备 GPU 的 VM。 付费云服务的优势在于你的会话不受时间限制。 另一个很棒的优点是，如果你发现正在使用的环境资源不足，你可以简单地将其替换为更强大的机器。
- *构建自己的深度学习机器。* 构建自己的设备最大的好处是，一旦你支付了硬件的初始成本，你就可以尽情训练任意多的神经网络，时间长短由你决定。 这种方法的缺点是你的硬件最终会过时，你很快会发现自己的机器不再足够强大。 那时唯一的解决方案就是通过购买新硬件来升级。
- *在 CPU 上训练模型用于学习和实验。* 这个最后的选项适用于任何只想体验模型训练的人。 由于在 CPU 上进行模型训练很慢，你需要在更小的语料上进行更少的 epoch 训练。 一种允许这样做的方法是降采样。 当你对数据集进行降采样（例如通过调用 `dataset.downsample(0.001)`），你将只得到原始数据集的一小部分，这可能小到足以在 CPU 上运行。 这意味着你生产的模型不太可能达到良好的效果。 但你仍然可以一窥模型训练的流程，并在获得更强大的配置后具备进行实际操作的知识和经验。

虽然前述某些选项可以让你训练最先进的模型，而某些仅足以用于学习和实验，但所有选项都足以跟随即将到来的动手部分，了解如何在 Flair 中训练序列标注模型。 但在我们这样做之前，我们需要对基本数学、指标、参数和深度学习术语有一定的了解。

## 使用训练和评估的参数

Flair 可以说是目前最简单的 NLP 框架之一。 但要生产出性能良好的标注器，我们仍需要对底层概念和传递给 Flair 的参数有一定程度的理解。

首先，让我们快速解释一下模型训练到底是什么。

## 了解神经模型训练

神经网络是机器学习的一个特殊子集。 它们松散地基于生物神经网络，如人类大脑。 神经网络通常包括网络架构及其权重和偏置。

网络架构定义了网络的设计。 它包括从层数、输入和输出节点的数量到所使用单元类型等一切内容。 这些属性被称为超参数。 在单次模型训练会话中它们保持不变。

然后是权重和偏置——它们是模型训练的最终结果。 模型训练的过程本质上是计算最优权重的过程。 训练首先通过随机值初始化所有权重和偏置开始。 然后我们多次重复以下步骤：

- 1. 模型被输入一批训练数据样本，我们可以为其计算输出。 对于序列标注模型而言，输出是标记标签。
- 2. 然后将输出（预测标签）与目标值（实际标签）进行比较，并使用损失函数（也称为 `objective` 函数）进行评估。 它告诉我们预测与真实情况有多接近。

模型训练器的目标是找到能产生尽可能小损失的权重。 损失为零将是理想的损失。 这意味着所有预测都与训练数据中的目标值一致。
- 3. 一旦计算出单个训练批次的损失，网络就会使用一种称为反向传播的技术调整权重，以期改进模型。
- 4. 然后我们将下一批训练数据输入模型并从 *步骤 1* 开始重复该过程，直到用完所有训练数据。

前述所有训练数据被用于训练的步骤序列称为 **轮次**。 但由于对模型权重所做的更改非常小，一次数据遍历通常是不够的。 我们通常会为此重复训练过程许多个 epoch，直到损失函数收敛并停止改善。 最佳 epoch 数在很大程度上取决于数据类型和数量、标签数量以及使用的超参数。 在每个 epoch 结束时，我们还使用准确率或 F1 分数等性能指标在测试数据集上评估模型。

训练完成后，存储在内存中的权重通常会被写入文件，以便在我们想要对模型进行推理或继续训练时加载。

本节描述的过程可能看起来复杂且令人不知所措。 但幸运的是，所有这些都已作为框架的一部分实现并自动化。 因此，对于希望构建序列标注器的人来说，没有完全理解神经网络训练背后的底层逻辑并不是致命问题。 重要的是理解传递给 Flair 中模型训练类和方法中的参数和值的目的和含义。

让我们逐一介绍这些参数。



## Flair 模型训练中使用的参数和术语

以下是你在 Flair 中训练模型时将会遇到的一些常见超参数、参数和通用机器学习概念：

- **Learning rate:** 它可能是模型训练中最重要的参数。正确设置该值至关重要。如果学习率设置过低，模型将需要大量的 epoch，且微小的增量会导致学习进展缓慢。另一方面，较低的学习率可能导致我们的模型陷入局部最小值（一个看似最佳的解，但通过探索更多选项可能得到改进）。如果学习率过高，它会使优化器过于激进地调整权重，导致权重修正出现超调。它会使损失在不同 epoch 间大幅波动，无法收敛到最小值。因此，选择合适的学习率至关重要。
- **Max epochs:** 这是一个设置，用于确定训练器在完成前对训练数据进行多少次遍历。该设置线性影响训练时间。当设置过高时，训练可能需要比我们准备等待的时间更长。当设置过低时，训练器会在找到最优权重之前就结束。
- **Loss:** 这是衡量我们的模型与真实标签相比表现如何的指标。该值通常在多个样本上取平均。模型训练的目标是最小化损失。如果损失值为零，则意味着我们的模型预测了所有标签都正确。损失通常绘制在图表上以说明学习进度和趋势。看到损失值下降通常表示模型正在学习，但也可能意味着它只是过拟合了训练数据。
- **Mini batch size:** 在训练模型时，我们不太可能一次性将整个训练集喂给模型。相反，由于内存限制，我们可能会选择将训练集拆分为更小的批次。然后我们可以将每个批次的数据单独喂给模型。决定单个批次中学习样本数量的大小称为 **mini-batch size**。
- **Patience:** 这是在停止之前要训练的坏 epoch 数（目标函数无改善的 epoch）。
- **Hidden size:** 在深度学习中，这指的是隐藏层的数量或隐藏 **Recurrent Neural Network (RNN)** 状态的数量。该设置作为参数提供给 tagger 类以及一些嵌入类，例如 **FlairEmbeddings**。
- **Checkpoints:** 这些是通常在每个 epoch 结束时完成的模型临时快照。如果我们认为训练会话可能会被中断，检查点会很有用，这样可以避免我们不得不从头开始。我们只需在新的训练会话开始时加载检查点，即可从会话中断前的断点继续。

前述列表中解释的术语涵盖了包括 Flair 在内的许多机器学习框架中出现的概念。对它们在模型训练中的作用有一个大致的了解意味着你已准备好进行实际操作。你现在已准备好在 Flair 中训练序列标注模型。让我们直接开始吧。

## 训练自定义序列标注模型

在本节中，我们将研究在 Flair 中训练自定义序列标注模型所涉及的过程、语法、对象和方法。如果你阅读了本章的前几节并理解了内容，你很幸运。一旦你理解了神经网络的底层概念，拥有一个配备 GPU 的设备，并且熟悉最常见的参数，实际的训练过程其实相当直接。

该过程可以分解为以下步骤：

- 1. Loading a tagged corpus
- 2. Loading the tag dictionary
- 3. Building the embedding stack
- 4. Initializing the `SequenceTagger` object
- 5. Training the model

这些步骤每一步只需要几行代码。为了更好地理解代码，让我们将其作为训练词性标注器的实际示例的一部分来讲解。例如，让我们假装 Flair 中没有预训练的英语标注器，并尝试从头开始训练英语词性标注器。

我们将遵循 Flair GitHub 仓库文档中提供的 Flair 推荐训练步骤的改编版本。

### 加载标注语料库

尝试在 Flair 中加载标注语料库时，我们需要执行以下操作：

- 1. 我们首先需要验证这样的语料库是否存在以及它具体叫什么名字。这些信息可以在 Flair 的 GitHub 页面上的官方文档中找到，网址为 https://github.com/flairNLP/flair/blob/master/resources/docs/TUTORIAL_6_CORPUS.md。

例如，英语的默认语料库称为 UD_ENGLISH。它可以用一个函数调用加载：

```
from flair.data import Corpus
from flair.datasets import UD_ENGLISH

corpus = UD_ENGLISH()
```

如果首次执行，这将从互联网存储中下载模型，可能需要几分钟。

模型随后将被存储以供将来使用，这意味着下次你运行同一命令时，它将从磁盘加载模型并在几秒内执行。

- 2. 由于我们的语料库包含各种不同的标签和标注，我们需要决定标注器的目标，以便过滤掉所有无关的标签。我们将通过将使用的标注器类型存储到一个变量中来实现这一点。该变量稍后将被传递给不同的方法：

```
tag_type = 'pos'
```

tag_type 变量设置为 pos 表示我们正在处理词性标注。

- 3. 最后，我们需要决定是追求最先进的模型，还是仅仅想尝试 Flair 以获得训练模型的经验，而不期望得到好的结果。如果你属于后者类别，只需对数据集进行下采样即可大幅减少训练时间。例如，我们可以将数据集减少到其原始大小的 1%，并通过运行以下命令将训练时间从数小时减少到数分钟：

```
import random

random.seed(123)  # ensure we get same split every time
corpus.downsample(0.01)
```

这将使我们即使在 CPU 上也能训练标注器。请注意，在前面的脚本中，除了对语料库进行下采样外，我们还导入了 random 库并设置了随机生成器种子。我们对数据集进行下采样，以便更快地完成此练习，而不必等待模型训练很长时间。当在少量数据上训练神经模型时，我们并不总是能得到预期结果，训练过程可能不稳定。这意味着如果你选择数据集的略有不同的子集，结果可能完全不同。为了解决这个问题，我们使用了 random.seed() 方法，这确保了 Flair 的随机子集选择方法每次都表现一致。

我们的语料库现在已准备就绪。让我们继续下一个数据加载和预处理步骤。

### 加载标签字典

标签词典只是一个所有可能标签的集合。对于词性，这包括动词、形容词、名词和感叹词等类别。它还包括一些特殊标签，例如 unk 标签，表示我们无法为其计算经典词嵌入的未知词。

可以通过运行以下代码块获取标签词典：

```
tag_dictionary = corpus.make_label_dictionary(tag_type)
print(tag_dictionary)
```

前面的代码段将打印出以下结果：

```
Dictionary with 50 tags: <unk>, O, DT, NN, VBZ, RB, NNP, CD, ., WP, PRP, JJ, '', '', IN, CC, NNS, ,, VBP, MD, VB, TO, POS, WRB, VBD, VBG, NNPS, VBN, HYPH, -LRB-
```

前面的输出显示我们处理的是 50 个不同的词性标签，其中一些（例如 unk 标签）具有特殊含义，并不代表词性。

我们现在可以进入下一节，在那里你将从第 3 章《Flair 中的嵌入》中获得的知识会大有裨益。

### 构建嵌入堆栈

构建嵌入堆栈的能力是 Flair 的一大特色，通常有助于其取得最先进的结果。堆栈可以通过简单地将 Flair 支持的嵌入对象列表传递给 StackedEmbeddings 类构造函数来构建。



在本文示例中，我们将构建一个非上下文英语 FastText 嵌入（使用语言代码如 en 时的默认嵌入）、上下文英语前向 Flair 嵌入和上下文英语后向 Flair 嵌入的堆叠。可以通过运行以下代码块来实现：

```
from flair.embeddings import (WordEmbeddings,
                              StackedEmbeddings,
                              FlairEmbeddings)

embedding_types = [
    WordEmbeddings('en'),
    FlairEmbeddings('news-forward'),
    FlairEmbeddings('news-backward'),
]

embeddings = StackedEmbeddings(embeddings=embedding_types)
```

Flair 在加载语料库时一样，会下载托管在网络上的嵌入对象并将其存储以供将来使用。此步骤可能需要几分钟时间。

我们现在可以进入实际训练前的最后两个准备步骤。

## 初始化 SequenceTagger 对象

我们将使用之前在第 4 章《序列标注》中用于加载预训练模型的同一个 SequenceTagger 类，但这次用于定义全新的模型。如果你还记得的话，我们曾使用 SequenceTagger.load() 的静态调用来从预训练模型创建 tagger 对象。在这里，我们将通过传入定义模型的属性直接初始化该类。这些属性定义了从模型架构、循环单元类型、隐藏 RNN 状态的数量到模型预测的可能标签集合的一切。

SequenceTagger 类的必需参数如下：

- hidden_size: 这定义了 RNN 中隐藏状态的数量。常用的起始值是 256，但最佳值完全取决于你要解决的问题。
- embeddings: 嵌入类需要是 TokenEmbeddings 类的一个实例。它可以是 CharacterEmbeddings、FlairEmbeddings、WordEmbeddings，或我们在示例中使用的类 – StackedEmbeddings。
- `tag_dictionary`: 包含我们在上一节中使用 `corpus.make_label_dictionary()` 方法生成的所有可能标签的字典。
- `tag_type`: 表示我们正在训练的标注器类型的字符串。对于 PoS 标注，使用 `pos`，对于 NER，请确保使用 `ner`。

要为本次练习中训练的新模型初始化一个 SequenceTagger 对象，请运行以下代码块：

```python
from flair.models import SequenceTagger

tagger = SequenceTagger(hidden_size=256,
                        embeddings=embeddings,
                        tag_dictionary=tag_dictionary,
                        tag_type=tag_type)
```

这里，`embeddings`、`tag_dictionary` 和 `tag_type` 是在前面的项目符号列表中定义的变量。

就这样！前面的代码执行时间不应超过一秒。我们现在创建了一个英语 PoS 序列标注器，完全能够对英语文本进行 PoS 标注。

如果前一段话让你皱起了眉头，你并不孤单。序列标注模型在训练时应该消耗不少资源和时间——没有序列标注模型应该在不到一秒钟内产生，对吧？嗯……并非完全如此。一个好的序列标注模型需要大量的资源和时间来生成。但一个糟糕的模型却不必如此！我们用这段代码生成的是一个完整且完全可用的序列标注模型，其权重是随机初始化的。这意味着模型完全能够对文本进行标注；只是结果的准确性与随机猜测一样好。

在下面的示例中，我们将使用权重随机初始化的 tagger 来标注一些文本：

```python
from flair.data import Sentence

sentence = Sentence('Hello world')
tagger.predict(sentence)
print(sentence)
```

此代码块将输出类似以下内容：

```
Sentence: "Hello world" → ["Hello"/RB, "world"/VB]
```

这里，单词 Hello 和 world 被标注为 RB 和 VB，暗示第一个词是副词，第二个词是动词。这显然是错误的。如果你自己运行这两个代码片段，你很可能会得到不同的结果。这是因为我们新生成的模型的权重是随机初始化的。除非你极其幸运，否则你的结果也将是错误的。

为了让我们的模型产生良好的结果，我们需要调整模型权重，使其尽可能准确地产生结果。这是通过模型训练完成的。让我们来弄清楚。

## 训练模型

我们现在已经将标注好的语料库加载到内存中，嵌入已定义，并且 SequenceTagger 对象已创建。我们现在准备开始训练。

训练过程首先涉及创建 ModelTrainer 对象，然后调用其 train() 方法。

ModelTrainer 需要以下必需参数：

- `model`: 一个 Flair 模型类 – 例如一个 `SequenceTagger` 对象
- `corpus`: 一个 `Corpus` 对象

了解了所有这些信息后，我们现在可以通过执行以下代码行来初始化 ModelTrainer 对象：

```
from flair.trainers import ModelTrainer

trainer = ModelTrainer(tagger, corpus)
```

这就剩下序列标注模型训练的最后一步——调用 train() 方法。它接收的参数数量非常多，但以下是一些你可能会觉得有用的参数：

- `base_path`: 模型将被存储的路径。如果目录或其子目录不存在，它们将被创建。
- `learning_rate`: 初始学习率。默认为 0.1。
- `mini_batch_size`: 训练期间的迷你批次大小。默认为 32。
- `max_epochs`: 训练结束前的轮次数。`150` 是一个不错的起始值。
- `patience`: 在终止之前没有改进的轮次数。默认为 3。
- `train_with_dev`: 是否同时使用 dev 和 train 数据集进行训练。dev 数据集（有时也称为验证数据集）用于调整超参数。如果不进行超参数调优，可以改用该数据集进行训练。这允许你在更多数据上训练模型。
- `checkpoint`: 如果为 True，Flair 将在每个 epoch 结束时在 `base_dir` 中存储名为 `checkpoint.pt` 的模型快照，以便在训练中断时可以恢复。
- `write_weights`: 如果为 True，Flair 将把权重写入名为 `weights.txt` 的文件。
- `optimizer`: 一个 **PyTorch** 优化器类。默认使用随机梯度下降。
- `epoch`: 如果继续中断的训练运行，则使用的起始轮次。
- `use_tensorboard`: 如果为 `True`，则输出 **TensorBoard** 信息。
- `tensorboard_log_dir`: TensorBoard 数据可能存储的目录。
- `metrics_for_tensorboard`: TensorBoard 的自定义可选指标。

## 那 TensorBoard 是什么？

TensorBoard 是一个用于检查和理解神经网络训练运行的网页工具。它绘制所有训练数据，如损失、准确率以及许多其他指标，并允许我们轻松监控训练过程。该工具最初是为与 **TensorFlow** 深度学习框架集成而构建的，但此后已集成到许多其他工具中。其中一个集成 TensorBoard 的工具是 **PyTorch**——为 Flair 提供动力的底层深度学习框架。这就是为什么将 TensorBoard 与 Flair 挂钩很容易，只需通过将 `Tensorboard` 参数设置为 `True` 并将正在运行的 **Tensorboard** 实例的日志目录传递给 `ModelTrainer` 构造函数即可启用它。

我们现在终于可以开始实际训练我们的模型了：

```
trainer.train('tagger',
    learning_rate=0.1,
    mini_batch_size=32,
    max_epochs=150,
    train_with_dev=True)
```

这将启动训练过程并最多运行 150 个 epoch。它会将模型结果存储在 tagger 目录中，该目录将在你运行训练脚本的目录内部创建。

如果一切正确，训练器应恢复第一个 epoch。当前两个 epoch 完成后，输出应类似于以下内容：

```
-------------------------------------------------------------
epoch 1 - iter 1/4 - loss 65.13372040 - lr: 0.100000
epoch 1 - iter 2/4 - loss 69.70247269 - lr: 0.100000
epoch 1 - iter 3/4 - loss 71.33122508 - lr: 0.100000
epoch 1 - iter 4/4 - loss 69.54229164 - lr: 0.100000
-------------------------------------------------------------
EPOCH 1 done: loss 69.5423 - lr 0.100000
DEV : loss 47.70802688598633 - score 0.0654
BAD EPOCHS (no improvement): 0
-------------------------------------------------------------
epoch 2 - iter 1/4 - loss 53.46533203 - lr: 0.100000
epoch 2 - iter 2/4 - loss 59.09352493 - lr: 0.100000
epoch 2 - iter 3/4 - loss 58.95171865 - lr: 0.100000
epoch 2 - iter 4/4 - loss 59.12093544 - lr: 0.100000
-------------------------------------------------------------
EPOCH 2 done: loss 59.1209 - lr 0.100000
DEV : loss 41.87521743774414 - score 0.1115
BAD EPOCHS (no improvement): 0
```



The first and probably most important thing to monitor during training is the trend of how a loss value is changing. A decrease in loss usually means the model is learning. From the previous output, we can see that the first epoch finished with a loss of 69.5423, and the second epoch finished with a loss of 59.1209. This indicates that the model is learning! The output also indicates that our accuracy score increased from 0.0654 to 0.1115.

If you remember, earlier in this section, we mentioned that the sequence tagger objects are initialized with random weights. This means that when you run the same exact code twice, the values will be slightly different.

Training, unless interrupted, will now continue to pass through a number of epochs until it does max_epochs, or until the trainer fails to improve the model for patience consecutive epochs.

On an average GPU, training a downsampled English dataset should finish within a few minutes. When it does, Flair will report the final results of the best model tested on the test dataset. The results should look something like this:

```
Results:
- F-score (micro): 0.8878
- F-score (macro): 0.702
- Accuracy (incl. no class): 0.8878
```

After these results, we should also see results reported for each tag individually.

Training is now complete. We can now try to tag the same pair of words (hello world) sentence on our newly trained model to see whether the results are now any better:

```
tagger.predict(sentence)
print(sentence)
```

This code will, if you followed the steps correctly, print out the following:

```
Sentence: "Hello world" -> ["Hello"/UH, "world"/NN]
```

The UH tag in the output stands for interjection and NN for noun. This means that our trained tagger labeled our example sentence correctly!

> **Important Note**
> 在不太可能的情况下，如果你运行了这段代码并且结果与预期不同，这只是因为你在太小的数据集上运行了标注器。要获得与前面输出相同的结果，请确保在下采样程度较低的数据集上重新开始训练过程。

In the preceding code snippet, we tagged text by referring to the `tagger` variable, which holds the model stored in memory. But on top of storing our trained model in memory, Flair also stored it in the filesystem so that it can be loaded and used at a later point.

## 理解训练输出文件

After a successful training session, Flair will have written a number of training output files to a directory defined by the `base_dir` parameter. They, aside from the actual model, hold important information about training.

Here is a list of files to expect in `base_dir` after a training run:

- `final-model.pt`: The actual model. This can also be called `best-model.pt` when using dev data for hyperparameter tuning.
- `loss.tsv`: A tab-separated document containing information about the training loss for each epoch.
- `training.log`: A complete log of standard output written out during training.
- `test.tsv`: A tab-separated document containing tokens, their real tags, and their predicted tags. This document allows you to do token-level analysis, verifying which tokens were predicted correctly and which weren't.
- `weights.txt`: Model weights. This file will only be populated only if `write_weights` was set to `True` during training.

With our trained model now stored in the filesystem, we can now test loading the model from a file in a fresh Python session.

## 加载和使用自定义 Flair 模型

In the final stages of model training, Flair will write a file called `final-model.pt` to a path defined by the `base_dir` argument of our `ModelTrainer`. Loading this model into Flair is as easy as passing it as the first argument to the `SequenceTagger.load` method.

Assuming tagger is our base_dir and our Python session's current working directory is the same as the one we were in when doing training, the model can be loaded by running the following:

```
from flair.models import SequenceTagger

tagger = SequenceTagger.load('tagger/final-model.pt')
```

We can then test the tagger out by running the following:

```
from flair.data import Sentence

sentence = Sentence("Hello world")
tagger.predict(sentence)
print(sentence)
```

This, as expected, produces the same output as the model we tested right after training:

```
Sentence: "Hello world" => ["Hello"/UH, "world"/NN]
```

This shows that the words hello and world were, yet again, correctly tagged with interjection and noun tags respectively.

## Resuming interrupted training sessions

While our model training example that used a severely downsampled dataset seems like a fairly straightforward process, the reality is different. Real training sessions on bigger datasets take a significantly longer amount of time to train, and the chance of unexpected interruptions therefore increases. Interrupts can happen when the machine runs out of resources because some dependency of the machine timed out, or simply because of a technical issue.

When a model training interrupt happens, all data that is stored in memory, including our trained weights, are lost.

Regardless of what really happened, the fact is, an interrupt happened and the memory-stored data is lost. Luckily, Flair has a good way around this called checkpoints. They are essentially a snapshot of the model that is stored after each epoch, regardless of whether the epoch improved the model or not. This checkpoint is stored in the base_dir directory with the filename of checkpoint.pt.

To make use of the checkpoint feature in Flair, we need to pass the checkpoint=True parameter value to the trainer.train() method when training. But the feature does come at a cost. Writing checkpoints essentially means writing model weights to the disk after each epoch, and that takes time. This means that using checkpoints is likely to slow down the model training process a bit. However, when doing serious model training for longer periods of time, this overhead is almost always worth it.

Assuming our base_dir is tagger and we have a corpus object-loaded memory, we can simply resume training from the last successful epoch by running the following:

```
from flair.trainers import ModelTrainer

chkpoint_path = 'tagger/checkpoint.pt'
trainer = ModelTrainer.load_checkpoint(chkpoint_path, corpus)

trainer.train(...)
```

Here, the arguments passed to trainer.train() should be no different from the parameters we used for the initial training session that got interrupted, where we also used the checkpoint=True model trainer parameter value.

The preceding exercise and explanations should help us train a tagger for just about any language included in Flair's corpora set. But it's entirely possible that the parameters we choose will not yield good results. There are many clues in the model training output we can look out for that will tell us whether training is progressing well or whether we should restart with a different approach.

## Knowing when to stop and try again

The question "Is my model good enough?" is a whole subfield of machine learning on its own and can't fully be explained in a single chapter. Nevertheless, there's still some practical advice we all can follow to help us determine whether it's worth restarting training with different parameters, different model types, or even new data.

The two basic techniques for measuring success during and after training are as follows:

- 1. Monitoring loss
- 2. Assessing and comparing performance metrics

## Monitoring loss

The first and most important thing you should look out for during training is whether the model is learning in an expected way. This is done by monitoring training output metrics such as loss. A typical training session follows the following pattern. In the initial stage of training, our weights are randomized, and loss will be huge (remember how bad our model with random weights was?). Then, even after one or two epochs, we will see a significant improvement in loss and model performance. Finally, as we progress towards two- or three-digit epoch numbers, the improvement will get smaller and smaller. Ideally, the loss should start to plateau and converge towards a certain value. That way, assuming we didn't hit a local minimum, we can be confident that, given the parameters we chose, the model trainer did a good job. If we fail to see a convergence toward a certain value before training finishes, this may mean we finished training too soon and may need to run it for a couple more epochs.

These training debug values such as loss will be printed out during training. But these values, shown as raw numbers, are sometimes very hard to make sense of. Just by looking at the numbers, it's hard to see what the general trend is, whether the numbers are converging toward a certain value, or whether they are jumping all over the place. This is much easier to comprehend when metrics such as training loss are visualized.

## Plotting training loss

Flair offers a simple tool that allows us to visually inspect metrics such as training loss and accuracy and store the output as an image.

Assuming we ran a training session from the same directory as our current Python session and used base_dir called tagger, we can plot the training loss by running the following:

```python
from flair.visual.training_curves import Plotter
plotter = Plotter()
plotter.plot_training_curves('tagger/loss.tsv')
```

This will store the plot as tagger/training.png and, if you are running this code snippet inside a Jupyter notebook, display the plot in the notebook itself.



## 评估和比较性能指标

最终决定我们的标注器好坏的指标，是评估模型在训练数据上的性能指标。在词性标注（PoS tagging）的情况下，这很可能就是 F1 分数。但这个数值本身并不能说明太多。例如，在前面的练习中，我们获得了 0.88 的 F1 分数。如果我们使用了具有代表性的数据集，这个数字本身确实能让我们对模型在现实世界中的表现有一个大致的了解，但它在帮助评估我们的标注器是否值得满意，或是否比其他标注器更好方面作用不大。

评估模型性能最客观的方法之一是将其与相关研究或实验中公布的最先进结果进行比较。这样，如果我们获得了更大的 F1 分数，我们就可以声称（在评估所使用的测试数据的前提下）我们的模型确实实现了更好的性能。然而，在不同研究或实验之间比较结果时，我们需要绝对确定所有实验都使用了相同的训练和测试数据。否则，我们本质上就是在拿苹果和橘子比较。例如，如果一项研究使用的是词汇量小、语言非常重复且简单的语料库，而另一项研究则关注某些高度专业化且晦涩的语言用法且词汇丰富，那么可以合理假设在前者类型的数据集上训练的标注器会取得更好的结果。因此，只比较在相同数据上训练的标注器的性能至关重要。

## 摘要

本章中我们涵盖的代码示例、解释和练习是对 Flair 中训练序列标注模型的一个快速介绍，并且应该能让你有信心准备、训练、验证和使用解决现实世界问题的 Flair 模型。

此外，你现在应该能够判断模型训练是否成功，或者是否应该使用不同的参数重新启动。我们了解到，有大量参数控制着序列标注模型的训练以及训练过程。其中包括学习率、epoch 数量、优化器类型、隐藏 RNN 层数，以及很可能影响我们模型性能的许多其他参数。在之前的示例中，我们通常选择 Flair 在大多数代码示例中使用的默认参数，但不能保证默认参数是我们问题的最优参数。事实上，这极不可能。这会让你疑惑到底应该使用哪些参数。

幸运的是，Flair 为此问题提供了解决方案——一种特殊技术，它尝试使用不同的参数组合进行模型训练。然后它在称为开发集或验证集的特殊数据集上衡量性能。这种技术称为超参数调优。让我们在下一章学习如何在 Flair 中实现它。
