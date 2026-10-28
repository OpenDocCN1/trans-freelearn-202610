

![](img/71ff737206f7d80bb625b3c9aca9dc7f_94_0.png)

顶点八和顶点九之间的最短路径就是连接顶点八到顶点九的边。假设该网络未加权，则该路径的最短距离为一。顶点一和顶点六之间的最短路径由连接顶点六到顶点三以及顶点三到顶点一的边给出。因此，顶点六和三之间的最短距离为二。然而，顶点八和顶点六之间的最短路径不存在，将被指定为无穷大距离。

Dijkstra 算法从一对顶点中的一个开始，并探索与该顶点相连的顶点。然后记录该初始顶点与其邻居顶点之间的最短路径，减去初始顶点本身（因为它已在我们的路径集合中）。然后探索与最短距离邻居顶点相连的下一组顶点，以找到存在的下一个最短路径。这种迭代探索会一直持续，直到找到另一对顶点。

让我们考虑 Dijkstra 算法如何帮助我们找到商店之间的最短路线。考虑郊区同一家连锁超市的五家商店，可能需要由母公司供应商补货。*表 4.1* 给出了这五家商店之间距离的汇总，单位为英里：

| | Store 1 | Store 2 | Store 3 | Store 4 | Store 5 |
|---|---|---|---|---|---|
| Store 1 | 0 | 2 | 2.4 | 3 | 3 |
| Store 2 | 2 | 0 | 3.7 | 1.4 | 4.3 |
| Store 3 | 2.4 | 3.7 | 0 | 4.9 | 0.9 |
| Store 4 | 3 | 1.4 | 4.9 | 0 | 5.4 |
| Store 5 | 3 | 4.3 | 0.9 | 5.4 | 0 |

表 4.1 – 郊区一家连锁商店之间的英里距离表

我们可以通过脚本 4.1 在 NetworkX 中创建这个商店网络：

```
#import packages
import networkx as nx

#create the Stores network
STRS = nx.Graph()
STRS.add_nodes_from(["store1","store2","store3","store4","store5"])

#define weighted ebunches, which represent lists of edges
e1 = [["store1","store2",2),(("store1","store3",2.4),
(("store1","store4",3),("store1","store5",3)]
e2 = [["store2","store3",3.7),(("store2","store4",1.4),
(("store2","store5",4.3)]
e3 = [["store3","store4",4.9),(("store3","store5",0.9),
(("store4","store5",5.4)]

#add edges
STRS.add_weighted_edges_from(e1)
STRS.add_weighted_edges_from(e2)
STRS.add_weighted_edges_from(e3)

#plot
weight_labels=nx.get_edge_attributes(STRS,'weight')
pos = nx.spring_layout(STRS)
nx.draw(STRS,pos,with_labels=True,)
nx.draw_networkx_edge_labels(STRS, pos,edge_labels=weight_labels)
```

脚本 4.1 为我们提供了五家商店的网络，并附有漂亮的绘图摘要，显示商店之间的距离，如图 4.9 所示：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_96_0.png)

图 4.9 – 我们五家商店及其之间距离的绘图

现在我们可以在脚本 4.1 的基础上创建基于网络的邻接矩阵并找到位置之间的最短路径。让我们计算商店 1 与我们连锁店其他各店之间的最短距离：

```
#create adjacency matrix
adj1 = nx.adjacency_matrix(STRS)
adj1.todense()

#find all shortest paths starting at Store 1
length1, path1 = nx.single_source_dijkstra(STRS,"store1")
length1
```

通过此计算，我们得出了商店 1 与所有其他商店之间的最短距离（length1）以及一般的最短路线（path1）。我们可以看到商店 1 与其他商店之间的最短距离来自 length1。前往商店 2 的最短路线为两英里，而前往商店 4 或商店 5 的最短路线为三英里。商店 3 距离商店 1 为 2.4 英里。

使用 Dijkstra 算法的主要缺点之一是它是一种盲目搜索算法，需要遍历所有可能的选择才能找到路径上每对可能顶点之间的最短路径。对于大型网络来说，这需要很长时间，实际上需要对 Dijkstra 算法进行修改以减少搜索时间。

**A* 算法**（发音为 A-star）通过使用启发式函数来指导搜索来修改 Dijkstra 算法（与 Dijkstra 算法中访问每个顶点不同）。启发式函数在搜索任务中很常见，因为它们加快了过程，并且可以在算法失败时修改搜索。A* 算法中使用的启发式函数结合了从起始顶点到路径的成本以及到达最终顶点的剩余成本的估计，以优先考虑最接近最终顶点的顶点。A* 算法的主要用途之一是为 GPS 用户提供方向以找到前往目的地的最佳路线。

总体而言，A* 算法能有效地找到最短路径候选，特别是在大型或密集的网络中。然而，其性能取决于用于估计遍历网络到达给定目的地的成本的启发式方法。虽然 Dijkstra 算法保证能找到最短路径，但 A* 算法不能保证找到最短路径。然而，在大型密集网络上，运行 Dijkstra 算法是不切实际的，因此必须使用 A*。

*表 4.2* 总结了 Dijkstra 算法和 A* 算法之间的差异，以指导您在最短路径计算中如何以及何时使用每种算法：

| Shortest path algorithm | Type of search | Result guarantees | Computational speed |
|---|---|---|---|
| Dijkstra’s algorithm | Blind search of all vertices | Always finds the shortest path | Slow enough to limit use on dense or large networks |
| A* algorithm | Heuristic function guiding search | No guarantees to finding the shortest path | Fast enough for most problems |

表 4.2 – Dijkstra 算法与 A* 算法的差异

让我们考虑一个不同的商店网络，其中并非每家商店之间都存在路线，通过 `Script 4.2` 在 NetworkX 中创建一个二级商店网络：

```
```python
#create a different Stores network
STRS2 = nx.Graph()
STRS2.add_nodes_from(["store1","store2","store3","store4","store5"])

#define weighted ebunch
e12 = [("store1","store2",2),("store1","store4",3)]
e22 = [("store2","store3",3.7),("store2","store4",1.4)]
```
```

让我们考虑从商店 2 到商店 5 的路线。存在几种可能的路线（商店 2 到商店 3 到商店 4 到商店 5，商店 2 到商店 1 到商店 4 到商店 5，以及商店 2 到商店 4 到商店 5）。如果所有距离都相对较短，最短的可能路线可能不是停靠次数最少的路线。在这种情况下，我们只需查看我们的网络即可发现商店 2 到商店 4 到商店 5 是最短路线。然而，在非常大的网络中，这将花费大量时间，并且可能涉及更长的路径。

我们可以通过在脚本 4.2 中添加以下内容，使用 NetworkX 中的 A* 算法找到最短路线：

```
#find shortest path from Store 2 to Store 5
nx.astar_path(STRS2,"store2","store5", weight='weight')
```

正如预期的那样，A* 算法找到了从商店 2 到商店 4 到商店 5 的最短路径。同样，对于更大的网络，我们需要像 A* 算法这样的算法来快速找到最短路线。即使是 20 家商店且商店之间有若干连接的网络，从网络可视化中手工计算也会有问题。我们已经看到算法如何帮助我们找到两个特定目的地之间的最短路径。现在让我们来看看包含多个不同顶点停靠的最短路径。这种类型的解决方案对于许多最优路由问题至关重要，并且建立在我们寻找两个不同目的地之间最短路径的问题之上。

## 旅行商问题

最短路径的自然扩展是停靠每个地点的最短可能路线。例如，考虑一辆需要为我们五家商店全部补货的蔬菜卡车。停靠我们五家商店的最短路线可以为司机节省时间和燃料，并使蔬菜在最短时间内到达每家商店。

旅行商问题旨在找到停靠每个地点的最短路线，或停靠任意数量可能地点的最短路线。在图论中，该问题（以及欧拉问题）与图的循环有关，循环定义为从同一顶点开始和结束的非空路径。

在实践中，需要算法来找到最短路径，对于大型问题，计算时间和收敛到解决方案可能会限制大多数算法的使用。NetworkX 提供了 Christofides 算法作为求解器，它找到最短生成树（结构类似于树且无环的网络），然后匹配树的顶点以找到最小距离。

让我们使用脚本 4.3 为我们原始的五家商店网络找到旅行商问题的解决方案：

```
#define traveling salesman algorithm and apply it to the stores's
#networks
tsp = nx.approximation.traveling_salesman_problem
tsp(STRS)
```

