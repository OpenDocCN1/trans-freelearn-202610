

尼罗-撒哈拉语系的语言学研究可追溯至至少 1800 年代，过去十年来许多论文都在争论语言漂移以及母语与子语之间的关系。在本节中，我们将考察其中几种提出的层级结构，然后在下一节中利用 Python 代码返回分析这些层级结构的相似性和差异。如果你对论文本身感兴趣，可以查阅本章末尾的参考文献，特别是 Dimmendaal 的论文，它总结了尼罗-撒哈拉语系中亲子关系最近的争论。

Greenberg 在 1963 年提出了以下层级结构，对若干语言家族分支进行了分类。图 11.4 显示了该层级结构，包括从更大的尼罗-撒哈拉语系的一个子分支衍生的子分支：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_223_0.png)

图 11.4 – Greenberg 对尼罗-撒哈拉语系的分类

图 11.4 显示了尼罗-撒哈拉语系的部分亚分类，这些亚分类属于该语言家族。每一个亚分类都继续延伸到单个语言，总计约有 130 种语言分布于该地区，这些语言起源于尼罗河支流汇合的地区，即曾经的努比亚。

自 1963 年以来，许多论文进一步细分了这些子类并增加了额外的子类，最终形成了我们将在本节讨论的 Dimmendaal 论文，有时形成了非常复杂和深层的网络结构。语法、用词、音系学（就本例而言是辅音的发音）以及术语通常决定了划分界限的位置，但语言往往是流动的，可能在某种程度上重叠，使得分类系统难以辨别。让我们转向尼罗-撒哈拉语系的最新研究之一。

Dimmendaal 的最新研究提出了尼罗-撒哈拉语亚家族的新层级结构，总结于图 11.5：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_224_0.png)

图 11.5 – Dimmendaal 的尼罗-撒哈拉语亚家族层级结构

**注意**
在图 11.5 中，Dimmendaal 的分类首先按地区划分语言，然后按方言或地区划分。还要注意的是，一些语言亚家族似乎是共同出现的（例如 Fur 和 Kunama），而没有中间家族的存在。这比图 11.4 中 Greenberg 的分类结构更为复杂。

随着语言被更系统地研究，语言家族层级结构随时间演变，记录出现的差异及其潜在原因非常重要。这照亮了进一步语言学研究的途径，并可为未来语言家族层级结构的发展做出贡献。

现在我们对尼罗-撒哈拉语言及其层级结构有了一些了解，让我们看看网络科学如何帮助我们理解可能重要的结构差异。

## 注意

我们的示例规模非常有限，因此结果易于可视化，并且我们分析可能显示差异的原因也很清楚。使用完整的 OWL 数据库或完整的语言层级结构会产生更细致和更大的网络来研究。然而，无论网络规模或复杂性如何，所使用的工具都是相同的。因此，我们即将开始的分析可以应用于更复杂的语言数据结构，在这些结构中，仅仅可视化不同研究小组创建的 OWL 数据库或不同语言家族层级结构之间的差异可能不可行。

## 映射语言家族

让我们回到尼罗-撒哈拉语言家族树的两个示例。为了检查结构差异，我们首先需要在 NetworkX 中创建它们。对于 Dimmendaal 家族树，我们将稍微偏离树结构，以显示某些子亚家族之间存在的相互关系。让我们开始通过脚本 11.1 创建并绘制 Greenberg 树：

```
#load needed packages
import numpy as np
import networkx as nx
import matplotlib.pyplot as plt

#create the Greenberg Nilo-Saharan language family network
G = nx.Graph()
G.add_nodes_from([1, 11])
G.add_edges_from([(1,2),(1,3),(1,4),(1,5),(1,6),(1,7),(7,8),
    (7,9),(7,10),(7,11)])

#plot the Greenberg Nilo-Saharan language family network
import matplotlib.pyplot as plt
G.nodes[1]['subfamily'] = 'Nilo_Saharan'
G.nodes[2]['subfamily'] = 'Koman'
G.nodes[3]['subfamily'] = 'Saharan'
G.nodes[4]['subfamily'] = 'Songhay'
G.nodes[5]['subfamily'] = 'Fur'
G.nodes[6]['subfamily'] = 'Maban'
G.nodes[7]['subfamily'] = 'Chari_Nile'
G.nodes[8]['subfamily'] = 'Central_Sudanic'
G.nodes[9]['subfamily'] = 'Kunama'
G.nodes[10]['subfamily'] = 'Berta'
G.nodes[11]['subfamily'] = 'Eastern_Sudanic'
labels = nx.get_node_attributes(G, 'subfamily')
nx.draw(G, labels=labels, font_weight='bold')
```

```
labels2 = nx.get_node_attributes(G2, 'subfamily')
nx.draw(G2, labels=labels2, font_weight='bold')
```

脚本的这一部分生成了 Dimmendaal 的尼罗-撒哈拉语亚家族树的图，显示在图 11.7 中：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_227_0.png)

图 11.7 – Dimmendaal 的尼罗-撒哈拉语亚家族树的图

注意 Dimmendaal 的尼罗-撒哈拉语亚家族树中 Kunama 与 Fur 之间、以及 Saharan 与 Eastern Sudanic 之间存在的连接。这些代表了尼罗-撒哈拉语东北分支内相关的子亚家族。从图 11.6 和 11.7 可以看出总体结构相似，其中存在大量浅层且不相连的分支。然而，Dimmendaal 的尼罗-撒哈拉语亚家族树在树中增加了额外的一层，并包含了相关亚家族，而这些亚家族没有共同祖先直接产生它们。

鉴于这些差异，我们预计两种语言亚家族树之间的中心性指标会有所不同。我们将从前几章中定义的一些熟悉指标开始。脚本 11.2 分析了我们两种语言亚家族树之间平均中介中心性的差异：

```
#compare betweenness centrality of language families
gb=nx.betweenness_centrality(G)
print(np.mean(np.array(list(gb.values()))))
gb2=nx.betweenness_centrality(G2)
print(np.mean(np.array(list(gb2.values()))))
```

我们得到平均中介中心性为 0.131（Greenberg）和 0.127（Dimmendaal），表明这些亚家族树的桥接属性相似。让我们通过添加到脚本 11.2 来计算平均度中心性来评估枢纽属性：

```
#compare degree centrality of language families
gd=nx.degree_centrality(G)
print(np.mean(np.array(list(gd.values()))))
gd2=nx.degree_centrality(G2)
print(np.mean(np.array(list(gd2.values()))))
```

我们得到平均度中心性为 0.182（Greenberg）和 0.278（Dimmendaal），表明这些亚家族树的枢纽属性在某种程度上有所不同。鉴于 Dimmendaal 的亚家族树结构中连接其他层的额外深度层，我们预计会看到这种差异。

让我们将注意力转向一种特别适合真实世界语言数据（以及其他本体或系统发育网络数据集）的中心性指标。

**子图中心性**衡量每个顶点在网络子图中的参与程度，通过加权网络邻接矩阵的谱，并通过网络上的随机游走估计子网络，对小型子网络赋予较大权重，对大型子网络赋予较小权重。这融合了**谱聚类**（见*第 5 章*）和度中心性的属性，创造出一种混合中心性度量。

我们可以在两个亚家族树上看到这一点，通过添加到脚本 11.2：

```
#compare subgraph centrality of language families
gs=nx.subgraph_centrality(G)
print(np.mean(np.array(list(gs.values()))))
gs2=nx.subgraph_centrality(G2)
print(np.mean(np.array(list(gs2.values()))))
```

该脚本显示两个亚家族树的平均子图中心性有所不同，Greenberg 的平均子图中心性为 2.478，Dimmendaal 的平均子图中心性为 3.276。这表明这些假设亚家族树的谱和度分布是不同的。

虽然本示例仅涉及两个小的本体结构，但当处理许多本体或比较非常大的本体而可视化不被推荐时，这类分析会很有帮助。例如，我们可能希望比较许多不同论文中假设的完整的尼罗-撒哈拉语言树。这些树将包括数百个顶点和可能数十棵树。视觉上跟踪差异将是一项相当大的挑战！

