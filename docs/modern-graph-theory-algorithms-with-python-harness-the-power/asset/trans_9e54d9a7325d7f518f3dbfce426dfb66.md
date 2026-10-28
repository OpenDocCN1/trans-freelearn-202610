

Csárdi G., Nepusz T. The igraph software package for complex network research. *InterJournal Complex Systems*, 1695, 2006.

Ducruet, C., & Beauguitte, L. (2014). Spatial science and network science: review and outcomes of a complex relationship. Networks and Spatial Economics, 14(3-4), 297-316.

Estrada E. *The Structure of Complex Networks: Theory and Applications*. OUP Oxford, 2012.

Fung, I. C. H., Fu, K. W., Chan, C. H., Chan, B. S. B., Cheung, C. N., Abraham, T., & Tse, Z. T. H. (2016). Social media’s initial reaction to information and misinformation on Ebola, August 2014: facts and rumors. Public health reports, 131(3), 461-473.

Hagberg, A., & Conway, D. (2020). NetworkX: Network analysis with Python. https://github.com/networkx.

Kim, M., & Sayama, H. (2017). Predicting stock market movements using network science: An information theoretic approach. Applied network science, 2(1), 1-14.

Kiss, I. Z., Miller, J. C., & Simon, P. L. (2017). Mathematics of epidemics on networks. Cham: Springer, 598, 31.

Mehler, A., Lücking, A., Banisch, S., Blanchard, P., & Job, B. (Eds.). (2016). Towards a theoretical framework for analyzing complex linguistic networks (pp. 3-26). Berlin: Springer.

Mocanu, D. C., Mocanu, E., Stone, P., Nguyen, P. H., Gibescu, M., & Liotta, A. (2018). Scalable training of artificial neural networks with adaptive sparse connectivity inspired by network science. Nature communications, 9(1), 2383.

Valente, T. W. (2005). Network models and methods for studying the diffusion of innovations. Models and methods in social network analysis, 28, 98-116.

## 使用 NetworkX 和 igraph 将数据整理为网络

在本章中，我们将介绍分析项目中常见的许多类型的数据，包括 **空间数据**、**时间数据** 和 **时空数据**。由于这些数据结构并不以网络形式存在，因此必须对其进行预处理以形成网络，以便进行进一步分析。在未来的章节中，我们将详细介绍最佳预处理策略，但本章的目标是熟悉基础知识以及 NetworkX 和 igraph 包中的预处理方法。

在本章及后续章节中，我们将考虑许多现实世界问题，以建立围绕可重新格式化并作为网络科学问题进行分析的数据的直觉。网络算法的计算成本通常低于为时间序列分析或空间数据设计的算法。到本章结束时，你将能够识别许多适合网络分析的问题类型，并为深入研究后续章节中的问题做好准备。

具体来说，本章将涵盖以下主题：

- 不同数据源的介绍
- 使用 igraph 将数据整理为网络
- 使用 NetworkX 的社交网络示例

### 技术要求

要运行本章中的实践示例，你需要熟悉 Python 编程，并必须安装 igraph 和 NetworkX Python 包。

本章的代码可在以下位置获取：https://github.com/PacktPublishing/Modern-Graph-Theory-Algorithms-with-Python

### 不同数据源的介绍

在实践中，我们很少从头创建网络或遇到自然以网络形式存在的数据源。我们通常必须从不同的数据源创建网络，包括调查或传感器数据、地理数据、时间序列数据、人口统计数据，甚至机器学习模型的输出数据。在本节中，我们将在深入介绍使用 igraph 和 NetworkX 进行数据整理的两个实践示例之前，先概述一些常见的数据源。让我们深入了解我们第一个数据源——社交互动数据。

### 社交互动数据

网络科学的很大一部分起源于社交网络，后者捕捉个人之间的关系或互动。营销活动通常会招募在目标人口群体内部与其他人有许多联系的个人，以及其联系人也在该人口群体内部有许多联系的个人。例如，一家玩具制造商可能有一款将在圣诞节推出的新玩具，也许是一款 *蒸汽朋克胡桃夹子芭蕾老鼠王娃娃*（如图 *2.1* 所示），他们希望根据市场研究关于哪些年龄和性别群体最有可能观看《胡桃夹子》芭蕾舞而将其营销给 5 至 11 岁的欧洲女孩：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_42_0.png)

图 2.1 – 圣诞节首发的全新老鼠王娃娃的营销广告

玩具制造商可能会寻找社交媒体影响者（在 YouTube 上拥有大量订阅者或在 TikTok 上拥有大量粉丝的个人），其观众网络主要由 5 至 11 岁的女孩组成，这些女孩从欧洲观看影响者的内容。有时，这些数据可以通过抓取影响者的内容 readily 获得。其他时候，这些数据需要分块收集并先进行分析，以便通过内容策划和相关内容指标的抓取，在感兴趣的人口群体中缩小影响者的范围。

构建社交网络的另一个常见数据源是 **调查数据**，其中个人对社交群体中其他人的友谊或互动模式进行排名。例如，学生可能会指出他们一起学习、社交或上课的其他学生。与彼此在许多环境中互动的学生共享更强的社交互动，而互动不频繁或仅在某些环境中互动的学生共享较弱的社交互动。加权社交网络不仅可以可视化存在的互动，还可以可视化其强度。在社会科学环境中，更强的联系在信息传播或产品或行为的采纳中通常发挥着重要作用。例如，当一名青少年开始吸烟或饮酒时，与该个体有强联系的个体开始这些行为的风险更高。在高度连接的青少年群体中，当一个个体采纳一种冒险行为时，该行为在群体中传播的风险很高。

较弱的社交联系可能与强社交联系一样重要（甚至更重要！），并且在社会学领域，这些联系构成了个人社会资本的基础。社会资本衡量个人在需要时可以从其所联系的人或其联系人的联系人那里获得的资源。在求职的背景下，求职者最强且最持久的联系可能并不是找到新工作所需的。然而，很可能朋友的朋友和熟人构成了更大的社交网络，这些联系可能会为求职者提供新的就业机会。通过口碑或社交媒体上的工作广告，朋友的朋友和熟人的更大网络通常能提供更多找到下一份工作的机会。

我们将在本章的 *使用 igraph 将数据整理为网络* 部分返回到基于调查的社交网络数据，在那里我们根据社交环境中的认可互动构建学生网络。

### 空间数据

另一个常见的网络数据源是来自地理信息系统收集或按地理组织的数据。**空间数据分析** 是数据科学的一个分支，分析基于地理的数据；然而，当数据覆盖大地理区域或涉及复杂的统计计算时，以网络科学的形式表述数据和问题可能更容易。网络算法通常比空间数据算法更快，并且可以轻松地将大型网络存储在 Neo4j 等图数据库中。

### 时间数据

网络很少是静态的。随着联系的建立和断裂，社交网络会随时间演变。随着新商品的加入、特定商品需求随季节变化，或经济变化引发消费者行为变化，销售模式也会发生变化。股票市场交易量和行业/区域相关性会随着政治和商业环境的改变而随时间变化。

所有这些变化都涉及数据中的时间成分。网络中建模时间趋势有几种方法，我们将从两个用例开始，并在本书的第 3 部分中构建更多用途。

首先，让我们回到求职者利用其社交网络寻找新工作的例子。随着个人开始求职，他们可能会在 LinkedIn 上添加建议的专业联系人、参加当地的社交活动，或与几位招聘人员建立联系；所有这些行动都会增加其社交网络的联系（并增加找到新工作的可能性）。图 2.3 显示了女性在社交活动上会面，在求职中建立新的专业联系：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_45_0.png)

图 2.3 – 8 周内不断变化的社交网络

