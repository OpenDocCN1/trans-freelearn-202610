
通过我们的`print()`语句，我们可以查看一些已找到的路径。`paths`变量包含从原始用户节点遍历的节点的igraph ID，以列表的列表形式存储。

> **注意**
> 有些列表的长度不是4，而这正是我们做出推荐所需的长度。推荐用户已购买的游戏没有意义，比如长度为2的路径；也没有理由向我们的指定用户推荐其他用户，比如长度为3的路径。

- 2. 因此，我们需要从`paths`列表中移除长度小于4的路径。我们只需要这些列表中的第四个元素，它对应一个可以作为推荐依据的游戏。我们可以使用带条件运算符的列表推导式来满足这两个要求，确保所有路径长度均为4：

```
rec_game_ids = [path[3] for path in paths if len(path) == 4]
```

- 3. 现在，我们得到了表示游戏的节点的igraph ID列表。我们需要这些游戏的名称，因此我们将使用另一个列表推导式，通过`g.vs`访问每个节点的`steam_id`属性：

```
game_names = [g.vs[game_id]['steam_id'] for game_id in rec_game_ids]
```

- 4. 前面的代码会给我们一个游戏名称列表。然而，这个列表包含我们的指定用户已经玩过或购买过的游戏，因此我们不想推荐它们。我们可以先使用igraph中的`neighbors()`方法找到所选用户的邻居节点，从而移除这些游戏。然后，我们可以使用与之前代码片段类似的列表推导式，通过`g.vs`查找`steam_id`值，找到对应的游戏名称：

```
neighbors = g.neighbors(user_id_ex)
purchased_games = [g.vs[node_id]['steam_id'] for node_id in g.neighbors(user_id_ex)]
```

- 5. 让我们用另一个列表推导式移除用户已玩的游戏，比较这些列表：

```
game_names = [game for game in game_names if game not in purchased_games]
```

- 6. 与之前使用MySQL实现相同功能的示例一样，让我们使用Python的`collections`库和`Counter()`来查看路径末端出现频率最高的游戏：

```
from collections import Counter
game_frequency = Counter(game_names)
print(game_frequency)
```

查看打印出的`Counter()`对象开头，我们可以看到可能推荐给用户的前三款游戏是《反恐精英：全球攻势》、《未转变者》和《传送门2》。

在本节中，我们使用图对数据进行了基于路径的分析。当涉及路径时，图是表示和查询实体之间关系的理想数据模型。我们研究了使用Steam数据图向用户推荐游戏的简单方法，但现实世界中的推荐引擎通常使用更复杂的方法。

接下来，我们将了解图数据模型如何帮助我们实现一种更智能的推荐方法。

## 我们的推荐系统

现在我们已经将数据放入Python图中，让我们更进一步，设计一个更健壮的推荐流程，这类流程通常使用图数据完成。

让我们以解决方案工程师或数据科学家的身份，基于Steam数据编写一个游戏推荐系统。推荐系统大量用于面向客户的应用中，向用户展示他们可能感兴趣的产品。产品推荐通常基于行为相似的用户玩过和购买过的内容。

## 通用的MySQL到igraph方法

这次，我们将编写一组可复用的通用方法，从MySQL表中的列创建igraph图。这些函数将设计为在给定一组列名的情况下，创建一个异构、二分、有向图。

让我们从编写主函数`mysql_to_graph()`开始。该方法需要接受一个MySQL表名、该表的源列名和目标列名，以及一个边权重列。在本例中，我们将使用`steam_play`表，其中权重是每个游戏（目标）被用户（源）游玩的小时数。我们将按步骤来组织这些方法，以便更容易实现：

- 1. 在我们的新方法中，我们可以使用f字符串和替换来生成符合所需搜索条件的SQL查询。`SELECT`后面的列名以及`FROM`要选择的表，现在可以通过`mysql_to_graph`接受的参数动态设置。然后，我们可以使用在*使用MySQL查询SQL*一节的第4步中定义的方法`query_mysql()`，从MySQL数据库获取一个列表的列表：

```
def mysql_to_graph(table, source, target, weights, password):
    import igraph as ig

    sql_query = f'SELECT {source}, {target}, {weights} FROM {table}'

    data = query_mysql(sql_query, password=PASSWORD)
```

- 2. 现在我们有了数据，我们可以使用一系列列表推导式将数据拆分为源节点和目标节点。这有助于我们以合理的格式获取数据，以便分配igraph ID。在编写完主方法`mysql_to_graph()`之后，我们将编写一个单独的方法`create_igraph_ids()`，用于创建{Steam ID: igraph ID}对的字典：

```
    source_nodes = sorted(list(set([source for source, _, _ in data])))
    target_nodes = sorted(list(set([target for _, target, _ in data])))

    source_igraph_ids = create_igraph_ids(source_nodes)
    target_igraph_ids = create_igraph_ids(target_nodes, len(source_igraph_ids))
```

- 3. 然后，我们需要使用所创建字典`source_igraph_ids`和`target_igraph_ids`中的键值对生成igraph边列表。我们可以使用另一个列表推导式，以source和target作为键来做到这一点。最后，在创建图之前，我们必须从MySQL返回的列表的列表中提取边权重，以备稍后添加到图的边中：

```
    edges = [(source_igraph_ids[source], target_igraph_ids[target])
        for source, target, _ in data]
    weights = [weight for _, _, weight in data]
```

- 4. 现在，所有数据都已准备好以正确的格式添加到igraph图中。首先，我们必须使用`ig.Graph()`创建一个空的有向`Graph()`对象。然后，我们必须使用`g.add_vertices()`添加节点，节点数等于我们的两个节点列表的长度之和。“内部ID”——在本例中即Steam ID——然后通过访问图的`g.vs`属性，使用`source_igraph_ids`和`target_igraph_ids`字典键添加到节点上。节点类型同样通过`g.vs`添加，方法是创建类型字符串列表source和target，就Steam数据而言，它们分别对应用户和游戏。最后，可以从包含源和目标igraph ID的`edges`列表中添加边，并通过访问图的`g.es`属性按列表添加边权重和游玩小时数。然后返回我们的图`g`，以便在单独的函数中进行分析：

```
    g = ig.Graph(directed=True)
    g.add_vertices(len(source_nodes + target_nodes))
    g.vs['internal_id'] = list(source_igraph_ids.keys()) + list(target_igraph_ids.keys())
    g.vs['type'] = ['source' for _ in source_nodes] + ['target' for _ in target_nodes]
    g.add_edges(edges)
    g.es['weight'] = weights

    return g
```

- 5. 在我们前面定义的`mysql_to_graph`方法中，我们调用了一个单独的用户自定义函数`create_igraph_ids()`，我们需要单独定义它。我们可以像本章前面所做的那样为每个节点创建igraph ID——即使用字典推导式并利用Python的`enumerate()`方法。然后，我们的`create_igraph_ids()`方法需要接受的参数只是节点名称列表和`enumerate()`的起始索引：

```
def create_igraph_ids(nodes, from_index=0):
    igraph_ids = {internal_id: igraph_id for igraph_id, internal_id
                  in enumerate(nodes, from_index)}
    return igraph_ids
```

我们现在有了一个可复用的通用方法，可以从MySQL数据创建图，描述任意具有源、目标以及该关系权重的关系。回到我们的Steam用例，在我们的图中，有用户、他们玩过的游戏，以及他们花费在这些游戏上的时间。这些信息足以让我们就现有用户接下来可能想购买哪些游戏提出建议。

在本章前面，我们根据用户是否玩过与指定用户相同的游戏，以及他们玩过哪些游戏，为一个新游戏做了一个非常简单的推荐。然而，这只是推荐系统的一个非常简单的例子。使用我们的图，我们可以实现一个更健壮的系统，该系统可以产品化，将用户ID作为输入，并使用更复杂的方法（即Jaccard相似度）推荐游戏。使用igraph还可以应用其他相似度方法，例如Dice相似度和逆对数加权相似度，它们也是比较节点组的有效方法。选择Jaccard相似度是因为它简单且总体性能良好，但请随意尝试不同的相似度方法。
