
# Transformer架构：嵌入、位置编码与注意力机制

## 简介

Transformer的出现源于处理序列数据中长距离依赖的需求，传统的**循环神经网络**（**RNN**）在这方面表现吃力，尤其是在**自然语言处理**（**NLP**）领域。Vaswani等人（2017年）发表的里程碑式论文《*Attention is All You Need*》（前文已提及）介绍了Transformer架构，该架构用注意力机制取代了循环层。这一转变使得训练过程中的并行化更加高效，并提升了涉及大规模数据集任务的性能。Transformer是一种旨在理解和生成数据序列（如句子）的模型。它不再按顺序逐个处理单词，而是一次性查看所有单词。其核心思想是*自注意力*，这使得模型能够根据句子各部分之间的相关性，聚焦于句子的不同部分。

## 嵌入输入

当Transformer处理文本时，它首先将单词转换为模型能理解的数字（向量）。这一步称为**嵌入**。例如，假设我们有一个单词序列：，其中每个`x_i`代表序列中的一个单词。由于单词被表示为独热向量（充满零的大向量，仅有一个1表示该单词），我们使用**嵌入矩阵** `W_e` 将这些独热向量转换为稠密、有意义的向量。顺便提一下，出于实际考虑，每个 `x_i` 可以是一个token（单词的一种定义好的分割方式）。但为了便于理解，我们这里将每个单词视为一个token。数学上，如果 `W_e \in \mathbb{R}^{V \times d_model}`，其中 `V` 是词汇表大小，`d_model` 是模型的隐藏维度，则每个单词嵌入为：`z_i = W_e^T x_i`。结果是一个嵌入序列：`Z = (z_1, ..., z_n)`，其中每个 `z_i` 是一个大小为 `d_model` 的稠密向量。**直观理解**：可以将嵌入视为一种捕捉单词含义的表示方式。含义相近的单词将拥有相似的嵌入，这有助于模型理解单词之间的关系。

![Figure 5.1 嵌入输入涉及使用嵌入模型将文本单词或token转换为数值向量。](media/file28.png)

图5.1 嵌入输入涉及使用嵌入模型将文本单词或token转换为数值向量。

## 位置编码

Transformer并行处理整个序列，但这带来了一个挑战：模型如何知道句子中单词的顺序？例如，"The sun rises from the east"（太阳从东方升起）在含义和语义上与 "The east rises from the sun"（东方从太阳升起）截然不同，尽管单词完全相同。为了解决这个问题，Transformer向每个单词嵌入添加了所谓的*位置编码*。这种编码注入了序列中每个单词位置的信息。每个位置 `t` 的位置编码在数学上定义为：`PE(t, 2i) = sin(t / 10000^{2i/d_model})`，`PE(t, 2i+1) = cos(t / 10000^{2i/d_model})`。这为每个单词的位置创建了一个唯一的位置向量 `p_t`。模型随后将该向量加到单词嵌入上：`z_i' = z_i + p_i`。这个新序列现在成为Transformer编码器-解码器架构的输入，从自注意力层开始。架构的一个简单组件如下所示：

![Figure 5.2 向输入嵌入添加位置编码，输出一个新序列Z，该序列考虑了单词或token相对于其他单词/token的位置。](media/file29.png)

图5.2 向输入嵌入添加位置编码，输出一个新序列Z，该序列考虑了单词或token相对于其他单词/token的位置。

位置编码帮助模型知晓单词的顺序。如果没有它，模型会将 "The sun rises from the east" 和 "The east rises from the sun" 视为相同。使用PyTorch可以按下面的代码片段所示实现：

```python
import torch
from torch import nn

class PositionalEncoding(nn.Module):
    def __init__(self, d_model, dropout=0.1, max_len=5000):
        super(PositionalEncoding, self).__init__()
        self.dropout = nn.Dropout(p=dropout)
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len, dtype=torch.float).unsqueeze(1)
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * -(torch.log(torch.tensor(10000.0)) / d_model))
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        pe = pe.unsqueeze(0)  # Shape (1, max_len, d_model)
        self.register_buffer('pe', pe)

    def forward(self, x):
        x = x + self.pe[:, :x.size(1), :]
        return self.dropout(x)
```

在上述片段中，我们创建了一个形状为 `(max_len, d_model)` 的零矩阵，以确保 `pe` 具有适当的维度。`d_model` 维度容纳了正弦和余弦两个分量，因此它是其参数 `position * div_term` 中项乘积的两倍。由于正弦和余弦分量内的参数都使用 `position` 作为第一项，其形状为 `(max_len, 1)`。同样，第二个 `div_term` 形状为 `(d_model/2)`，加起来为 `d_model`，一半用于正弦，一半用于余弦。在 `d_model` 中，正弦和余弦分别应用于偶数和奇数索引，正如 `pe[:, 0::2]` 和 `pe[:, 1::2]` 的实现方式所示。我们在 `PositionalEncoding` 类的 `__init__` 方法中添加所有这些内容。`forward` 方法将位置编码加到输入张量上，并在应用 `dropout` 进行正则化以防止过拟合后返回。现在我们了解了如何将位置编码添加到输入嵌入中，并融入了单词位置的重要性，接下来我们将探讨将单词关系学习联系在一起的自注意力机制。

## 自注意力：学习单词关系

**自注意力**机制是Transformer的核心。它允许模型在处理每个单词时，聚焦于句子中的不同单词。例如，在句子 "The sun rises from the east" 中，当模型处理 "rises" 时，它应同时关注 "sun"（主语）和 "east"（宾语）以理解含义。对于每个单词，模型创建三个向量：

*   一个**查询** `Q`，
*   一个**键** `K`，
*   一个**值** `V`。

这些向量通过将输入 `Z` 与学习到的权重矩阵相乘来计算：`Q = Z W_Q`，`K = Z W_K`，`V = Z W_V`。为了确定每个单词应给予其他单词多少注意力，模型计算一个单词的查询与所有单词的键之间的相似度分数。这通过点积和缩放完成：`scores = (Q K^T) / sqrt(d_k)`。`softmax` 函数确保分数之和为1，将其转换为概率。这给出了值向量的加权组合，更重要的单词获得更大的权重。自注意力让每个单词"审视"句子中的其他单词，并聚焦于对理解含义最重要的那些。这就像为句子的每个部分高亮显示最重要的单词。

![Figure 5.3 展示自注意力（缩放点积）和多头注意力的示意图。](media/file30.png)

图5.3 展示自注意力（缩放点积）和多头注意力的示意图。

## 多头注意力：从多视角观察
