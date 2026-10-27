

```
tv_degree = list(zip(g.vs[tv_indices]['page_name'], g.degree(tv_indices)))
sorted_tv_degree = sorted(tv_degree, key=lambda x: x[1], reverse=True)
print(sorted_tv_degree[:10])
```

这告诉我们，在我们的图中，度最高的是 Today Show，拥有 141 个相互点赞。

## 使用 select() 来 interrogate 图

在上一节中，我们使用了 vs.select()，值得更详细地讨论，因为它是一个特别强大的方法。其强大之处在于其灵活的参数，这些参数由节点类型或属性以及比较运算符组成。

在之前的示例中，我们使用了 page_type_eq 参数，设置为 tvshow。这告诉 vs.select() 搜索具有 page_type 属性等于（由 _eq 指定）tvshow 的节点。这里，_eq 可以替换为许多其他运算符，如下所示：

| Select 参数元素 | 比较运算符 |
|---|---|
| _eq | 等于 |
| _ne | 不等于 |
| _lt | 小于 |
| _gt | 大于 |
| _le | 小于或等于 |
| _ge | 大于或等于 |
| _in | 在给定列表中 |
| _not in | 不在给定列表中 |

表 2.1 – select() 的 Dunder 表达式

因此，使用上表，如果我们想找到类型为 government 或 politician 的节点，我们可以使用以下代码：

```
gov_pol_nodes = g.vs.select(page_type_in=['government', 'politician'])
```

在此基础上，在 select() 方法内，在 Graph() 方法前加上下划线，可以在运行时计算节点属性。例如，我们可以使用以下代码查找度在 100 到 140 之间的所有 TV 节点：

```
one_to_three_100_nodes = g.vs.select(_degree_ge=100,
                            _degree_le=140,
                            page_type_eq='tvshow')
print(one_to_three_100_nodes['page_name'])
```

这里，select() 参数被分层以返回更具体的仅满足我们条件的四个节点集合。我们还通过在 degree 关键字前加上下划线在 select() 方法中使用了 degree。例如，要获取度大于 100，我们使用了 magic 方法 _degree_ge=100；这充当了下限，上限指定为 _degree_le=140。

值得注意的是，使用 select() 在运行时计算节点和图特征最适合于度等简单计算。对于一些计算要求更高的网络科学计算，最好将计算的节点属性作为节点属性添加，而不是像前一个 degree 示例中那样计算两次。

与度相比，度计算每个节点的边，一些算法涉及更复杂的例程——例如，在图中沿路径行走的算法。涉及沿路径行走的算法通常涉及更多操作，因此计算时间比简单的节点度计算更长。还有更复杂的算法可供使用，我们将在本书中探讨其中的一些。

为了说明，之前的示例可以重写如下，以便节点度仅计算一次，写入图节点作为 node_degree_property，然后由 select() 查询，而不是为每次比较计算度：

```
g.vs['node_degree'] = g.degree()
one_to_three_100_nodes = g.vs.select(node_degree_ge=100,
                            node_degree_le=140,
                            page_type_eq='tvshow')
print(one_to_three_100_nodes['page_name'])
```

执行这些类型的 select() 操作的最有效方式将因用例而异，但上述考虑可以帮助设计最佳流程。

接下来，我们将看看如何以类似的方式对我们流行的节点使用这些操作。

## 我们流行节点的属性

了解了更多关于 select() 方法的知识后，我们现在可以深入探索我们最受欢迎的 TV 节点属性，并了解其在相互点赞 Facebook 图中的重要性。

我们之前发现，按相互点赞计算，最受欢迎的 TV 节点是 Today Show，其度中心性为 141。我们可以通过检查其邻居来了解更多关于我们最高度中心性节点的信息。在无向网络中，例如我们的 Facebook 相互点赞图，通过边连接的节点称为关联节点。找到 Today Show 的所有关联节点将揭示在我们的数据集中哪些 Facebook 页面彼此共享相互兴趣。

- 1. 为此，我们将再次使用 select() 方法，这次用于边。es.select() 方法可以根据其属性（如果有）查找边，使用与表 2.1 相同的语法。然而，我们的边不包含任何属性，我们更感兴趣的是它们链接的节点。

与 vs.select() 方法一样，es.select() 有一些特殊参数可用于查找链接具有特定条件的节点的边——同样，每个参数都以下划线开头。

下表显示了与 vs.select() 命令一起使用的各种 dunder 方法，也称为 magic 方法。可以通过这些方法访问执行函数列中列出的功能：

| 边 select 参数前缀 | 功能 |
|---|---|
| _incident | 查找与特定节点或节点列表关联的边，忽略边方向。 |
| _source (或 _from) | 在有向图中，查找节点或节点列表为边源的边。 |
| _target (或 _to) | 在有向图中，查找节点或节点列表为边源的边。 |
| _within | 查找边中构成边的两个节点都包含在给定列表中的边。 |
| _between | 查找边中构成边的两个节点包含在单独的列表中的边（这两个列表作为元组提供）。 |

表 2.2 – es.select() 特殊参数

- 2. 现在，由于我们的相互点赞图是无向的，让我们使用 _incident 参数在 es.select() 中选择与 Today Show 关联的边。es.select() 使用节点 ID，因此我们需要先找到我们感兴趣的 TV 节点的节点 ID：

```
today_show_id = g.vs.select(page_name_eq='Today Show')[0].index
print(today_show_id)
```

这里，类似于本章早些时候，我们使用 vs.select() 查找 page name 等于 Today Show 的节点。因为 vs.select() 返回一个可迭代对象，我们需要使用 [0] 取第一个元素，以便访问第一个元素的 .index 属性。我们的 print 语句将显示我们的节点 ID 为 909。

3. 现在，我们可以使用 es.select() 找到包含节点 909 的边。我们指定 _incident 参数等于我们的 today_show_id 变量，并将其放在列表中（注意 es.select() 要求表 2.2 中的所有特殊参数的节点列表）：

```
today_show_edges = g.es.select(_incident=[today_show_id])
```

4. 就像 vs.select() 一样，这一行返回一个包含边的可迭代对象。我们可以使用可迭代对象中每个边的 .source 和 .target 属性提取 today_show_edges 中每条边的两个节点：

```
sources = [edge.source for edge in today_show_edges]
targets = [edge.target for edge in today_show_edges]
print(sources)
print(targets)
```

这将向我们展示两个节点 ID 列表，都存在于与 Today Show 关联的边中。

注意
注意，在我们的相互点赞图中，source 和 target 在现实中是等价的，因为图是无向的。这里，source 和 target 仅由 igraph 为每条边表示，基于之前使用 add_edges() 添加的每个两元素节点对的索引。

5. 注意，节点 909，即 Today Show，在我们的列表中出现了多次。这是可以预期的，因为我们要求 igraph 返回包含它的所有边。然而，对于我们检查 Today Show 的邻居节点的目的，我们需要从结果中删除它。我们可以使用 set() 从 sources 和 targets 列表的连接中返回唯一元素集合，然后删除节点 909，对应 Today Show：

```
neighbor_nodes = list(set(sources + targets))
neighbor_nodes.remove(909)
print(neighbor_nodes)
print(len(neighbor_nodes))
```

我们的第一个 print() 语句现在应该显示一个唯一的节点 ID 列表。我们还可以打印 neighbor_nodes 列表元素的唯一数量，应为 141。请注意，这等于之前计算的 Today Show 的度中心性。

