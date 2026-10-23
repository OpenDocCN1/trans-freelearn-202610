

为应对这些挑战，**近似最近邻 (ANN)** 搜索算法，例如 HNSW 图，通常被用于优化搜索过程。这些算法以少量精度的损失换取显著的搜索速度提升，使其成为大规模图像搜索应用的实用选择。

随着 Elasticsearch 密集向量字段类型的出现，现在可以在 Elasticsearch 集群内直接索引和搜索高维向量。这种功能结合适当的特征提取模型，为构建高效且可扩展的图像搜索系统提供了强大的工具集。

在以下章节中，我们将深入探讨图像特征提取、向量索引和搜索技术的细节。我们还将演示如何使用 Elasticsearch 和预训练的 CNN 模型进行特征提取来实现图像搜索系统。总体目标是提供一份全面的指南，指导使用最先进技术构建和优化图像搜索系统。

## 使用图像的向量搜索

向量搜索是 Elasticsearch 和其他向量存储的变革性功能，它能够在图像等复杂数据类型中执行搜索。通过这种方法，图像被转换为可以被索引、搜索并相互比较的向量，从而彻底改变了我们检索和分析图像数据的方式。这种产生嵌入的固有特性同样适用于其他媒体类型。本节提供了使用图像的向量搜索过程的深入概述，包括图像向量化、Elasticsearch 中的向量索引、kNN 搜索、向量相似度度量以及 kNN 算法的微调。

## 图像向量化

向量搜索过程的第一阶段是将图像数据转换为向量，这一过程称为图像向量化。深度学习模型，特别是 CNN，通常用于此任务。CNN 设计用于理解和捕捉图像的复杂特征，例如颜色分布、形状、纹理和模式。通过将图像处理为卷积、池化和全连接节点的层，CNN 可以将图像表示为高维向量。该向量封装了图像的关键特征，作为其数值表示。

预训练 CNN 的输出层（通常称为嵌入或特征向量）通常用于此目的。该向量中的每个维度代表从图像中学习到的某个特征。例如，一个维度可能对应于特定颜色或纹理模式的存在。向量中的值量化了这些特征在图像中存在的程度。

![](img/21c3d4250a746422a7489bc2647611d8_115_0.png)

图 5.1：CNN 模型的层

如前图所示，这些是 CNN 模型的层：

- 1. 接受图像的原始像素值作为输入。
- 2. 每一层提取特定的特征，如边缘、角点、纹理等。
- 3. 引入非线性，从错误中学习，并近似更复杂的功能。
- 4. 通过下采样降低特征图的维度以减少计算复杂度。
- 5. 由前几层的权重和偏置组成，用于进行分类过程。
- 6. 输出类别上的概率分布。

## 在 Elasticsearch 中索引图像向量

一旦获得图像向量，下一步就是将这些向量索引到 Elasticsearch 中以供将来搜索。Elasticsearch 提供了一种特殊的字段类型，dense_vector 字段，用于处理这些高维向量的存储。

dense_vector 字段被定义为数值数组，通常是浮点数，具有指定的维度数（dims）。目前允许索引向量的最大维度数为 2,048，未来可能会进一步增加。需要注意的是，每个 dense_vector 字段是单值的，这意味着不可能在一个这样的字段中存储多个值。

在图像搜索的上下文中，每个图像（现在表示为向量）都被索引到 Elasticsearch 文档中。该向量可以是每个文档一个或每个文档多个向量。表示图像的向量存储在文档中的 dense_vector 字段内。此外，关于图像的其他相关信息或元数据可以存储在同一文档中的其他字段中。

完整示例代码可以在本书 GitHub 仓库的 chapter 5 文件夹中的 Jupyter Notebook 中找到，地址为 https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/tree/main/chapter5，但我们将在此处讨论相关部分。

首先，我们将使用 SentenceTransformer 库初始化一个预训练模型。

clip-ViT-B-32-multilingual-v1 模型将在本章后面详细讨论：

```
model = SentenceTransformer('clip-ViT-B-32-multilingual-v1')
```

接下来，我们将准备图像转换函数：

```
transform = transforms.Compose([
    transforms.Resize(224),
    transforms.CenterCrop(224),
    lambda image: image.convert("RGB"),
    transforms.ToTensor(),
    transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5)),
])
```

Transforms.Compose() 组合了以下所有转换：

- transforms.Resize(224)：在保持宽高比的同时将图像的较短边调整为 224 像素。
- transforms.CenterCrop(224)：裁剪图像中心，使结果图像的尺寸为 224x224 像素。
- lambda image: image.convert("RGB")：这是一个将图像转换为 RGB 格式的转换。这对于灰度图像或带 alpha 通道的图像很有用，因为深度学习模型通常期望 RGB 输入。
- transforms.ToTensor()：将图像（PIL 图像格式）转换为 PyTorch 张量。这会将数据从 PIL 图像格式的 [0, 255] 范围更改为 [0.0, 1.0] 范围的浮点数。
- transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))：使用给定的均值和标准差对每个通道的张量图像进行归一化。在这种情况下，所有三个通道（R、G、B）的均值和标准差均为 0.5。这种归一化会将数据范围从 [0.0, 1.0] 转换为 [-1.0, 1.0]。

我们可以使用以下代码对图像文件应用转换，然后使用模型生成图像向量。有关针对实际图像文件运行的示例，请参阅本章的 Python 笔记本：

```
from PIL import Image
img = Image.open("image_file.jpg")
image = transform(img).unsqueeze(0)
image_vector = model.encode(image)
```

然后可以将向量和其他相关数据索引到 Elasticsearch 中以用于 kNN 搜索：

```
# Create document
document = {'_index': index_name,
'_source': {"filename": filename,
"image_vector": vector
```

请参阅本书 GitHub 仓库的 chapter 5 文件夹中的完整代码。

在向量生成并索引到 Elasticsearch 后，我们可以继续进行相似图像搜索。

## k-最近邻 (kNN) 搜索

向量现在已索引到 Elasticsearch 中，下一步是使用 kNN 搜索。你可以参考第 2 章《Getting Started with Vector Search in Elastic》，以了解有关 kNN 和 HNSW 搜索的完整讨论。

与基于文本的向量搜索一样，在使用图像进行向量搜索时，我们首先需要将查询图像转换为向量。该过程与我们在索引时将图像转换为向量的过程相同。

我们将图像转换为向量并将其包含在 knn 搜索函数的 query_vector 参数中：

```
knn = {
"field": "image_vector",
"query_vector": search_image_vector[0],
"k": 1,
"num_candidates": 10
}
```

在这里，我们指定了以下内容：

- field：索引中包含我们要搜索的图像向量表示的字段
- query_vector：我们查询图像的向量表示
- k：我们只想要一个最接近的图像
- num_candidates：每个分片上要搜索的近似最近邻候选数量

