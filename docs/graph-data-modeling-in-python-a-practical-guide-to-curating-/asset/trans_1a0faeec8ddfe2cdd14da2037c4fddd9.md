
3. 设置好会话后，我们现在可以查询数据库了。在本例中，我们将在 Neo4j 会话中添加用于创建图关系的查询参数：

```python
query = 'CREATE (:Person {name: "Jeremy"})-[:FOLLOWS]->(:Person {name:"Mark"})'
result = session.run(query)
driver.close()
```

这些代码行不会返回任何内容，因为我们对 Neo4j 数据库执行了写操作。不过，我们可以通过打开 Neo4j 浏览器或使用 Python 中的读取查询来确认操作是否成功。对于这第一个查询，我们将同时演示这两种方法，先从浏览器开始。

4. 回到我们 Python DB 数据库的 Neo4j 浏览器，在窗口顶部的命令行中运行以下查询：

```python
MATCH (n) RETURN n
```

该查询本质上匹配数据库中的每个节点并将其返回以供查看。我们在 Python 中添加的节点和关系将显示出来：

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_113_0.png)

图 5.2 – Neo4j 浏览器结果

5. 另外，我们可以在 Python 中编写读取查询，以确认我们的写操作按预期运行。为了建立会话，我们将使用与之前的写查询相同的代码框架：

```python
driver = GraphDatabase.driver(uri='bolt://localhost:7687',
                            auth=('admin', 'testpython'))

session = driver.session()
```

6. 现在，不要向图数据库写入，而是让我们定义两个读取查询。一个查询将匹配图中的所有 Person 节点，而另一个查询将匹配所有 FOLLOWS 边：

```python
node_query = 'MATCH (n:Person) RETURN n'
edge_query = 'MATCH () -[e:FOLLOWS]->() RETURN e'
```

7. 现在，我们可以在关闭连接之前运行这两个查询。访问 run() 的 data() 方法可将结果转换为更易处理的格式，从而更容易在 Python 中处理：

```python
node_result = session.run(node_query).data()
edge_result = session.run(edge_query).data()
driver.close()
print(node_result)
print(edge_result)
```

打印结果显示，我们有两条 n 的结果，每条结果都包含我们添加的 Person 节点之一。我们还看到找到了一条 FOLLOWS 边，确认我们的写过程按预期运行。

此时，你可能会注意到，要实现 Python 与 Neo4j 的交互，我们需要经常运行这类查询。因此，我们将创建一个可重用的 Python 类来处理与数据库的连接和查询，从而无需每次运行查询时都手动连接数据库。

8. 让我们定义一个名为 Neo4jConnect 的 Python 类。该类将有一个 __init__ 方法，当 Neo4jConnect 被实例化时，该方法会被自动调用。因为我们总是希望在调用 Neo4jConnect 时连接数据库，所以我们可以在构造函数中处理驱动程序连接，使用提供给该类的 uri、user 和 password 参数。我们还将在这个类中定义一个 close() 方法来关闭连接。一个 query() 方法将完善这个类，该方法接收一个 Cypher 查询，并使用已建立的连接执行它，然后返回结果：

```python
class Neo4jConnect:
    def __init__(self, uri, user, password):
        self.driver = GraphDatabase.driver(uri, auth=(user, password))
    def close(self):
        self.driver.close()
    def query(self, query):
        session = self.driver.session()
        result = session.run(query)
        return result
```

9. 让我们通过重新运行之前执行的读取查询来确保我们的类按预期工作。现在，我们可以创建一个 Neo4jConnect 类的连接对象，该对象处理认证和驱动程序。现在可以通过访问连接对象的 query() 方法来运行查询，并且可以使用 close() 方法关闭连接：

```python
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')

node_cypher = 'MATCH (n:Person) RETURN n'
edge_cypher = 'MATCH ()-[e:FOLLOWS]->() RETURN e'

node_result = connection.query(node_cypher).data()
edge_result = connection.query(edge_cypher).data()

connection.close()
print(node_result)
print(edge_result)
```

打印 node_result 和 edge_result 显示的结果与我们之前获得的结果相同，表明我们的类实现正确。在本章的其余部分，我们将继续使用 Neo4jConnect 类来读写 Neo4j。

接下来，我们将介绍一个图数据库的实际用例，并使用 Python 和 Cypher 查询数据库并执行复杂的分析。

## 在 Neo4j 中存储图

搭建好图数据库并编写好与 Neo4j 交互的方法后，我们就可以开始使用 Python 和 Neo4j 来存储和探索图数据了。

在本节中，我们将研究一个美国与加拿大之间的航空旅行网络，并分析其属性以找到两地之间的高效航线。

### 预处理数据

首先，让我们看一下我们的数据（来源：斯坦福大学：https://snap.stanford.edu/data/reachability.html）。我们有两个文件：reachability_250.txt 和 reachability-meta.csv。

如果我们打开 reachability-meta.csv 并查看前几行，我们会看到一份关于美国和加拿大城市的信息列表：

```text
"node_id","name","metro_pop","latitude","longitude"
0,"Abbotsford, BC",133497.0,49.051575,-122.328849
1,"Aberdeen, SD",40878.0,45.45909,-98.487324
2,"Abilene, TX",166416.0,32.449175,-99.741424
3,"Akron/Canton, OH",701456.0,40.79781,-81.371567
```

该文件包含我们城市节点的节点属性，以节点 ID 为键。我们有城市名称、城市人口以及各自的纬度和经度。这个 CSV 文件可以直接加载到 Neo4j 中，因此我们不需要进行任何预处理。

然而，我们的另一个文件 reachability_250.txt 不是 CSV 格式，而 Neo4j 导入时更倾向于 CSV 格式。打开 reachability_250.txt 并查看前几行，我们可以看到其中包含一个边列表：

```text
# Directed graph: reachability.txt
# Transportation reachability of cities in the United States and Canada
# (edge from i to j if the estimated travel time from i to j is less than a threshold)
# Edges are weighted by similarity (more negative --> more dissimilar)
# Nodes: 456 Edges: 27677
# FromNodeId ToNodeId Weight
57 0 -84
113 0 -90
235 0 -170
```

根据数据源，该图是有向的。每条边表示两个城市之间的旅行，由节点 ID 表示。边的权重衡量两个城市之间的旅行时间，其中更负的值表示更远的距离。还有六行信息我们需要移除。

在接下来的步骤中，我们将开始清理和预处理这些数据，使其为图做好准备。

为了将这些信息预处理为可供 Neo4j 导入的格式，我们将其转换为 CSV 格式并移除头部行。我们还可以将距离度量改为非负值，以简化权重。所有这些预处理都可以通过 Python 的 csv 模块和一些列表推导式来处理，正如我们在前几章中所做的那样：

```python
import csv
with open('./data/reachability_250.txt', 'r') as txt:
    reader = csv.reader(txt, delimiter=' ')
    edges = [edge for edge in reader][6:]
    edges = [[edge[0], edge[1], int(edge[2])*-1] for edge in edges]

with open('./data/reachability.csv', 'w', newline='') as c:
    writer = csv.writer(c)
    for edge in edges:
        writer.writerow(edge)
```

这一次，我们的数据源已经具有合理的图格式，包含一个节点属性文件和一个边列表。该数据源还为我们提供了图模式的信息。图包含有向边，并且这些边始终连接城市，因此它是一个同构图。我们的每个节点的节点属性类型相同，因此这个航空旅行图的模式相当简单：

```text
node_id: int (unique)
name: str
population: int
latitude: float
longitude: float
```

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_117_0.png)

图 5.3 – 美国和加拿大航空旅行图模式

我们的数据已具备可供 Neo4j 导入的正确格式，航空旅行模式也已确定，接下来我们可以开始在 Neo4j 数据库中创建图。

### 将节点、边和属性移入 Neo4j

我们将使用 Python 向 Neo4j 数据库写入数据。我们需要先编写一些 Cypher 查询，然后再让 Python 将它们发送给 Neo4j。

在这个阶段，我们还应该确保 Neo4j 数据库是空的；如果不是，可以在开始之前在 Neo4j 浏览器窗口中运行 MATCH (n) DETACH DELETE n 查询。

我们的 Cypher 脚本将从 CSV 文件中读取数据，在本地安装的 Neo4j 中，CSV 文件应放在 Neo4j 可以访问的特定文件夹中。请按照以下步骤将数据导入 Neo4j：

1. 在 Neo4j 主应用程序窗口（不是当前数据库的浏览器窗口）中，单击我们新建数据库旁边的 ... 图标。
2. 然后，选择“打开文件夹”再选择“导入”，以打开操作系统文件浏览器中的 /import 文件夹。
3. 将 reachability.csv 和 reachability-meta.csv 复制到该导入文件夹中。

