

11. 回到 Python，现在我们准备在 igraph 中进行更复杂的分析，找出从圣地亚哥到圣约翰斯的最短出行方式。在建立好图之后，用代码实现其实相当简单。我们可以先用 `g.vs.select()` 指定要查找路径的节点，并将其命名为 source 和 target。要运行 Dijkstra 算法，我们可以在源城市和目标城市之间调用 igraph 的 `get_shortest_paths()` 方法。只要将权重参数指定为 `travel_time`，igraph 就会在路径计算中将出行时间作为边的权重，并返回最小距离路径：

```
source = g.vs.select(name_eq='San Diego, CA')
target = g.vs.select(name_eq='St. Johns, NL')
shortest_path = g.get_shortest_paths(source[0], target[0], weights='travel_time')
```

12. `get_shortest_paths()` 的结果将包含 igraph 的整数 ID，因此让我们获取路径中这些节点对应的每个城市的名称并查看：

```
shortest_path = [g.vs[node]['name'] for node in shortest_path]
print(shortest_path)
```

13. 我们可以看到，在出行时间方面最优的路径是经由芝加哥，然后到蒙特利尔。要获取每一步的距离，我们可以在 `get_shortest_paths()` 中指定一个可选参数 `output='epath'`，它会返回边的 ID。然后，我们可以用列表推导式访问这些边 ID 关联的出行时间并查看。我们还可以进行求和以找出总出行时间：

```
short_path_rels = g.get_shortest_paths(source[0], target[0], weights='travel_time', output='epath')
short_path_distances = [g.es[edge]['travel_time'] for edge in short_path_rels]
print(short_path_distances)
shortest_travel_time = sum(short_path_distances[0])
print(shortest_travel_time)
```

因此，我们可以使用这种方法在我们的数据集中找到城市之间的最优出行路线。然而，如果我们想查看边的其他特征来推荐城市之间的特定路径呢？例如，节点之间的物理距离可能是个关注点。同样，计算这个更适合用 Python 而不是图数据库，所以让我们看看如何实现。

14. 我们可以首先通过访问 `g.es` 和 `g.vs` 并执行推导式来从 igraph 图中提取边以及纬度和经度。我们将使用节点 ID 将节点与对应的边匹配：

```
edges = [[g.vs[edge.source]['node_id'], g.vs[edge.target]['node_id']] for edge in g.es]
latitudes = {node['node_id']: node['latitude'] for node in g.vs}
longitudes = {node['node_id']: node['longitude'] for node in g.vs}
```

15. 接下来，我们需要一个函数根据纬度和经度计算距离。让我们定义一个函数 `find_distances()`，它将接收一个边列表以及两个字典 `latitudes` 和 `longitudes`，每个字典以节点 ID 为键。我们可以使用 geopy 模块导入 `geodesic()`，它会计算两个经纬度坐标点之间的距离。对于每条边，我们为 source 和 target 分别构造一个包含纬度和经度的元组，即 `loc_1` 和 `loc_2`。我们可以使用 `geodesic()` 并将这些点作为参数来计算距离，然后通过访问 `km` 属性将距离转换为千米。每对连接节点之间的距离被追加到一个列表中并返回：

```
from geopy.distance import geodesic
def find_distances(edges, latitudes, longitudes):
    distances = []
    for n, m in edges:
        loc_1 = (latitudes[n], longitudes[n])
        loc_2 = (latitudes[m], longitudes[m])
        distance = geodesic(loc_1, loc_2).km
        distances.append(int(distance))
    return distances
```

16. 现在，我们可以在准备好的边、纬度和经度上调用我们的方法，并使用 `g.es` 将距离应用到图中的每条边：

```
distances = find_distances(edges, latitudes, longitudes)
g.es['distance'] = distances
```

17. 将距离添加到 igraph 后，我们可以使用与之前相同的过程，找到圣地亚哥和圣约翰斯之间在物理距离方面的最短路径：

```
source = g.vs.select(name_eq='San Diego, CA')
target = g.vs.select(name_eq='St. Johns, NL')
shortest_path = g.get_shortest_paths(source[0],
    target[0], weights='distance')
shortest_path = [g.vs[node]['name'] for node in shortest_path]
print(shortest_path)
short_path_rels = g.get_shortest_paths(source[0],
    target[0], weights='distance', output='epath')
short_path_distances = [g.es[edge]['distance'] for edge in short_path_rels]
print(short_path_distances)
shortest_travel_time = sum(short_path_distances[0])
print(shortest_travel_time)
```

这给了我们一个不同的答案，路径是经过芝加哥再到多伦多。

在旅行推荐系统中，客户可能对其拟议旅行路线的其他特征感兴趣。在物理距离受关注的场景中，我们可能希望预先计算这些距离并将它们添加到服务于 Web 应用的图数据库中。

这样，每次进行推荐时就不必重新计算它们。我们可以为美国和加拿大的航空旅行图做到这一点，方法是先用一些简单的 Cypher 从 Neo4j 数据库中提取所有边、节点、纬度和经度，然后对这些查询运行 Neo4j：

```
edges_cypher = 'MATCH (n)-[:AIR_TRAVEL]->(m) \
' 'RETURN n.node_id, m.node_id'
nodes_cypher = 'MATCH (n) \
' 'RETURN n.node_id, n.latitude, n.longitude'
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
edges = connection.query(edges_cypher).data()
lat_longs = connection.query(nodes_cypher).data()
connection.close()
```

18. 然后，我们可以使用类似于子图示例的方法来计算所有边的物理距离，并重用我们自定义的 `find_distances()` 方法。这些可以写入 CSV 文件，准备导入 Neo4j：

```
edges = [[edge['n.node_id'], edge['m.node_id']] for edge in edges]
latitudes = {node['n.node_id']: node['n.latitude'] for node in lat_longs}
longitudes = {node['n.node_id']: node['n.longitude'] for node in lat_longs}
distances = find_distances(edges, latitudes, longitudes)
distances = list(zip(edges, distances))
import csv
with open('chapter05/data/distances.csv', 'w', newline='') as c:
    writer = csv.writer(c)
    for edge in distances:
        writer.writerow(edge)
```

19. 现在，就像我们之前首次在 Neo4j 中组装图时那样，我们可以将生成的 `distances.csv` 文件移动到 Neo4j 的导入文件夹中。使用我们在本章中学到的知识，一些简单的 Cypher 就能将所有新的距离边属性导入 Neo4j。这里唯一的新函数是 `SET`，它为指定的节点或边更新给定的特征：

```
LOAD CSV from 'file:///distances.csv' as row
MATCH (n:City {node_id: toInteger(row[0])})
MATCH (m:City {node_id: toInteger(row[1])})
MATCH (n)-[r:AIR_TRAVEL]->(m)
SET r.distance = toInteger(row[2])
```

现在，让我们进入下一节！

## 进入摄取管道

在本章中，我们创建了一个静态的 Neo4j 图，对其进行了查询和分析，并更新了其特征。这类解决方案可能会在生产环境中使用，用于在应用程序中提供结果。

然而，这类流程中还有一些我们尚未涵盖的元素。在生产系统中，节点和边可能会定期以小块的形式读写到图数据库中。复杂处理可能会在数据摄取之前使用 Python 或其他语言在 Neo4j 之外进行。想一想真正的旅行优化或推荐服务实际会提供多少选项——每一个都必须嵌入到图数据库管道中。

在第 6 章“管道开发”中，我们将看一个复杂图数据管道的示例，并探讨如何使其可靠且高效。

## 总结

我们在本章中涵盖了大量内容，现在你应该对内存数据库（如 Neo4j）的强大之处有了很好的理解。我们开始求知之旅时，先了解了如何设置 Neo4j 实例，在那里我们完成了所有配置，安装了本地版本的 Neo4j 并快速启动运行。

接下来，我们了解了如何使用一种名为 Cypher 的查询语言对 Neo4j 数据库执行查询。对你们中的一些人来说，这可能是第一次接触 Cypher，我们向你介绍了这种语言如何与其他查询语言（如 SQL）相似。为了更好地理解如何使用 Cypher，Neo4j 网站（https://neo4j.com/developer/cypher/）是一个很好的资源。

更进一步，我们开始研究如何结合使用 Cypher 和 Python 的 cypher 包在 Neo4j 图数据库中存储数据，从而开始与图数据库进行交互。

最终，我们探索了一个用例：推荐最佳的出行路线，并分析两点之间的最优路径。在这里，我们介绍了 Dijkstra 算法（https://isaaccomputerscience.org/concepts/dsa_search_dijkstra?examBoard=all&stage=all），并用它来计算两点之间的最短路径。这模拟了你日常使用的管道和应用程序，例如 Google 地图或 Uber 的路线规划系统。在这里，你亲手实践了 Python、Cypher、igraph 和 Neo4j。可以说，为了在高潮中结束，我们接着开始解释在下一章中我们将如何扩展摄取管道，并将拓展本章中提供的示例。

自从第一章以来，你已经取得了长足的进步，所以恭喜你走到这里。我们期待在下一章中带你学习如何创建可用于生产的图管道，这将是下一章的重点。

