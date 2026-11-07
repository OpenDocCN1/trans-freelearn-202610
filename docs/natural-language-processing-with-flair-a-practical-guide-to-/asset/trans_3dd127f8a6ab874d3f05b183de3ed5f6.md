
前面的输出提供了有价值的信息。它告诉我们当前正在处理的 epoch（本例中为 epoch 97），训练损失是多少（本例中为 0.77），验证损失是多少（本例中为 0.99），并且打印出了由我们的语言模型生成的一些文本：ni li sike. mi pini e ma mije? kala ni li sin e pan lawa en kena t.

在最初几个 epoch 中，你会注意到我们的语言模型产生的是一堆胡言乱语。大多数词都是词汇表外的词，输出完全随机。但随着训练的进行，你会注意到损失在下降，生成的文本开始看起来像 Toki Pona.

在普通机器上，用 CPU 训练这 100 个 epoch 不应超过 10 分钟。训练完成后，Flair 会打印出最终的验证损失和困惑度：

```
TEST: valid loss 0.92 | valid ppl 2.52
```

就这样！

我们的 Flair 嵌入模型现在已经训练完成。我们可以开始生成一些嵌入。但在这样做之前，我们可以先稍微玩一下语言模型，并使用 generate_text() 方法生成一些文本：

```
t = language_model.generate_text(number_of_characters=40)[0]
print(t)
```

这将打印出语言模型生成的 40 个字符（由于权重随机初始化，每次训练的结果都会不同）：

```
sike ni li pona mute. soweli ni li pana
```

虽然这些 Toki Pona 语句对人类来说意义不大，但至少我们可以欣慰的是，生成的词都是真正的 Toki Pona 词，词的结构似乎遵循某种逻辑模式。还请注意，我们只生成了 40 个字符，这意味着最后一个词可能会被截断。如果你自己训练模型，你的随机初始化模型权重会不同，模型产生的文本几乎肯定会与所示示例不同。

## 使用自定义嵌入进行下游任务

训练 Flair 嵌入模型的主要目的并不是玩弄生成的文本和打印嵌入向量。很可能，我们训练嵌入是为了在下游 NLP 任务中使用它们。

幸运的是，加载自定义训练的 Flair 嵌入的语法与加载任何其他预训练 Flair 嵌入模型相同。我们只需将模型路径传递给构造函数即可。

如果传递给 LanguageModelTrainer 类的输出路径是 forward_model_directory/，我们将在 forward_model_directory/best-lm.pt 找到训练好的模型。

然后我们可以通过运行以下代码实例化一个 FlairEmbeddings 对象：

```
from flair.embeddings import FlairEmbeddings
fw = FlairEmbeddings('forward_model_directory/best-lm.pt')
```

前面的代码片段将我们的自定义嵌入模型加载到 fw 变量中。

请注意，在本练习中，我们只训练了前向嵌入模型，它考虑的是词之前的上下文。为了获得更好的性能，你现在可能希望使用 forward 和 is_forward_lm 参数设为 False 重复整个嵌入训练过程，这将改为训练反向模型。然后你可以将嵌入模型堆叠在一起形成一个 StackedEmbeddings 对象，这是第 5 章 Training Sequence Labeling Models 中深入介绍的过程的一部分。

你现在可以在任何使用词嵌入的 Flair 下游 NLP 任务中使用此模型。这样做还可以让你通过评估使用这些自定义嵌入的标注器的性能来对嵌入模型进行外部评估。

但训练标注器需要很长时间。因此，在这样做之前，你可能只想做一个合理性检查，以验证我们的嵌入是否至少包含一些有用的信息。

## 对自定义 Flair 嵌入进行内在评估

在我们进行冗长的序列标注模型训练过程以验证我们的嵌入是否有意义之前，我们可以改做一个非常简单的内在评估作为合理性检查。

我们可以做的一个简单的内在检查是为几个同义词计算嵌入，然后使用相似度指标判断 resulting 嵌入是否相似。同样，我们可以为语义上不相似的词计算嵌入并检查它们的嵌入是否也不相似。为了简化操作，我们将对不相似的词使用一个词汇表外（OOV）词。因此，我们的测试将验证两个相似词的嵌入是否比一个词典词和一个 OOV 词的嵌入更接近（以余弦距离衡量）。

对于相似词，我们将选择 lukin 和 oko。虽然不是真正的同义词，但在 Toki Pona 中它们的含义都与视觉和眼睛有关。我们将为这两个词和另一个 OOV 词生成嵌入，然后测量这些嵌入之间的相似度。如果我们的嵌入质量不错，lukin 和 oko 在相似度指标上应该得分很高，而 OOV 词则不期望与前两者相似。

让我们使用我们的自定义嵌入模型为这三个词全部计算嵌入：

```
from flair.embeddings import FlairEmbeddings
from flair.data import Sentence

synonym_1 = Sentence('lukin')
synonym_2 = Sentence('oko')
rand_word = Sentence('oovword')

fw = FlairEmbeddings('forward_model_directory/best-lm.pt')
fw.embed(synonym_1)
fw.embed(synonym_2)
fw.embed(rand_word)

embedding_syn_1 = synonym_1[0].embedding.tolist()
embedding_syn_2 = synonym_2[0].embedding.tolist()
embedding_rnd_wrd = rand_word[0].embedding.tolist()
```

在前述代码片段的第一部分，我们只是为词 lukin、oko 和 oovword（随机 OOV 词）初始化 Sentence 类。

然后我们可以使用余弦相似度指标来衡量这三个嵌入之间的相似度：

```
from sklearn.metrics.pairwise import cosine_similarity as sim

s_synonym = sim([embedding_syn_1], [embedding_syn_2])[0][0]
s_rand_1 = sim([embedding_syn_1], [embedding_rnd_wrd])[0][0]
s_rand_2 = sim([embedding_syn_2], [embedding_rnd_wrd])[0][0]
```

前面的片段计算了三个词对之间的相似度，并将 lukin 和 oko（同义词）之间的相似度存储到 s_synonym 中。它还计算了 lukin 与 oovword 之间，以及 oko 与 oovword 之间的相似度。

让我们看看我们的假设是否正确：

```
print(s_synonym , s_rand_1 , s_rand_2)
```

如果你的嵌入训练得不错，前面的代码片段将打印出类似以下内容：

```
0.6111595281781783 0.2691232575537895 0.18826418665007352
```

太好了！

事实证明我们的假设得到了支持。相似词在相似度得分上得分很高（打印出的第一个数字），而不相关的词（打印出的第二个和第三个数字）得分较低。请注意，我们从这些结果中唯一可以得出的结论是我们的嵌入中存储了一些程度的 Toki Pona 信息，但我们不应就这些嵌入在下游 NLP 任务中的有用性做出任何结论。要正确训练 Flair 嵌入以供实际使用，我们需要在更大的数据集上训练，使用更高的嵌入维度，训练更多 epoch，并使用更长的序列长度。

## 总结

训练自定义嵌入是 Flair 中较为复杂的功能之一。它需要所有有助于我们选择正确参数并正确准备数据的知识和理解，以便利用训练较大语言嵌入所需的大量计算能力。它也是一个关键概念，必须正确理解和执行，因为 Flair 的几乎所有其他功能都以某种方式依赖于嵌入。

在本章中，我们介绍了训练自定义 Flair 词嵌入的动机，并概述了嵌入设计。我们通过为世界上最小的语言 – Toki Pona 训练前向词嵌入，介绍了训练这些嵌入所需的语法。

到目前为止，我们将嵌入呈现为可作为下游 NLP 序列标注任务输入的东西。但嵌入也可以用于其他 NLP 应用，例如文本分类。让我们在下一章学习关于那方面的内容。
