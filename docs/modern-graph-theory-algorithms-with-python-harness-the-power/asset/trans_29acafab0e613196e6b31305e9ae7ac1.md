

![](img/71ff737206f7d80bb625b3c9aca9dc7f_237_0.png)

图 12.4 – Neo4j 下载页面

点击图 12.4 中的“下载”按钮后，系统会提示你按照安装程序的说明完成安装。安装完成后，你会看到一个图标，或者安装过程会直接在你的机器上启动 Neo4j。这应该会带你进入一个如下所示的页面：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_237_1.png)

图 12.5 – Neo4j 的起始页面

如图 12.6 所示，一旦你通过点击“启动”按钮启动了 Movie DBMS 数据库，你就能够连接到该数据库。启动这个数据库可能需要几分钟时间。由于你在阅读本章之前可能不熟悉 Neo4j，因此我们将点击 about-movies.neo4j-browser-guide 文件选项，并运行几个查询示例来熟悉 Cypher 的语法和结果。

当你点击 about-movies.neo4j-browser-guide 文件选项时，你会看到类似图 12.7 的页面。第一页展示了一个基本查询，用于提取某位演员的数据。在这里，我们将演员改为 Brad Pitt，并在图中查询 Brad Pitt 的实例。当你的代码改为图 12.7 所示的内容后，点击箭头运行代码。请注意，你会看到所写代码下方有默认教程。我们的 Neo4j 查询图片中没有显示这个教程，但它会出现在你的屏幕上：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_239_0.png)

图 12.7 – 一个搜索 Brad Pitt 的基本 Cypher 查询

这个数据库不包含任何关于 Brad Pitt 的信息。对 Tom Hanks 运行建议的查询确实会产生 JSON 形式的信息，因为该数据不包含图元素。当你查询数据库中关于 Tom Hanks 的信息时，你应该会看到类似图 12.8 的内容：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_239_1.png)

图 12.8 – 在 Movie DBMS 中查询 Tom Hanks 的结果

让我们进入指南的下一页，向数据库中添加一位演员。我们将添加 Brad Pitt，因为我们的查询最初返回为空。图 12.9 展示了如何向数据库中添加演员及其出生年份。

![](img/71ff737206f7d80bb625b3c9aca9dc7f_239_2.png)

图 12.9 – 为 Brad Pitt 创建条目的说明

我们还可以使用 `MERGE` 语句结合 `CREATE` 和 `MATCH` 语句来创建或更新数据库中的记录，这取决于数据库中已存在的内容，以创建新记录或更新现有记录。我们将使用图 12.14 中所示的代码将 Brad Pitt 的信息重新添加到数据库中：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_242_0.png)

图 12.14 – Cypher 代码中的更新示例

当我们运行图 12.14 中的代码时，会再次看到图 12.9 的内容，只是此时 Brad Pitt 的信息已被添加到数据库中。既然我们掌握了一些基本的 Cypher 命令，就可以在下一节中继续学习更复杂的图数据库操作。

## 更复杂的查询示例

既然我们已经知道如何执行基本查询，就可以继续学习 Neo4j 中更复杂的操作，例如添加条目之间的关系。让我们通过一个 Cypher 查询，将 Brad Pitt 与《西藏七年》以及他所饰演的角色 Heinrich Harrer 连接起来。

> 注意
> 在运行此查询之前，你需要使用“基本查询示例”一节中的代码将电影和演员重新添加到数据库中。该查询如图 12.15 所示。

![](img/71ff737206f7d80bb625b3c9aca9dc7f_242_1.png)

图 12.15 – 一个将 Brad Pitt 与其在《西藏七年》中饰演的角色 Heinrich Harrer 连接起来的 Cypher 查询

运行此查询会返回有关演员、电影以及我们刚刚在两个数据集之间创建的关系的信息，如图 12.16 所示：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_243_0.png)

图 12.16 – 图 12.15 中用于连接 Brad Pitt 和《西藏七年》的 Cypher 查询的结果

Neo4j Cypher 指南的下一页展示了创建此关系的另一种方式。我们鼓励你修改该代码，添加另一位演员和一部电影作为练习。接下来，让我们关注 Cypher 的 `WHERE` 子句，其功能与 SQL 中的 `WHERE` 子句非常相似。

我们将搜索名字以“Brad”开头的人，这应该会拉出 Brad Pitt 的条目，以及任何其他名字为 Brad 的演员。图 12.17 展示了此查询：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_243_1.png)

图 12.17 – Cypher 中 `WHERE` 查询的示例

当我们运行图 12.17 中的 Cypher 代码时，应该会在数据库中看到两个 Brad Pitt 实例，如图 12.18 所示：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_244_0.png)

图 12.18 – 图 12.17 中 `WHERE` Cypher 查询的结果

我们还可以基于关系进行匹配，例如查找与某位演员关联的电影。让我们尝试一个关于关系的 Cypher 查询，以查找与 Brad Pitt 关联的所有电影（这里只有《西藏七年》，因为我们添加了这个关系）。图 12.19 展示了我们将用于查询关系的语句：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_244_1.png)

图 12.19 – 一个关于人物与电影之间图关系的 Cypher 查询

当我们运行图 12.19 中所示的 Cypher 查询时，我们会以图的形式看到 Brad Pitt 实例与《西藏七年》之间的联系（图 12.20）：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_245_0.png)

图 12.20 – 图 12.19 中图关系查询的结果

图 12.20 展示了图数据库的核心优势。我们能够看到跨表（此处为 Person 表和 Movie 表）连接不同条目的关系，以及这种关系的性质。很容易看出演员与电影之间的关联；如果存在更多电影，我们也能通过 Neo4j 中基于图的查询轻松可视化查询结果。

虽然这个电影数据库包含基本的表和关系，但我们可以想象“你能在图数据库中表示什么？”一节中的示例涉及更多表之间更复杂的关系。以图的形式跨表和表内条目可视化查询结果，能够为查询中感兴趣的数据和关系提供易于理解的摘要。图数据库的强大之处正在于此。即使对于包含许多子查询的查询，或者当搜索非常大的表及其关系时，查询速度也往往很快；而结果的可视化则允许用户深入探索跨表的关系（或者为报告提供简洁的视觉呈现）。

如果你想进一步探索 Neo4j，Neo4j 的 AuraDB 允许用户通过 Python 连接数据库：先创建账户，然后使用 Python 中的 `neo4j` 包和 `GraphDatabase.driver()` 函数通过 API 连接 Neo4j。

**注意**
对于任何大型数据库或希望永久保存的数据库，你需要付费账户。

数据库创建完成后，你可以通过 Python 的 `neo4j driver.execute_query()` 语句查询 Neo4j 数据库，并将数据作为网络导入到 igraph 中进行分析，以便分析结果。这为你的分析项目提供了图存储与图分析的强大组合。如果你感兴趣，我们鼓励你进一步探索 Neo4j，并创建属于自己的将 igraph 与 Neo4j AuraDB 账户连接起来的项目。

## 总结

在本章中，我们探讨了图数据库具有优势的场景，熟悉了一个名为 Neo4j 的开源图数据库，并学习了 Neo4j 的查询语言 Cypher 的一些基础知识。我们在一个 Neo4j 电影数据库中创建、删除和修改了记录。我们还探讨了查询图数据库的优势，以及图数据库所特有的查询结果可视化功能。如果你感兴趣，我建议你查阅 Cypher 和 Neo4j 的相关资源，深入了解图数据库可以实现的功能。

在下一章中，我们将综合运用本书迄今为止学到的所有技能，解决一个现实世界的问题：预测埃博拉疫情在刚果民主共和国各地区随时间和地理分布的严重程度。

## 参考文献

Adler, M. W., and Johnson, A. M. (1988). *Contact tracing for HIV infection.* British Medical Journal (Clinical research ed.), 296(6634), 1420.

Angles, R., Arenas, M., Barceló, P., Hogan, A., Reutter, J., and Vrgoč, D. (2017). *Foundations of modern query languages for graph databases.* ACM Computing Surveys (CSUR), 50(5), 1–40.

Beas-Luna, R., Novak, M., Carr, M. H., Tinker, M. T., Black, A., Caselle, J. E., ... and Iles, A. (2014). *An online database for informing ecological network models:* `http://kelpforest.ucsc.edu`. PloS one, 9(10), e109356.

Hyman, J. M., Li, J., and Stanley, E. A. (2003). *Modeling the impact of random screening and contact tracing in reducing the spread of HIV.* Mathematical biosciences, 181(1), 17–54.

Mueller, W., Rudowicz-Nawrocka, J., Otrzasek, J., Idziaszek, P., and Weres, J. (2016). *Spatial data and graph databases for identifying relations among members of cattle herd.* International Multidisciplinary Scientific GeoConference: SGEM, 1, 835–841.

