

我们最终得到的节点名称属性包含在字典中，接下来将这些属性转换为共同出演演员的两两列表：

```
def get_co_stars_neo4j(connection):
    query = 'MATCH (act1:Entity)<-[:STARRING]-(film:Entity)'  \
            '-[:STARRING]->(act2:Entity)  '  \
            'RETURN act1, act2'

    result = connection.query(query).data()
    result = [[act['act1']['name'], act['act2']['name']] for act in result]

    return result
```

5. 现在，我们可以使用包含凭据的 Neo4jConnect 类创建 Neo4j 连接，调用 `get_co_stars_neo4j()` 获取共同出演演员的配对，之后记得关闭数据库连接。让我们打印前几对数据，确保一切符合预期：

```
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
co_stars = get_co_stars_neo4j(connection)
connection.close()
print(co_stars[:5])
```

6. 有了包含演员配对的列表的列表，我们实际上就拥有了 Python 中的边列表，可以用它创建图。正如前面章节所介绍的，我们还需要节点列表和属性列表才能在 igraph 中组装图。

7. 我们可以使用双重列表推导式获取边列表中的所有节点，再对该列表取集合，从而得到唯一节点列表：

```
nodes = list(set([node for edge in co_stars for node in edge]))
```

8. 此外，我们可以通过创建字典为这些节点分配递增的整数 igraph ID。使用字典推导式，在唯一节点列表上调用 `enumerate()`，为每个节点创建映射后的递增 ID：

```
igraph_ids = {film:node_id for node_id, film in enumerate(nodes)}
```

9. 在 igraph 中，我们需要使用这些新 ID 向图添加边，因此必须将每个节点名称替换为 `igraph_ids` 中对应的唯一 igraph ID。我们可以再次使用推导式完成此项任务，得到一个对应于我们影片的整数对边列表：

```
edgelist = [[igraph_ids[n], igraph_ids[m]] for n, m in co_stars]
```

10. 现在我们已经拥有了组装图所需的一切：

    - I. 首先导入 *igraph*，并使用 `igraph.Graph()` 创建一个空图。

    - II. 接下来，使用 `add_vertices()` 向图添加节点，数量等于我们唯一 *igraph* 节点 ID 的数量。

    - III. 然后，通过图对象 `g` 的 `add_edges()` 方法，并将 ID 边列表作为参数传入，来连接这些节点。

    - IV. 最后，通过访问 `g.vs`，我们可以将演员属性按列表方式添加到每个节点，这些属性就是唯一节点列表中每位演员的名称：

```
import igraph
g = igraph.Graph()
g.add_vertices(len(igraph_ids))
g.add_edges(edgelist)
g.vs['actor'] = nodes
```

11. 作为最后一步，我们应该确保图的元素符合预期。我们可以使用 Python 中的 `assert` 语句来确保节点属性的命名正确，将其与节点演员名称列表进行比较。我们还可以确保节点数量符合预期：

```
assert g.vs[2]['actor'] == nodes[2]
assert len(g.vs) == len(nodes)
```

12. 为了最终确认我们的投影已按预期应用，让我们在 *igraph* 图中找到一条边，并在 Neo4j 中存储的原始图数据库中找到对应的路径。首先，为找到 *igraph* 图中一条边上的节点，我们可以访问图的 `g.es` 属性，并通过依次访问 `source` 和 `target` 属性，找到索引为 0 的边所连接节点的 ID。然后，使用这些 *igraph* 节点 ID，将该索引传入 `g.vs`，找到我们将在 Neo4j 中查找对应路径所用的演员属性，并将它们赋值给 `actor1` 和 `actor2` 变量：

```
actor1 = g.vs[g.es[0].source]['actor']
actor2 = g.vs[g.es[0].target]['actor']
```

13. 现在，我们可以使用 Neo4jConnect 打开一个到 Neo4j 的连接，并将这些演员名称插入 Cypher 查询中。我们可以使用 f-strings 构造有效的 Cypher 查询，查找我们最初查询 Neo4j 以创建投影时的 Entity | Film | Entity 模式。返回演员和影片后，使用 `.data()` 创建结果字典，然后使用 `.close()` 关闭连接，并将其打印到控制台：

```
connection = Neo4jConnect('bolt://localhost:7687', 'admin',
'testpython')
query = f'MATCH (act1:Entity {{name:"{actor1}"}}) \
          <-[:STARRING]-(film:Entity)' \
        f'-[:STARRING]->(act2:Entity {{name:"{actor2}"}}) ' \
        'RETURN act1, film, act2'
result = connection.query(query).data()
connection.close()
print(result)
```

这里的 print 语句应显示原始图中的有效路径，与我们在 igraph 中投影的边相对应，从而确认投影已正确创建：

```
[{'act1': {'name': 'Roger Pryor (actor)'}, 'film': {'name': 'The Man with Nine Lives (film)'}, 'act2': {'name': 'Boris Karloff'}}]
```

至此，我们从 Neo4j 中原始、复杂的知识图谱中提取了共同出演演员的投影，并将其设置在 igraph 中，以便进行进一步分析。我们将在最后一节回到这个投影，进一步探索这些数据。现在，我们将回到 Neo4j 浏览器，介绍另一种生成投影的方法，以备需要更持久地存储投影时使用。

## 在 Neo4j 中创建投影

虽然 Python 中的投影支持一些高度高级的数据分析，但它存储在内存中，因此不是持久的。如果我们想要一个可重复访问或需要长期存储的投影，我们可能也希望使用像 Neo4j 这样的图数据库。要构建我们的 Neo4j 投影，我们需要执行以下步骤：

1. 在 Projection DB 中，接下来我们将创建一个投影，将影片按照共享演员及其拍摄国家连接起来，以便进行更多分析。就本章而言，我们可以将这个投影与原始图数据一起存储，只要确保使用新的标签和边类型来保持该图独立即可。这可以防止之后在查询图获取见解时混淆信息。

2. 我们的新图将包含具有 name 属性的 Film 节点。正如我们对主要电影信息图所做的那样，我们首先为 Film 节点在 name 属性上创建索引。这将大幅加快使用影片名称进行匹配的操作。我们将使用本章前面“如何使用投影”部分第 9 步中相同的语法：

```
CREATE INDEX film
FOR (f:Film)
ON f.name
```

3. 接下来，我们可以继续查询现有的 Entity 节点以获取特定信息，并将这些信息添加到新的独立图投影中。这里我们将再次使用一些相当高级的 Cypher，并再次借助 APOC 插件。这次，我们使用 `apoc.periodic.iterate()` 函数将查询的执行分批进行。这可以防止 Neo4j 一次性加载过多数据到内存中，从而避免因达到内存限制而引发错误。我们将通过使用 CALL 调用 `periodic.iterate()` 来启动此查询，后面跟一个左括号，其中第一个参数包含匹配 Entity 节点的 Cypher 逻辑。我们希望在投影中连接具有共同演员的影片，因此需要匹配包含两部影片的路径，每部影片都与另一个实体具有 STARRING 关系，且该实体包含一位演员。路径匹配后，我们通过返回 film1 和 film2 结束第一个 Cypher 字符串，这两个节点就是我们想要添加到投影中的节点。

在 `periodic.iterate()` 中，第二个参数包含另一个 Cypher 字符串，其中包含对前一个查询返回结果进行处理的逻辑。对于找到的每条路径，我们希望合并两个新的 Film 节点，并为它们设置 name 属性。然后，我们可以将新创建的节点别名 f1 和 f2 用 HAS_COMMON_ACTORS 边连接起来。函数的最后一个参数是一个字典，包含如何执行批处理查询的信息。`batchSize` 值为 1000 告诉函数先从第一个 Cypher 查询参数中查找 1,000 条结果，然后再用这些结果执行第二个参数的 Cypher。我们传入的另一个选项是 `parallel`，当设为 True 时，允许以每批 1,000 条进行并行处理。在这个特定案例中，并行处理这些批次会因为更新期间节点锁定而导致错误，我们将在第 9 章“常见错误与调试”中详细讨论。现在，让我们用右括号结束这个复杂的 APOC 函数，并在 Neo4j 浏览器中运行查询：

```
CALL apoc.periodic.iterate(
  "MATCH (film1:Entity)-[:STARRING]->(actor:Entity)
    <-[:STARRING]-(film2:Entity)
  RETURN film1, film2",
  "MERGE (f1:Film {name:film1.name})
  MERGE (f2:Film {name:film2.name})
  MERGE (f1)-[:HAS_COMMON_ACTORS]->(f2)",
  {batchSize:1000, parallel:false}
)
```

这个批量查询可能需要几分钟才能运行完成，具体取决于运行它的机器配置。完成后，Neo4j 浏览器将显示完成 `apoc.periodic.iterate()` 任务所需的批次数，以及是否有任何批次失败。如果我们此处使用了 `parallel:true`，很可能会注意到一些批次失败，并且由于可能同时写入相同节点，一些写操作将无法完成。

4. 图结构设置好后，我们现在可以向节点添加额外属性。对于我们的图投影，我们关注影片拍摄的国家，而在主图中，这些关系由 COUNTRY 边表示。我们可以将此关系转换为一个属性，即影片拍摄国家列表。

