
我们现在可以根据词 A、B 和 C 的词嵌入来计算词 D 的词嵌入。一旦我们得到词 D 的词嵌入，找到其对应的词就很简单了。我们只需搜索整个词汇表的词嵌入并找到匹配最接近的词。

现在是时候用 Flair 来测试我们的类比求解器了。

## 实现

我们将实现一个算法，给定词 A、B 和 C，计算词 D，使得 A 比 B 如同 C 比 D 的类比成立。例如，给定输入 King is to man as queen is to...，算法将返回 woman。该算法包含以下步骤：

- 1. 选择并实例化我们在 Flair 中使用的嵌入类
- 2. 获取词 A、B 和 C 的词嵌入
- 3. 使用 D = B + C − A 公式计算词 D 的近似词嵌入
- 4. 获取 Flair 中所有英语词的词嵌入
- 5. 找到最匹配的词

### 选择并初始化嵌入类

选择嵌入类型时，重要的是我们选择一种非上下文相关的技术，因为我们将独立于可能出现的上下文来计算新词。

因此我们将使用 crawl ID 选择一组经典的随机词嵌入。crawl 嵌入是在网络爬虫数据上训练的英语词嵌入。嵌入对象可以通过运行以下代码简单初始化：

```
from flair.embeddings import WordEmbeddings
fasttext = WordEmbeddings('crawl')
```

首次执行此代码时，Flair 将下载嵌入模型，这会占用一些空间，并且完全下载可能需要几分钟。

为了避免我们再次下载模型，Flair 在下次使用时会确保使用本地存储的版本。

一旦嵌入对象加载完成，我们可以将其保留在内存中并重复使用并传递给后续的辅助函数以节省执行时间。

### 获取词 A、B 和 C 的词嵌入并计算词嵌入 D

正如我们在前几章学到的，我们可以通过先将词包装在 `Sentence` 对象中来在 Flair 中嵌入词。例如，给定一个如前所述定义的 `fasttext` 对象，我们的 `Sentence` 对象可以用 `sentence = Sentence('word')` 定义。然后，我们可以通过运行 `fasttext.embed(sentence)` 来嵌入 `Sentence` 对象中的词。我们还可以通过调用 `sentence[0].embedding` 来获取句子中第一个词的词嵌入。这将返回一个 **PyTorch** `torch.Tensor` 对象。它可以通过调用 `sentence[0].embedding.tolist()` 转换为向量分量的列表。

有了这些知识，我们可以实现一个 `compute_embedding_for_D(A, B, C, embedding)` 函数，该函数接收我们的三个词（A、B 和 C）以及一个嵌入对象，并返回计算得到的词 D 的词嵌入向量。

该方法的实现如下：

```python
from flair.data import Sentence

def compute_embedding_for_D(A, B, C, embedding):
    wordsABC_sentence = Sentence(' '.join([A, B, C]))
    embedding.embed(wordsABC_sentence)

    A_embedded = wordsABC_sentence[0].embedding
    B_embedded = wordsABC_sentence[1].embedding
    C_embedded = wordsABC_sentence[2].embedding

    D_embedding = B_embedded + C_embedded - A_embedded

    return D_embedding.tolist()
```

在此辅助函数的第一部分中，我们定义了 `wordsABC_sentence` 对象，其中包含一个包含我们三个输入词 A、B 和 C 的以空格分隔的字符串。然后我们继续嵌入该句子中的所有词并提取我们三个输入词的词嵌入，从而获得名为 `A_embedded`、`B_embedded` 和 `C_embedded` 的词嵌入。使用我们的 $D = B + C - A$ 公式和 Python 的算术运算符，我们然后简单地计算 `D_embedding` 并将其作为列表返回。

要测试我们的方法，我们可以运行以下代码：

```python
from flair.embeddings import WordEmbeddings
fasttext = WordEmbeddings('crawl')
D = compute_embedding_for_D('king', 'man', 'queen', fasttext)
print(D)
```

这应该打印出我们的 D 词嵌入——一个长数字列表，看起来像这样：

```
[0.13569998741149902, -0.3856000006198883, -0.102299957203865,
-0.13539999723434448, -0.01410001516342163,
-0.04699999839067459, ...]
```

鉴于我们的 King is to man as queen is to woman 类比， resulting embedding 应该与词 woman 的词嵌入相似。为了验证这一点，我们需要先计算英语词汇表的词嵌入，然后找到最匹配的词。

### 在 Flair 中获取所有英语词的词嵌入

为了获得在英语中在合理程度上可以预期的词列表，我们将从一个大型英语语料库中提取词汇表。这样，我们将获得一套几乎完整的常用英语词，同时去掉英语中很少使用的词，而我们极不可能遇到这些词，从而减少搜索空间并加快代码运行速度。

然后我们将把这些词作为包含词汇表以空格分隔的列表的单个句子的一部分进行嵌入。这只能在使用非上下文词嵌入时完成。如果我们使用上下文字符串嵌入，则必须为每个词单独定义一个 Sentence 对象。

我们将把解决方案实现为 get_embedded_english_vocab(embedding) 函数，该函数接收嵌入对象并返回一个 Sentence 对象，其中包含我们的整个词汇表作为标记以及相应的词嵌入：

```python
from flair import datasets
from flair.data import Sentence

def get_embedded_english_vocab(embedding):
    dataset = datasets.UD_ENGLISH()
    vocab_list = dataset.make_vocab_dictionary().get_items()
```

```
vocab = Sentence(' '.join(vocab_list))
embedding.embed(vocab)
return vocab
```

get_embedded_english_vocab(embedding) 函数使用 Flair 的 UD_ENGLISH 英语通用依存树库数据集。它将数据集的整个词汇表连接成一个以空格分隔的字符串，然后将其标记化并作为 Sentence 对象的一部分进行嵌入。

可以通过运行以下代码来测试前述函数：

```
from flair.embeddings import WordEmbeddings
fasttext = WordEmbeddings('crawl')
print(get_embedded_english_vocab(fasttext)[6].embedding)
```

在这里，我们初始化一个词嵌入类并将其传递给 get_embedded_english_vocab 函数。然后该片段最终将打印出我们词汇表中第七个词的词嵌入。

鉴于我们现在可以访问英语中可以合理预期的所有词的词嵌入，我们现在可以计算最接近我们计算得到的 D 词嵌入的词。

### 找到最匹配的词

一旦我们计算出 D 词嵌入，我们最终需要找到词嵌入所代表的词。因为词嵌入极不可能与词汇表中的词完全匹配，所以我们需要使用某种相似度度量来找到最匹配的词。我们将使用余弦相似度。为了找到最匹配的词，我们只需计算 D 与词汇表中每个词之间的余弦相似度，并选择相似度分数最高的词。这将实现为 find_closest_matching_word(D, vocab, ABC)，其中 D 是我们计算得到的词嵌入，vocab 是我们嵌入的词汇表，ABC 是词 A、B 和 C 的列表：

```python
from sklearn.metrics.pairwise import cosine_similarity as sim
def find_closest_matching_word(D, vocab, ABC):
    max_match = -1
```

```
for word in vocab:
    match = sim([D], [word.embedding.tolist()])[0][0]
    if match > max_match and word.text not in ABC:
        max_match = match
        closest_matching_word = word.text
return closest_matching_word
```

前述代码简单地遍历我们的 vocab 对象并计算 D 词嵌入与 vocab 对象中每个词之间的相似度。然后我们选择并返回 closest_matching_word，即我们类比求解器的解决方案。

> 提示
> 虽然我们使用算术运算来解决类比的想法在理论上是有意义的，但在实践中结果往往不如你预期的那么合理。经常发生的情况是，新计算的词嵌入实际上最接近我们原始词中的一个。当然，我们希望 D 是一个新词，而不是原始词 A、B 或 C 中的一个。当这种情况发生时，我们可以简单地忽略最匹配的词并选择第二、第三或第四最匹配的词。我们通过在前述 if 语句中检查 word.text not in ABC 来实现这一点，从而避免结果是原始词之一并增加产生合理结果的机会。

在实现了类比求解器所需的所有逻辑后，我们现在可以编写一个简单的辅助函数，将所有函数集成在一起并打印出类比谜题的解决方案：

```python
def A_is_to_B_as_C_is_to(A, B, C):
    fasttext = WordEmbeddings('crawl')
    result = compute_embedding_for_D(A, B, C, fasttext)
    vocab = get_embedded_english_vocab(fasttext)
    D = find_closest_matching_word(result, vocab, {A, B, C})
    
    print(f'{A} is to {B} as {C} is to {D}')
```
