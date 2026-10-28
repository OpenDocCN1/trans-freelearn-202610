

这应该会给出一个高度互联的学生网络的图示，显示在 *图 3.9：*

![](img/71ff737206f7d80bb625b3c9aca9dc7f_80_0.png)

图 3.9 – 按原籍国和专业领域连接的 AIMS 喀麦隆学生网络

图 3.9 显示了一个密集连接的网络，表明许多学生很可能与许多其他学生互动。该网络没有很多桥梁，但大多数顶点似乎是枢纽。在非常密集的网络中，传播过程往往移动得更快。该网络中顶点之间的最短路径与法语非洲网络中的相比会相对较短，因为大多数顶点彼此连接或几乎连接。网络的直径，即最长最短路径，表示信息或疾病在两个顶点之间传播的最大最短旅行路线。小的网络直径（或小的平均最短路径长度）表明传播过程的效率，例如信息、趋势或疾病。因此，我们预计我们的 AIMS 喀麦隆学生网络比我们的法语非洲网络更容易受到传播过程的影响。从技术上讲，直径为定义在网络上的传播过程等微分方程提供了界限。

在这个网络上 R₀ 超过 1 的流行病很可能会迅速传播并感染网络的大部分。让我们在 AIMS 喀麦隆学生网络上运行一个与我们在法语网络上相同的 beta 和 gamma 参数的流行病，并绘制结果，以了解类似于西班牙流感的流行病可能如何通过我们的学生群体传播，通过向脚本 3.2 添加内容：

```
#Run the simulated epidemic on the AIMS Cameroon student network
model = ep.SIRModel(G)
config = mc.Configuration()
config.add_model_parameter('beta', 0.25)
config.add_model_parameter('gamma', 0.1)
config.add_model_parameter("fraction_infected", 0.05)
model.set_initial_status(config)
iterations = model.iteration_bunch(50)
trends = model.build_trends(iterations)
viz = DiffusionTrend(model, trends)
p = viz.plot(width=500, height=400)
show(p)
```

图 3.10 显示了流行病的动态，包括在流行病高峰时有许多感染的学生，并且到第 32 周流行病减弱时几乎整个网络都被感染：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_82_0.png)

图 3.10 – beta = 0.25 且 gamma = 0.1 的流行病在 AIMS 喀麦隆学生网络中传播

图 3.10 显示了一个非常严重的流行病，很可能需要关闭课程并隔离感染的学生。在第 8 章中，我们将考虑您可以利用的具体策略，以降低在人口层面流行病的影响，包括移除连接人群或形成枢纽的关键顶点。现在，让我们修改脚本 3.2 以运行第二种类型的流行病，其中 R₀ 小于 1（beta = 0.25 且 gamma = 0.3）：

```
#Run the simulated epidemic on the AIMS Cameroon student network
model = ep.SIRModel(G)
config = mc.Configuration()
config.add_model_parameter('beta', 0.25)
config.add_model_parameter('gamma', 0.3)
config.add_model_parameter("fraction_infected", 0.05)
model.set_initial_status(config)
iterations = model.iteration_bunch(50)
trends = model.build_trends(iterations)
viz = DiffusionTrend(model, trends)
```

```
```python
p = viz.plot(width=500, height=400)
show(p)
```
```

*图 3.11* 显示了一种非常不同类型的流行病，它并没有开始感染大量学生并迅速消退：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_83_0.png)

图 3.11 – beta = 0.25 且 gamma = 0.3 的流行病在 AIMS 喀麦隆学生网络中传播

正如我们从 *图 3.11* 中看到的，即使是密集连接的网络也并非对某些类型的流行病构成威胁。如果没有通过网络结构或通过影响传播潜力的疾病特征来有效传播的正确条件，流行病会在人群中消退。然而，一种病毒有可能从 *图 3.11* 所示的初始动态突变为更具传染性的东西。

如本章前面所述，1918 年西班牙流感毒株从一种传染病突变为一种高度传染性疾病，使 R₀ 从刚好超过 1（允许传播）变为 3.5，预示着一场全球灾难。实际上，在模拟通过感兴趣网络传播的流行病时，考虑各种现实的参数值很重要，因为参数的微小差异可能导致非常不同的结果；流行病规划在模拟各种场景时效果最好。

## 总结

在本章中，我们介绍了人口统计因素以及它们如何通过同质性影响网络发展。我们还介绍了枢纽和桥梁的概念。然后我们转向 SIR 模型，以显示基于微分方程模型中传播因素的社会网络中的趋势和疾病传播。我们考虑了两个网络：邻近法语国家的地理网络和基于人口统计因素推断边的 AIMS 喀麦隆学生网络。然后我们通过改变流行病参数模拟流行病，检查了 R₀ 在两个网络上的作用；这证明了趋势或疾病在网络中传播需要特定的条件。我们将在第 8 章再次重温这些想法和影响传播行为的策略，当我们更详细地深入研究中心性度量时，关于动态网络指标如何随着个人随着时间的推移改变与其他人的联系而随时间变化。

在下一章中，我们将考虑网络在运输物流中的作用以及如何将运输路线数据表示为网络。

## 参考文献

Aiello, L. M., Barrat, A., Schifanella, R., Cattuto, C., Markines, B., & Menczer, F. (2012). Friendship prediction and homophily in social media. ACM Transactions on the Web (TWEB), 6(2), 1-33.

Cooper, I., Mondal, A., & Antonopoulos, C. G. (2020). A SIR model assumption for the spread of COVID-19 in different communities. Chaos, Solitons & Fractals, 139, 110057.

Das, K., Samanta, S., & Pal, M. (2018). Study on centrality measures in social networks: a survey. Social network analysis and mining, 8, 1-11.

Estrada, E., Kalala-Mutombo, F., & Valverde-Colmeiro, A. (2011). Epidemic spreading in networks with nonrandom long-range interactions. Physical Review E, 84(3), 036110.

Iyaniwura, S. A., Rabiu, M., David, J. F., & Kong, J. D. (2022). The basic reproduction number of COVID-19 across Africa. Plos one, 17(2), e0264455.

McPherson, M., Smith-Lovin, L., & Cook, J. M. (2001). Birds of a feather: Homophily in social networks. Annual review of sociology, 27(1), 415-444.

Rodrigues, F. A. (2019). Network centrality: an introduction. A mathematical modeling approach from nonlinear dynamics to complex systems, 177-196.

Smith, H. L. (2003). Some thoughts on causation as it relates to demography and population studies. Population and Development Review, 29(3), 459-469.

Weiss, H. H. (2013). The SIR model and the foundations of public health. Materials mathematics, 0001-17.

*图 4.1* 的都市区显示了一些附近的商店（右下角）和一些彼此相距较远的商店（例如左边的那些）。也许我们正从附近的农场向其中五家商店运输新鲜水果和蔬菜。在不考虑街道、加油站或其他考虑因素的情况下（我们将在本章后面讨论这些），似乎商店之间的路线很容易定义。*图 4.2* 显示了这样一条路径：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_87_0.png)

*图 4.2* — 向假设郊区各家杂货店运送水果和蔬菜的路线规划

与 *图 4.2* 相反，在实际情况中，卡车在商店之间可能有许多路线可走。可能存在替代街道和路线组合。卡车可能需要在商店之间加油，加油站的位置可能会影响理想路线的选择。道路可能因施工而封闭。下午的雷暴可能会淹没一些好路上的街道，造成卡车需要避免的危险。一些商店可能缺橙子和葡萄柚，这使得它们需要优先进行早期送货，以便顾客可以购买这些水果。

