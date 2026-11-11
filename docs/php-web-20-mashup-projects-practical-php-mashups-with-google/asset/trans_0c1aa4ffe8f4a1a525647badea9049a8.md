
RDF 的目的是描述网络资源，即描述互联网上的某事物。例如，如果一个购物网站列出了某物的价格，那么价格究竟是什么？是以美元？墨西哥比索？还是俄罗斯卢布？对于一个网站来说，时间戳究竟是什么？机器解析器是否应该将 12 小时制的时间戳与 24 小时制的时间戳区别对待？XML 在非常高的层面上，本意是让各团体在交易格式上达成标准化。由于 XML 只是一种语言，实现细节被留给了相关方。RDF 是这一原始目标的下一步演进。它为该实现提供了一个框架。通过定义时间戳是什么，任何遇到该 RDF 文档的机器或人类都将毫无歧义地知道该时间戳是什么、它意味着什么以及应采用何种格式。

RDF 的基本概念和语法相当简单直接。RDF 将事物分组为它所称的三元组。一个三元组基本上表示“某物具有一个属性，其值为某物”。三元组使用主语、谓语和宾语的语法概念。在句子“该页面的大小为 21 千字节”中，该页面是主语。谓语是属性，在此例中为大小。宾语是该属性的值，即 21 千字节。通常在 RDF 中，主语由父元素的 about 属性表示。属性和值由该父元素下的元素和值对表示。页面大小的句子可以用 XML 记法表示如下：

```
<rdf:RDF
    xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    xmlns="http://www.example.org/pageProperties"
>
<rdf:Description rdf:about="http://www.shuchow.com/thecats.html">
    <creator>Shu Chow</creator>
    <title>My Cats</title>
    <lastMod>01/24/2007</lastMod>
    <size>21 kilobytes</size>
</rdf:Description>
</rdf:RDF>
```

在 RDF 中，每个元素都必须有命名空间。rdf 命名空间是必需的，并且必须指向 http://www.w3.org/1999/02/22-rdf-syntax-ns。这使我们能够访问构成该文档为 RDF 文档的核心 RDF 元素。在本文档中，我们三次访问 RDF 元素——一次作为文档的根元素，再一次使用 Description 元素来标识资源，再一次使用 about 属性来标识特定资源。用人类语言表述，标题可以表述为，“http://www.shuchow.com/thecats.html 处的网络资源，具有一个标题属性，其值为 'My Cats'”。更随意地说，我们可以说，“该页面的标题是 'My Cats'”。

将其分解为主语、谓语和宾语：

- 主语是 http://www.shuchow.com/thecats.html。
- 谓语是 title。这也可以表示为 URI。
- 宾语是 "My Cats"。

在 RDF 中，主语和谓语必须是 URI。然而，像前面的例子一样，谓语可以有命名空间。值可以是 URI，或者更常见的是字面量。字面量是谓语元素内的字符串值。

在我们的 mashup 中我们还会遇到另一个 RDF 元素。在前面的例子中，从上下文可以明显看出该网络资源是一个 HTML 页面。RDF Schema 规范有一个 type 元素的 resource 属性，它将主语分类为编程对象（与三元组对象相对），例如 PHP 或 Java 对象。

```
<rdf:RDF
    xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    xmlns="http://www.example.org/pageProperties"
>
<rdf:Description rdf:about="http://www.shuchow.com/thecats.html">
    <creator>Shu Chow</creator>
    <title>My Cats</title>
    <lastMod>01/24/2007</lastMod>
    <size>21 kilobytes</size>
    <rdf:type rdf:resource="http://www.example.org/objects#An_HTML_Page" />
</rdf:Description>
</rdf:RDF>
```

resource 属性始终是一个 URI。结合 type 元素，它们告诉我们，为了弄清楚这个资源到底是什么，我们应该访问 resource 属性的值。在此例中，该资源在 http://www.example.org/objects#An_HTML_Page 处有描述，其大概描述了一个 HTML 页面。

仅了解三元组的简单本质就足以让我们开始使用 RDF。在核心 RDF 规范中，还有一些与集合分组相关的元素。然而，由于规范设计为可扩展和扩展，超出这些的元素并不多。扩展的命名空间是 RDF 强大之处的来源。在我们的 mashup 中，我们将遇到更多扩展，并在遇到时更详细地检查它们。目前，我们已具备阅读和使用我们的经纬度数据源的基本技能。

![](img/c637937d44400f1da21fb7cca101de80_226_0.png)

> RDF 的常见扩展及其应用可以将 RDF 变成一个非常深奥的主题。要了解更多关于 RDF 的信息，W3C 创建了一个优秀的入门指南，位于 http://www.w3.org/TR/rdf-primer/。请注意，人们很容易陷入 RDF 的哲学基础——官方规范实际上是六份独立的文档。

## SPARQL

RDF 设计为一种数据存储。由此可知，RDF 推出后，人们就想要一种查询方式，就像传统数据库一样。SPARQL 是一种新的 RDF 查询语言，最近已成为 W3C 推荐标准。你可以将 SPARQL 视为编写查询，类似于数据库的 SQL，用来解析 XML 文件，特别是 RDF 文件。返回给你的结果是与 SQL 中一样的行列表。

大多数人都是借助查询数据库的命令行客户端来学习 SQL。这使我们能够试验和操作查询结构。幸运的是，SPARQL 也有类似的东西；SPARQLer，位于 http://www.sparql.org/sparql.html，是一款交互式网络工具，允许你指定网络上的 RDF 文档作为输入并对其编写 SPARQL 查询。它会像数据库客户端的结果一样向我们显示查询结果。在我们初步讨论 SPARQL 时，我们将使用此查询工具以及位于 http://www.shuchow.com/mashups/ch6/pets.rdf 的示例文档 RDF 文档。该 RDF 文档列出了我工资单供养的所有动物。

## Analyzing the Query Subject

在数据库世界中，在开始编写查询之前，你需要先了解一点模式，要么通过实体关系图（如果你有良好的文档），要么简单地使用 SHOW TABLES 和 EXAMINE SQL 命令。你需要对 SPARQL 做同样的事情。有时主机会有文档，但通常，你只需要阅读 RDF 文件以大致了解文档。让我们从打开我们将要使用的 RDF 文件开始练习，地址为 http://www.shuchow.com/mashups/ch6/pets.rdf。你的浏览器要么将此文件下载到硬盘，要么在窗口中打开它。如果在窗口中打开，它可能会应用样式表来美化呈现。在这种情况下，你需要查看文档的源代码才能看到所有标签和命名空间前缀。

此 RDF 文件非常直接简单。我们从根元素开始，后面跟着命名空间：

```
<rdf:RDF
    xmlns:mypets="http://www.shuchow.com/"
    xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    xmlns:rdfs="http://www.w3.org/2000/01/rdf-schema#"
>
```

命名空间 rdf 和 rdfs 绑定到 w3.org 资源，这告诉我们它们是行业标准。mypets 则绑定到 shuchow.com，即文件的域名。这意味着它很可能是由 shuchow.com 组织创建的专有词汇，以支持信息。要了解更多，我们可以访问该站点。如此应该会带我们找到有关我们将遇到的一些语法的文档。

文件的其余部分基本上是围绕 Description 元素包装的宠物列表，其中包含一些作为子元素的详细信息。Description 元素中的 about 属性指向此项目的确切主题。

```
<rdf:Description rdf:about="http://www.shuchow.com/thecats.html#avi">
    <mypets:name>Avi</mypets:name>
    <mypets:age>6</mypets:age>
    <mypets:gender>F</mypets:gender>
    <rdfs:type rdf:resource="http://www.shuchow.com/#parrot"/>
</rdf:Description>
```

每只宠物的名字、年龄和性别都是其各自元素的值。这些元素都命名空间为 mypets。项目的类型是一个指向描述此“事物”是什么的位置的 URI。对于此文件，它是一个虚构的 URI，仅用作区分我家动物类型的方式。在现实世界中，它也可能不指向真实文件，或者背后可能有复杂的 RDF 分类法定义。这些 Description 块针对每只宠物重复。

## Anatomy of a SPARQL Query

如果你了解 SQL，那么理解 SPARQL 查询的前几行应该很容易。让我们看一个简单的 SPARQL 查询来了解其各部分。假设我们想提取某只特定宠物的某条特定信息。假设我们想提取 Saffy 的年龄。我们知道在文档中，年龄是 mypets:age 元素的值。我们也知道宠物的名字 Saffy 在 mypets:name 元素中。我们需要一个查询来提取 mypets:age 的值，并限制为 mypets:name 的值。
