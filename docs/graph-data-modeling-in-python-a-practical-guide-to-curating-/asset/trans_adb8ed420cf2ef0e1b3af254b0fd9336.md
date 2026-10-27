

```
MATCH (b)<-[:HAS_BRAND]-(r:Product)
WHERE NOT (c)-[:PURCHASED]->(r)
RETURN c, p, b, r
```

与其返回推荐的产品 ID，在此查询中我们返回匹配到的 Customer、Brand 和 Product 节点，并可以验证它们之间的路径是否符合预期。我们可以调整 RETURN 语句以仅筛选推荐的产品或其他感兴趣的节点类型。

5. 当我们对按品牌进行的推荐功能感到满意后，可以为所有客户执行这些操作，并基于这些推荐模拟购买。对于客户列表中的每个 Customer 对象，我们可以使用 rec_by_brand() 进行推荐，并将结果分配到一个列表中。我们指定一个条件，即 brand_recommendations 的长度必须不为 0，因为对于没有任何购买历史的新客户，使用此方法无法选择推荐产品。空列表也会导致下一步失败，在下一步中我们再次使用 random.choice() 方法从列表中为客户选择一个推荐产品进行购买。我们再次使用 add_purchase() 函数在图中创建一条边，连接我们的 Customer 与其基于推荐而新购买的产品：

```
for customer in customers:
    connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
    brand_recommendations = rec_by_brand(customer, connection)
    connection.close()
    if len(brand_recommendations) != 0:
        product = random.choice(brand_recommendations)
        connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
        add_purchase(customer, product, datetime.now(), connection)
        connection.close()
```

6. 当然，我们可能还想通过其他简单方法推荐产品，例如基于之前购买的类型或子类型。这将是更改 rec_by_brand() 中的路径匹配逻辑以遍历不同类型的关系、不同类型的节点的情况。我们可能还想防止基于产品是否与已购买的产品类型相同来推荐产品，假设客户不太可能购买自己已拥有的东西。在 Neo4j Browser 中，开始设计此过程的 Cypher 可能看起来像这样，利用模式匹配查找产品类型，并通过在 WHERE 子句中添加额外条件来否定为推荐匹配这些类型：

```
MATCH (c:Customer {customerID:10010})-[:PURCHASED]->(p:Product)
MATCH (p)-[:HAS_TYPE]->(st:Type)
MATCH (p)-[:HAS_BRAND]->(b:Brand)
```

```
MATCH (b)<-[:HAS_BRAND]-(r:Product)
WHERE NOT (c)-[:PURCHASED]->(r) AND (r)-[:HAS_TYPE]->(st)
RETURN DISTINCT r
```

一旦确认此逻辑工作正确，我们就可以更新 rec_by_brand() 方法以使用此逻辑。现在，让我们继续介绍另一种真实在线零售商常用的产品推荐类型。这将在下一节中介绍。

## 利用其他客户的购买行为

基于品牌推荐产品是一种简单但合理的方式，利用我们的图本体驱动进一步的客户购买。然而，这并不是在线零售商促进销售所采用的唯一方法，并且只使用了我们图数据库中可用的部分数据。

到目前为止，我们一直在收集模拟客户购买的许多数据，我们可以更整体地使用这些数据来查找客户感兴趣的产品。考虑一位过去通过我们的 Web 应用进行过购买的客户。这将作为图中一条边存储在我们的图中，同时还有其他客户购买相同产品的边。通过遍历与其他客户的关系，我们可以找到购买初始产品的客户随后购买的其他产品，并推荐它们。这种类型的推荐依赖于客户更可能购买彼此相似的一组产品，而不是随机购买商品的假设。

以下步骤展示了我们如何实现这一点：

- 1. 让我们编写一个方法，使用其他客户的购买行为向客户推荐产品，命名为 rec_by_copurchase()。该方法的结构与 rec_by_brand() 相似，只是 Cypher 修改为沿 PURCHASED 边回溯到我们目标客户已购买产品的其他客户。我们使用 WHERE 子句防止回溯到我们正在为其做推荐的原始客户。然后我们可以从具有相同购买历史的客户，即别名为 c2，到他们购买的其他产品。我们还确保不推荐客户 c 已购买的产品，同样使用 WHERE。其余部分与 rec_by_brand() 相同，并返回产品 ID 列表：

```
def rec_by_copurchase(c, connection):
    query = f'MATCH (c:Customer {{customerID: toInteger({c.customerID})}})' \
        '-[ :PURCHASED]->(p:Product)' \
        'MATCH (p)<-[:PURCHASED]-(c2:Customer)' \
        'WHERE c2 <> c ' \
        'MATCH (c2)-[:PURCHASED]->(r:Product)' \
        'WHERE p <> r ' \
        'RETURN DISTINCT r.productID as productID'
```

## 使用相似度分数推荐产品

在本章中我们将考虑的最后一个推荐产品的方法更依赖于 Python 及其与 Neo4j 数据库的接口。之前基于联合客户购买的推荐系统利用了客户数据，可能在零售应用中完成购买后使用，以引导用户关注其他感兴趣的产品。然而，我们可以通过查看整体购买历史相似度来扩展客户可能购买相似产品集合的想法。这种基于过去购买集合的通用推荐，更可能在应用的首页向客户提供。

为了比较客户购买情况，我们可以查看相似度度量。这些度量根据两个集合中元素的相似性返回一个分数。几种方法，每种都源于集合论，可用于计算节点组之间的相似性。示例有重叠相似度、Dice 系数，以及由于其易于实现和可解释性而常用的 Jaccard 相似度。

Jaccard 相似度定义为两个集合的交集除以同一集合的并集。基本上，这会产生 0 到 1 之间的值，其中 0 表示没有相似性，分数为 1 表示两个集合相同。我们可以使用此度量来查找在购买方面相似的客户。我们将为 Jaccard 相似度的计算定义自己的简单实现，但更多信息和示例可在 scikit-learn 文档中找到。

就我们的目的而言，当找到具有较高但不完全重叠的产品购买的相似客户时，我们可以识别它们购买集合之间剩余的差异并向客户推荐这些产品。到目前为止，我们的数据库包含我们 100 位客户的大量购买记录，因此充分利用我们可用的数据，让我们继续在 Python 中实现本章的最终推荐方法。以下步骤将展示如何在我们的用例中使用 Jaccard 相似度：

- 1. 计算客户购买之间的 Jaccard 相似度比之前的方法更复杂一些，而 Cypher 在基于路径的计算中表现出色，Python 更适合相似度计算。因此，让我们首先定义一个简单的方法，使用与 rec_by_brand() 和 rec_by_copurchase() 类似的步骤来提取给定客户 ID c_id 的所有购买记录：

```
def get_customer_purchases(c_id, connection):
    query = f'MATCH (c:Customer {{customerID: toInteger({c_id})}})' \
            '-[ :PURCHASED ]->(p:Product)' \
            'RETURN DISTINCT p.productID as productID'
    result = connection.query(query).data()
    result = [product['productID'] for product in result]
    return result
```

