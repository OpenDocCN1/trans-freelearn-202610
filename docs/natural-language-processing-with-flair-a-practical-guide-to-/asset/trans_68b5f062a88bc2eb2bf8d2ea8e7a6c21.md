
## Flair 中的分词

根据你的需求和所用语言的类型，你可以从 Flair 中提供的以下任何分词器中选择：

- `SpaceTokenizer()`: 一个简单的分词器，按空格字符分割文本。请注意，此分词器仅按空格字符 (" ") 分割。它不会按包含在 Python 的 `string.whitespace` 中的字符分割，该字符常用于分割或格式化文本的方法中。空白字符（连同空格字符）还包括换行符和制表符，而 `SpaceTokenizer` 不会考虑这些。
- `SpacyTokenizer(model)`: 使用一个名为 **spaCy** 的常用 NLP 库的分词器。它接受一个名为 `model` 的参数，该参数可以是表示 **spaCy** 模型（例如，`en_core_web_sm`）的字符串，也可以是 spaCy `Language` 对象。
- `SciSpacyTokenizer()`: 使用 spaCy 的 **`en_core_sci_sm`** 模型的分词器，并扩展了诸如 `(, )` 和 `-.` 等特殊字符。这些用作额外的词元分隔符。
- `SegtokTokenizer()`: 使用 Flair 依赖项 **segtok** 的分词器，该依赖项专注于分割（印欧语系）文本。
- `JapaneseTokenizer(tokenizer, sudachi_mode)`: 使用 **konoha** 库的分词器类，并为日语提供多种分词器选择。`tokenizer` 参数可以是 `mecab`、`janome` 或 `sudachi`。

现在我们对 Flair 中可用的分词器有了很好的了解，让我们尝试使用非默认分词器使用 Sentence 对象：

```python
from flair.data import Sentence
from flair.tokenization import SpaceTokenizer

tokenizer = SpaceTokenizer()
s = Sentence('Some nice text.', use_tokenizer=tokenizer)

# getting the string representation using magic method __str__
print(s, len(s))
```

上述脚本使用 SpaceTokenizer，应该打印出以下内容：

```
Sentence: "Some nice text." 3
```

这与 SegtokTokenizer 不同，由于仅按空格字符分割，它将 text. 字符串视为单个词元。

在下一节中，我们将了解如何从 Sentence 和 Token 对象中提取信息以及如何为其添加标签。

## Sentence 和 Token 对象辅助方法

Sentence 对象有几个有用的方法和属性。最直接的一个，也是我们在前面的示例中已经使用过的，是 __str__ 魔法方法的实现，它返回对象的字符串表示形式，例如在尝试打印它时。

为了能够完全理解 Sentence 对象的字符串表示格式，我们需要为至少一个词元添加标签。为此，我们将使用 get_token(n) 方法获取第 n 个词元的 Token 对象，然后使用 add_label(label_type, value) 方法为词元分配标签：

```python
from flair.data import Sentence

sentence = Sentence('A short sentence')
sentence.get_token(1).add_label('manual-pos', 'DT')
print(sentence)
```

上述脚本将打印出以下内容：

```
Sentence: "A short sentence" → ["A"/DT]
```

打印字符串的第一部分显示整个句子在其原始形式，而句子的第二部分显示带有相应标签的词元。在此代码示例中，我们手动标记了第一个词元以测试对象的字符串表示，而在 Flair 中，词元通常会由序列标注器进行标记。

类似的 Sentence 对象字符串表示也可以通过调用 to_tagged_string() 方法直接获得。

> **重要提示**
> get_token(n) 方法使用基于 1 的索引，这意味着 n=1 将检索第一个词元，而 n=0 将返回 None。使用 get_token(n) 的基于 0 的索引替代方案是使用索引运算符。例如，要获取名为 sentence 的 Sentence 对象的第一个词元，我们可以简单地调用 sentence[0]。

标记化的 Sentence 对象中的词元也可以通过使用其 __iter__() 魔法函数获得，这使我们可以通过简单地迭代对象来获取所有词元。我们例如可以通过使用 for 循环来做到这一点。

让我们使用前一个脚本中的相同 sentence 变量运行以下脚本：

```python
for token in sentence:
    print(token)
```

脚本将打印出以下内容：

```
Token[0]: "A" → DT (1.0)
Token[1]: "short"
Token[2]: "sentence"
```

这使我们能够清楚地了解原始字符串是如何被分词的。

在使用 Sentence 对象时，了解使用 len() 方法的行为很重要。它将返回词元数量，而不是原始字符串的长度。这意味着使用前一个脚本中的变量调用 len(sentence) 将返回 3。如果您希望获取实际句子（即原始文本）的长度，则需要运行 len(sentence.to_original_text())。

如果这里描述的任何分词器都不适合你，Flair 也能满足你的需求。你需要做的只是实现自己的自定义分词器并将其传递给 Flair。让我们学习如何操作。

## 使用自定义分词器

虽然 Flair 随附了支持最常用语言的多个分词器，但你可能正在处理一种使用目前未被 Flair 覆盖的分词规则的语言。幸运的是，Flair 提供了一个简单的接口，允许我们实现自己的分词器或使用第三方库。

### 使用 TokenizerWrapper 类

TokenizerWrapper 类为构建自定义分词器提供了一个简单的接口。要构建一个，你只需通过传递 tokenizer_func 参数来实例化该类。该参数是一个函数，它接收整个句子文本作为输入并返回一个词元字符串列表。

作为练习，让我们尝试实现一个将文本拆分为字符的自定义分词器。此分词器将把每个字符视为一个词元：

```python
from flair.data import Token
from flair.tokenization import TokenizerWrapper

def char_splitter(sentence):
    return list(sentence)

char_tokenizer = TokenizerWrapper(char_splitter)
```

在前面的代码中，我们实现了 space_splitter 函数，它只是将句子拆分为字符列表。最后，我们通过使用新的函数实例化 TokenizerWrapper 类来创建新的分词器对象。

现在，我们可以通过使用 Sentence 对象并打印生成的词元来测试我们的分词器：

```python
from flair.data import Sentence

text = "Good day."
sentence = Sentence(text, use_tokenizer=char_tokenizer)

for token in sentence:
    print(token)
```

结果脚本将打印出以下内容：

```text
Token[0]: "G"
Token[1]: "o"
Token[2]: "o"
Token[3]: "d"
Token[4]: " "
Token[5]: "d"
Token[6]: "a"
Token[7]: "y"
Token[8]: "."
```

打印的文本表明我们的分词器按预期执行并且与 Sentence 对象兼容。

> **关于分词器的重要提示**
> 语言的分词规则通常很复杂并且包含许多边缘情况。因此，强烈建议使用来自成熟 NLP 包的第三方分词器，而不是自己实现。

现在我们已经介绍了分词以及 Sentence 和 Token 基础类型，是时候继续深入了解 Corpus 对象了。

## 理解 Corpus 对象

Corpus 对象是 Flair 中在内存中存储语料库的主要对象。每个 Corpus 都是三个数据集的集合，这些数据集的行为类似于 Sentence 对象列表。可以通过以下属性访问这些数据集：

- **train** 属性，其中包含将用于训练模型的数据集。
- **test** 属性，其中包含独立于 train 数据集的数据集。它用于模型验证。
- **dev** 属性，其中包含用于超参数调优的数据集。

这三个数据集理想情况下包含来自相同数据源的数据并遵循相同的概率分布。

可以通过加载 Flair 的其中一个准备好的数据集来获得示例语料库对象：

```python
from flair import datasets

corpus = datasets.UD_ENGLISH()
```

可以通过简单地打印对象来获得语料库摘要：

```python
print(corpus)
```
