

点积在处理同时包含正值和负值的数据，并且向量之间的角度并不重要时很有趣。点积可以为正、为负或为零。它没有归一化，因此向量的大小会影响结果。此外，如果向量是归一化的（即其大小为 1），则归一化向量的余弦相似度等价于点积：

cos similarity = A . B = dot product

这是一个有价值的信息，因为在 Elasticsearch 上使用点积进行向量搜索速度更快。

在表示文本的向量的上下文中，向量可以同时具有负值和正值。这些值源自训练过程以及用于创建嵌入的算法。这些向量中的正值或负值本身并不一定具有特定的含义。因此，具有较大大小但方向非常不同的两个向量会具有较大的点积，即使它们在语义上并不相似。

相比之下，余弦相似度源自点积，它归一化了向量的大小并关注它们之间的角度，这使得它们更适合文本数据。它衡量向量之间角度的余弦，从而捕捉其语义相似性。由于它是归一化的，范围从 −1 到 1，余弦相似度对特征尺度不那么敏感，也对大小不那么敏感。

请注意这里需要理解的两个重要组成部分：

- 方向，它由向量之间的角度定义
- 大小

让我们详细了解这些组成部分。

## 方向和大小

我们已经看到，在文本数据的上下文中，向量通常使用词嵌入或文档嵌入创建，它们是捕捉单词或文档在连续向量空间中的语义含义的稠密向量表示。

这些向量的方向和大小与实际文本通过数据集中词语之间的关系相关。文本的向量表示的方向表示文本在高维向量空间中的语义取向。方向相似的向量表示语义相似的文本。它们共享相似的上下文或含义。换句话说，当两段文本在语义上相关时，它们的向量之间的角度很小。

相比之下，如果角度很大，则表示文本具有不同的含义或上下文。这就是为什么余弦相似度专注于向量之间的角度，是衡量文本数据中语义相似性的热门选择。

以下是向量方向的一个示例，假设以下样本将使用词嵌入技术转换为向量表示：

- A: “The cat is playing with a toy.”
- B: “A kitten is interacting with a plaything.”
- C: “The chef is cooking a delicious meal.”

A 和 B 之间的角度可能很小；它们含义相似。相比之下，表示 A 和 C 的向量之间的角度肯定会很大，因为它们在语义上不同。

向量的大小表示文本在向量空间中的权重。在某些情况下，大小可能与文本中词语的频率或文本在数据集中的重要性相关。然而，大小也可能受到诸如文本长度或某些词语的存在等因素的影响，这些因素可能表明语义相似性。

这里是另一个示例：

- D: “Economics is the social science that studies the production, distribution, and consumption of goods and services.”
- E: “Economics studies goods and services.”

D 更长，并提供了经济学更详细的定义。E 更短，并提供了更简洁的定义。这种长度差异可能导致表示 D 的向量的大小大于 E。然而，这种差异并不表示语义上的差异。事实上，D 和 E 含义相似，应被视为语义相似，尽管它们的幅度不同。

在高维空间中比较文本时，如果目标是捕捉语义相似性，方向通常比大小更重要，因为角度直接代表词语之间的相似性。

以下是您可以在 Google Collab 中运行的代码示例，您可以在本书 GitHub 仓库的 chapter 2 文件夹中找到它 (https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/tree/main/chapter2)，以查看我们文本样本的向量表示的欧几里得距离、余弦相似度和大小。我们将使用 spaCy 库来创建向量表示：

```
# Install spaCy and download the 'en_core_web_md' model
!pip install spacy
!python -m spacy download en_core_web_md
import spacy
import numpy as np
from scipy.spatial.distance import cosine, euclidean
```

```
# Load the pre-trained word embeddings model
nlp = spacy.load('en_core_web_md')
# Define the texts
text_a = "The cat is playing with a toy."
text_b = "A kitten is interacting with a plaything."
text_c = "The chef is cooking a delicious meal."
text_d = "Economics is the social science that studies the production, distribution, and consumption of goods and services."
text_e = "Economics studies goods and services."
# Convert the texts to vector representations using the spaCy model
vector_a = nlp(text_a).vector
vector_b = nlp(text_b).vector
vector_c = nlp(text_c).vector
vector_d = nlp(text_d).vector
vector_e = nlp(text_e).vector
# Calculate the cosine similarity between the vectors
cosine_sim_ab = 1 - cosine(vector_a, vector_b)
cosine_sim_ac = 1 - cosine(vector_a, vector_c)
cosine_sim_de = 1 - cosine(vector_d, vector_e)
print(f"Cosine similarity between Text A and Text B: {cosine_sim_ab:.2f}")
print(f"Cosine similarity between Text A and Text C: {cosine_sim_ac:.2f}")
print(f"Cosine similarity between Text D and Text E: {cosine_sim_de:.2f}")
# Calculate the Euclidean distance between the vectors
euclidean_dist_ab = euclidean(vector_a, vector_b)
euclidean_dist_ac = euclidean(vector_a, vector_c)
euclidean_dist_de = euclidean(vector_d, vector_e)
print(f"Euclidean distance between Text A and Text B: {euclidean_dist_ab:.2f}")
print(f"Euclidean distance between Text A and Text C: {euclidean_dist_ac:.2f}")
print(f"Euclidean distance between Text D and Text E: {euclidean_dist_de:.2f}")
# Calculate the magnitudes of the vectors
magnitude_d = np.linalg.norm(vector_d)
magnitude_e = np.linalg.norm(vector_e)
print(f"Magnitude of Text D's vector: {magnitude_d:.2f}")
print(f"Magnitude of Text E's vector: {magnitude_e:.2f}")
```

输出将向您展示距离计算的结果，这将为您提供在这种情况下使用哪一种的视角。

在此时，您应该很好地掌握构成文本向量表示的一些基本概念，以及如何确定这些向量之间的相似性，从而确定文本之间的语义相似性。我们现在将探索 Elastic 并查看如何将其付诸实践。

## 新的向量数据类型和向量搜索查询 API

在本章的这一点上，您应该对 Elasticsearch 中的相关性排序有很好的理解，以及向量如何扩展搜索在关键词搜索甚至无法竞争的领域中的能力。我们还介绍了向量如何组织成 HNSW 图、在 Elasticsearch 中存储在内存中，以及评估向量之间距离的选项。现在，我们将把这些知识付诸行动，通过了解 Elasticsearch 中可用的 dense vector 数据类型、设置我们的 Elastic Cloud 环境，最后构建和运行向量搜索查询。

## 稀疏向量和稠密向量

Elasticsearch 支持映射中作为新数据类型的一部分的 dense_vector。它用于存储数值数组。这些数组是文本语义的向量表示。最终，稠密向量在向量搜索和 kNN 搜索的上下文中被利用。

dense_vector 类型的文档可以在这里找到：https://www.elastic.co/guide/en/elasticsearch/reference/current/dense-vector.html。

稀疏向量是具有很少非零值的向量，大多数维度具有零值。这导致低维向量空间，比稠密向量更节省内存且处理速度更快。

例如，考虑一个包含 100,000 个词的词汇表和一个包含 100 个词的文档。如果我们使用稠密向量表示该文档，我们需要为 100,000 个词分配内存，即使其中大部分为零。相比之下，相同的文档使用稀疏向量只需要 100 个非零值的内存，这是一个显著的内存使用量减少。原因在于文档或文本的稠密向量表示为词汇表中每个可能的词分配一个非零值。

如果您想亲眼看到一个示例，这里有一个笔记本构建了文本的稀疏和稠密向量表示，并帮助您通过热图直观地了解两者之间的差异：

```
import numpy as np
from scipy.sparse import random
from sklearn.decomposition import TruncatedSVD
import matplotlib.pyplot as plt

# Generate a corpus of 100 documents, each containing 1000 words
```

