

当它被实例化时。我们将为每个客户传递一个唯一的 customerID，并在 Customer 对象创建时使用 Faker 为其生成姓名和地址：

```
```python
class Customer:
    def __init__(self, customerID):
        self.customerID = customerID
        self.name = fake.name()
        self.address = fake.address()
```
```

+   4. 让我们通过生成一个客户来测试我们新的类。为了测试目的，我们可以用任意 ID 9999 创建一个客户，并打印我们 Customer 对象的属性：

```
```python
new_customer = Customer(9999)
print(new_customer.customerID)
print(new_customer.name)
print(new_customer.address)
```
```

+   5. 每次调用 Customer 类时，__init__() 方法都会自动生成客户的姓名和地址。有了我们新的类，我们现在能够快速生成客户来填充我们的图数据库。

+   6. 为了将此客户信息传递到我们的图数据库，以及我们整个管道，我们需要像第 5 章《使用图数据库》中那样从 Python 访问 Neo4j。实例化时，此类使用传递给它的凭据建立与 Neo4j 的连接。它还可以使用 query() 方法与 Neo4j 建立会话并运行查询，以及使用 close() 关闭连接。这是我们在上一章中使用的同一个 Neo4jConnect 类，因此有关如何将 Python 连接到 Neo4j 的更多信息，请参阅第 5 章《使用图数据库》。尽管我们还包含了一个名为 Graphtastic 的自定义包，您可以使用 from graphtastic.database.neo4j import Neo4jConnect 快速导入此类。为了透明起见，此连接方法的完整实现再次包含在以下代码块中：

```
```python
from neo4j import GraphDatabase
class Neo4jConnect:
    def __init__(self, uri, user, password):
        self.driver = GraphDatabase.driver(uri,
                      auth=(user, password))
    def close(self):
        self.driver.close()
    def query(self, query):
        session = self.driver.session()
        result = session.run(query)
        return result
```
```

+   7. 我们现在准备好生成并向 Neo4j 添加客户。这可以被视为客户注册我们的在线零售应用或网站。接下来我们需要能够将生成的客户数据传递到图数据库的能力。为此，我们可以创建一个方法 `add_customer()`。该方法将接受一个 `Customer` 对象 `c`，以及一个使用我们的 `Neo4jConnect` 类创建的经过身份验证的 Neo4j 连接。有了这些参数，我们就可以创建一个 Cypher 查询来添加客户，将节点属性替换为我们客户的属性，使用 `c.customerID`、`c.name` 和 `c.address`。这一切都通过 Python 的 f 字符串功能处理，围绕一个简单的 MERGE 函数。使用的额外大括号是为了在某些地方转义 f 字符串行为，因为 Cypher 指定节点属性的语法也使用 `{ }` 字符。根据我们的图模式，我们还指定新节点的 `customerID` 属性应为整数，使用 Cypher 的 `toInteger()`。组装好的查询然后可以使用 `connection.query()` 调用：

```
```python
def add_customer(c, connection):
    query = f'MERGE (:Customer {{customerID: toInteger({c.customerID}), '\
        f'name: "{c.name}", '\
        f'address: "{c.address}"}})'
    connection.query(query)
```
```

+   8. 让我们通过创建一个另一个示例客户来尝试这个新方法。使用我们的 `Customer` 类，我们可以生成一个具有所需属性的新 `Customer` 对象。与所有从 Python 到 Neo4j 的 Cypher 查询一样，我们然后使用 `Neo4jConnect` 建立连接，然后使用我们生成的 `Customer` 和我们的 Neo4j 连接调用新的 `add_customer` 方法，该方法处理向我们的图数据库写入节点。最后，我们使用 `connection.close()` 关闭会话：

```
```python
test_customer = Customer(9999)
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
add_customer(test_customer, connection)
connection.close()
```
```

+   9. 回到 Neo4j Browser，我们可以以多种方式检查节点是否已正确添加，但为了更直观，让我们打开 Neo4j Desktop 用于 Store DB 数据库，我们已写入该数据库。使用非常简单的 Cypher 匹配所有类型为 Customer 的节点，我们可以将客户返回到浏览器：

```
```cypher
MATCH (c:Customer) RETURN c
```
```

我们应该看到返回了一个节点。点击该 Customer 节点应该在 Neo4j Browser 窗口右侧显示节点属性，确认我们的属性已正确设置。接下来，我们现在可以向我们的图添加客户，但目前它们仍然与数据库中其余节点断开连接。根据我们的模式，我们需要创建关系来表示产品购买，但我们仍然需要一个模拟这些交易的流程。

10. 让我们接下来创建一个用于添加客户购买的方法，与我们定义 add_customer() 的方式类似。我们可以命名该方法为 add_purchase，这次，为了正确建模购买，我们需要更多参数。函数将接受一个客户 c、产品 ID、当前时间以及 Neo4j 连接。我们再次使用 f 字符串处理 Cypher 查询的组装。当客户选择产品并购买它时，为了在我们的图中建模这一点，我们需要在正确的 Customer 节点的 customerID 属性上 MATCH，并在匹配的 Product 节点的 productID 上 MATCH。这些 ID 适合匹配，因为我们在模式中将它们定义为唯一的——没有两个客户或两个产品会共享相同的 ID。我们查询的最后一步是在我们别名的 Customer 和 Product 节点 c 和 p 之间 MERGE 一条边。我们使用 PURCHASED 关系来描述这条边，并在 datetime 边属性上标注时间参数。最后一步是将查询发送到我们的 Neo4j 数据库，使用 connection.query()：

```
def add_purchase(c, productID, time, connection):
    query = f'MATCH (c:Customer {{customerID: toInteger({c.customerID})}})' \
    f'MATCH (p:Product {{productID: toInteger({productID})}})' \
    f'MERGE (c)-[:PURCHASED {{datetime:"{time}"}}]->(p)'
    connection.query(query)
```

11. 现在我们可以测试 add_purchase() 的行为。让我们使用我们已创建的 ID 为 9999 的 Customer 对象，它也已存在于数据库中，并模拟一次购买。我们可以在 products.csv 原始数据中或在 Neo4j 中找到客户要购买的产品——对于本示例，我们将使用 productID 为 1010 的节点。

12. 首先，我们从 datetime 导入 datetime，以便在购买时访问当前时间。然后我们建立与数据库的连接。我们使用我们的 add_purchase() 方法，并向其传递我们现有的 test_customer，以及 productID 1010、datetime.now() 的当前时间，以及 Neo4j 连接。边的添加将由我们的函数处理，因此我们可以关闭连接完成：

```
from datetime import datetime
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
add_purchase(test_customer, 1010, datetime.now(),
    connection)
connection.close()
```

13. 如同之前所做，让我们进入 Store DB 数据库的 Neo4j Browser，看看是否能找到我们新创建的边。我们可以再次运行一些 Cypher 来查找 Customer 和 Product 节点之间具有 PURCHASED 边类型的边，并返回我们匹配的节点：

```
MATCH (c:Customer)-[:PURCHASED]->(p:Product) RETURN c, p
```

如果边已正确添加，在浏览器中，我们应该看到两个节点由 PURCHASED 关系连接。如同新添加的 Customer 节点一样，我们可以通过点击它并检查 Browser 窗口右侧的其属性来检查 datetime 属性是否已正确添加到我们的新边。

+   14. 在我们的图模式中，我们注意到客户可能想要重复购买同一产品，并且这种行为应由同一对 Customer 和 Product 节点之间的多条边表示。我们应该确保我们的 Python 管道方法能够实现这种行为。为此，让我们再次运行相同的代码，其中 `test_customer` 第二次购买 ID 为 1010 的产品：

```
```python
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
add_purchase(test_customer, 1010, datetime.now(),
    connection)
connection.close()
```
```

+   15. 当我们的代码运行时，让我们通过在 Neo4j Browser 中运行另一个 Cypher 查询来检查是否已在我们的 Product 和 Customer 节点之间添加了一条新的独立关系。我们可以要么使用之前运行的查询在视觉上显示我们在节点之间有两条边，要么运行一个查询将客户和产品详细信息打印到表格中，以及节点之间关系的 datetime：

```
```cypher
MATCH (c:Customer)-[purchase:PURCHASED]->(p:Product)
RETURN c.name, p.name, purchase.datetime
```
```

我们应该看到在这对节点之间成功添加了两条边。在 `add_purchase()` 中，我们使用 MERGE 添加边，我们在 *第 5 章，使用图数据库* 中了解到，Cypher 的 MERGE 函数如果已存在匹配指定模式的节点或边，则不会创建新的节点或边。在我们的图中，我们可以通过在 MERGE 模式中指定 datetime 来允许多条边。因为我们在节点对之间不存在具有完全相同毫秒日期和时间的边，所以模式不匹配，从而创建了新的边。

+   16. 我们的测试表明 `add_customer()` 和 `add_purchase()` 按预期工作，并成功将客户数据写入我们的 Neo4j 数据库。让我们在继续之前快速清理我们添加到图中测试客户和购买。 为此，我们可以专门匹配客户节点，防止任何产品数据被删除。然后我们可以 DETACH 它们的关系，并从数据库中 DELETE 它们。这可以从 Neo4j Browser 运行：

```
```cypher
MATCH (c:Customer) DETACH DELETE c
```
```

+   17. 在设置了自定义方法后，我们现在拥有了模拟客户使用我们应用程序基本行为所需的一切。然而，在现实场景中，通常在很短的时间内会有许多客户与在线零售商互动。我们接下来可以使用 Python 函数

