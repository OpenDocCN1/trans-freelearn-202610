
该模拟显示，到我们的观察期结束时，一场疫情感染了鹭鸟种群近一半的个体，且到第二个时间点时感染和恢复水平均较低。新的感染会周期性出现，其中一只鹭鸟的感染期似乎比预期的更长（第4–7期）。图8.10可视化了这一疫情的动态（由于感染模型是概率性的，你运行的结果可能有所不同）：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_176_0.png)

图8.10——鹭鸟网络上的疫情可视化

如果这场疫情发生在真实的鹭鸟种群中，或许可以对那只感染期较长的鹭鸟进行隔离，以避免疫情在种群中进一步扩散。在受保护种群中，例如野生动物保护区或康复中心的种群，以及为人类食用而养殖的种群，这种策略通常用于防止疾病在动物间传播。而在野外，很难识别被感染的动物，甚至很难在监测区域内出现大量动物生病之前就发现疫情爆发。

现在，让我们通过向脚本8.3中补充内容来考虑疫情过程中的网络指标，并再次添加一个try/except子句，因为在观察期间鹭鸟有时会被隔离：

```
#obtain network centrality statistics for the blue heron network
for i in range(10):
    dg = blue_heron.time_slice(i)
    try:
        # computing betweenness centrality
        dd = nx.betweenness_centrality(dg)
        plt.plot(dd.values())
        plt.title("betweenness centrality")
        print()
```

```
plt.show()
print ("timestamp = ", i,",", "nodes =",
    len(dg.nodes()),",",
    "max degree = ",max(dg.degree().values()))
print ("diameter = ", nx.diameter(dg))
print ("radius = ", nx.radius(dg))
except:
    print('Infinite value observed.')
```

在第一个观察周期内，我们得到了一个介数中心性图，其数值变化很大，如图8.11所示：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_177_0.png)

在整个观察周期中，介数中心性变化很大，从接近1到0。直径从1（时间点6）变化到6（时间点8）；半径从1（时间点6）变化到3（时间点7和8）。孤立的顶点很常见，我们看到种群在互动中分裂（时间点3和9），有些鹭鸟在不同地点互动，有些则完全不互动。图8.12展示了一个分裂的鹭鸟种群示例：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_178_0.png)

图8.12——在时间点3观察到的鹭鸟种群分裂互动模式

互动模式的频繁变化可能限制了这场疫情的潜在传播，因为相对于鳄鱼种群中频繁且持续的互动，这些鹭鸟的互动频率足够低、互动持续时间足够短，从而限制了对传染个体的暴露。在野外，同种动物之间互动的频率和持续时间，对于决定共享同一地理区域的物种发生种群疫情的可能性起着关键作用。独居物种的传染病传播率往往低于形成大型兽群或鸟群的物种。

自然发生的现象，如饵球或暴风雨后的临时水源，为疾病在独居物种或孤立兽群之间传播提供了有时限的互动机会。因此，它们为保护主义者提供了一个机会，来研究那些除了涉及食物或水等必需资源的一次性事件外可能不经常互动的动物种群中的流行潜力。我们所研究的这类模型提供了工具，用于估计传染病对通常相互隔离但在此类事件中混合的种群的影响。

## 总结

在本章中，我们通过鳄鱼和蓝鹭种群中疾病传播的例子，探讨了动态网络以及网络结构随时间的变化如何影响疫情传播。我们还探讨了网络指标与疫情传播之间的关系，指出了在假设种群中与更高传播率和更严重疫情相关的连接模式。这些工具对于理解现实世界网络中的趋势和疫情传播至关重要，使研究人员能够规划关键基础设施，以应对各种危机，例如人类群体中的COVID-19或埃博拉、威胁濒危动物种群的传染病、国家选举前的假新闻，或社交媒体上在青年中传播的危险行为趋势。

在下一章中，我们将把重点转向网络上的机器学习（ML）。

## 参考文献

Bianconi, G., Darst, R. K., Iacovacci, J., & Fortunato, S. (2014). Triadic closure as a basic generating mechanism of communities in complex networks. Physical Review E, 90(4), 042806.

Büttner, K., Krieter, J., Traulsen, A., & Traulsen, I. (2016). Epidemic spreading in an animal trade network–Comparison of distance-based and network-based control measures. Transboundary and Emerging Diseases, 63(1), e122-e134.

Fèvre, E. M., Bronsvoort, B. M. D. C., Hamilton, K. A., & Cleaveland, S. (2006). Animal movements and the spread of infectious diseases. Trends in microbiology, 14(3), 125-131.

Ganesh, A., Massoulié, L., & Towsley, D. (2005, March). The effect of network topology on the spread of epidemics. In Proceedings IEEE 24th Annual Joint Conference of the IEEE Computer and Communications Societies. (Vol. 2, pp. 1455-1466). IEEE.

Garin, F., Varagnolo, D., & Johansson, K. H. (2012). Distributed estimation of diameter, radius and eccentricities in anonymous networks. IFAC Proceedings Volumes, 45(26), 13-18.

Giuggioli, L., Pérez-Becker, S., & Sanders, D. P. (2013). Encounter times in overlapping domains: application to epidemic spread in a population of territorial animals. Physical review letters, 110(5), 058103.

Liu, C., & Zhang, Z. K. (2014). Information spreading on dynamic social networks. Communications in Nonlinear Science and Numerical Simulation, 19(4), 896-904.

Lou, T., Tang, J., Hopcroft, J., Fang, Z., & Ding, X. (2013). Learning to predict reciprocity and triadic closure in social networks. ACM Transactions on Knowledge Discovery from Data (TKDD), 7(2), 1-25.

Martcheva, M. (2014). Avian flu: modeling and implications for control. Journal of Biological Systems, 22(01), 151-175.

*Mili, L. (2018). Understanding spreading and evolution in complex networks.*

Moore, R. L., Isberg, S. R., Shilton, C. M., & Milic, N. L. (2017). Impact of poxvirus lesions on saltwater crocodile (Crocodylus porosus) skins. *Veterinary microbiology, 211*, 29-35.

Read, J. M., Eames, K. T., & Edmunds, W. J. (2008). Dynamic social networks and the implications for the spread of infectious disease. *Journal of the Royal Society Interface, 5*(26), 1001-1007.

Sarker S, Isberg SR, Moran JL, Araujo R, Elliott N, Melville L, Beddoe T, Helbig KJ. Crocodilepox Virus Evolutionary Genomics Supports Observed Poxvirus Infection Dynamics on Saltwater Crocodile (Crocodylus porosus). *Viruses*. 2019 Dec 2;11(12):1116. doi: 10.3390/v11121116. PMID: 31810339; PMCID: PMC6950651.

Sekara, V., Stopczynski, A., & Lehmann, S. (2016). Fundamental structures of dynamic social networks. *Proceedings of the national academy of sciences, 113*(36), 9977-9982.

Stewart, J. D., Barroso, A., Butler, R. H., & Munns, R. J. (2018). Caught at the surface. *Ecology, 99*(8), 1894-1896.

van Dam, E. R., & Kooij, R. E. (2007). The minimal spectral radius of graphs with a given diameter. *Linear Algebra and its Applications, 423*(2-3), 408-419.

Wang, Y., Cao, J., Alofi, A., Abdullah, A. M., & Elaiw, A. (2015). Revisiting node-based SIR models in complex networks with degree correlations. *Physica A: Statistical Mechanics and its Applications, 437*, 75-88.

## 第4部分：高级应用

第4部分介绍了更高级的算法来处理网络问题，包括图神经网络、顶点聚类、贝叶斯网络、Web本体语言、子图挖掘和图数据库。本部分解决的问题包括：按人口统计和网络结构因素对社交网络顶点进行聚类，理解药物滥用的演变，挖掘与学生学业成果相关的因果路径，创建基因本体，比较尼罗-撒哈拉语系的语言分类，绘制食物网，以及创建具有各种关系的电影数据库。

在第4部分中，值得关注的是第13章和第14章。第13章结合本书中的概念，构建并分析与刚果民主共和国埃博拉疫情相关的数据。第14章介绍了前沿网络科学应用，包括量子网络科学、深度学习架构的网络分析、网络的高阶结构，以及超图，重点关注医学、环境、图像和语言数据应用。

本部分包含以下章节：

- 第9章，网络机器学习
- 第10章，路径挖掘
- 第11章，映射语系——一种本体论方法
- 第12章，图数据库
- 第13章，综合应用
- 第14章，新前沿

