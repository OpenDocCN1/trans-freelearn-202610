

**资源描述框架（RDF）**是一种可被机器解释的数据模型。虽然人类可以在网络上阅读 HTML，但机器无法理解自然语言，因此必须以另一种形式提供信息。消歧是一个重要方面；华盛顿指的是城市还是人物？是哪个人？为了表达这一点，RDF 中的信息通过 URI 或 URL 来引用，就像我们在 OpenRefine 中进行 reconciliation 时所做的那样。这就是 RDF 在 reconciliation 中派上用场的原因。

**SPARQL**（**SPARQL 协议和 RDF 查询语言**的递归缩写）是一种用于查询 RDF 数据源的语言。传统的 relational 数据库使用 SQL 作为查询语言；RDF 数据库以及对我们而言重要的 reconciliation 服务使用 SPARQL 进行通信。

如果你想将你的数据与 RDF 数据源进行 reconciliation，那么你必须告诉 OpenRefine 它如何与该数据源通信。当你安装了 RDF 扩展时，你已经为 OpenRefine 添加了 SPARQL 支持。然而，在我们能够将单元格值 reconciliation 到 URL 之前，我们必须配置数据源。为此，点击右上角的新 RDF 按钮并导航到 **Add reconciliation service | Based on SPARQL endpoint...**。请注意，你也可以使用本地 RDF 文件进行 reconciliation，如果你有自己的 URL 数据集，这很方便。OpenRefine 会显示 **Add SPARQL-based reconciliation service** 对话框。

![](img/f5874463a5242c6e8e1824fdc2a73ac7_86_0.png)

此对话框允许你为 SPARQL 端点选择一个名称，添加端点的详细信息（它位于何处以及如何工作），以及用于将单元格值匹配到 URL 的信息类型。你可以使用自己的 SPARQL 端点或在 `http://www.w3.org/wiki/SparqlEndpoints` 上列出的任何公共可用端点。

对于本示例，我们将使用我们设置的一个端点。它包含公开可用的 **美国国会图书馆主题词表（LCSH）** 数据集的处理版本。要使用此端点，请按如下方式填写详细信息：

- **Name:** LCSH
- **Endpoint URL:** `http://sparql.freeyourmetadata.org/`
- **Graph URI:** `http://sparql.freeyourmetadata.org/authorities-processed/`
- **Type:** Virtuoso
- **Label properties:** 仅勾选 **skos:prefLabel**

**Endpoint URL** 是端点所在的地址，而 **Graph URI** 标识在此端点中应使用的数据集。端点的 **Type** 用于指示底层端点软件；指定此项不是强制性的，但知道正确的类型可以启用低级技巧，从而大大提高 reconciliation 速度。最后，**Label properties** 指示可用于查找单元格值的字段名称。在 RDF 中，这些属性通常具有通用名称（实际上它们本身大多是 URL）。设置好这些属性后，点击 **OK** 按钮。

由于现在没有任何反应，你可能会好奇如何检查新 reconciliation 是否已正确添加。你可以通过转到 **Categories** 下拉菜单并导航到 **Reconcile | Start reconciling...** 来验证这一点。在左侧，你会看到新添加的服务。现在如果迫不及待想使用它，请继续阅读下一条 recipe！

## Recipe 4 – reconciling with Linked Data

在上一条 recipe 中，我们谈到了 RDF 和 SPARQL，但没有勾勒出这些技术诞生的更广泛背景，所以现在让我们介绍一下。2000 年左右，网络研究人员和工程师注意到人类不再是网络的唯一消费者；越来越多的机器客户端，也就是软件，开始为各种目的使用网络。然而，每一个这样的软件都必须为特定任务硬编码，并且无法解析人类网络上文档中的自然语言。因此，提出了一个名为 **Semantic Web** 的愿景，即一个信息也能被机器解释的网络。这是 RDF 和 SPARQL 的开端。

### Linking Datasets

然而，该愿景对许多人来说相当抽象且难以理解。其中几个概念依赖于本体和推理等概念，这些概念会迅速变得非常复杂。*Tim Berners-Lee*，网络的发明者，也是 Semantic Web 愿景的创建者之一，意识到了这一点，并推出了 Linked Data 原则（http://www.w3.org/DesignIssues/LinkedData.html）。这些原则将 Semantic Web 的重点转移到创建与其他数据集互联的数据上。原则如下：

- 1. 使用 URI 作为事物的名称
- 2. 使用 HTTP URI，以便人们可以查找这些名称
- 3. 当有人查找一个 URI 时，使用标准（RDF、SPARQL）提供有用的信息
- 4. 包含指向其他 URI 的链接，以便他们可以发现更多事物

因此，第一条原则要求为你的概念使用无歧义的标识符，第二条原则特别要求使用 HTTP URI，即 URL。这些原则已在我们将用于 reconciliation 的数据集中使用。第三条原则关注你的数据如何发布。事实上，RDF 扩展允许我们将数据集导出为 RDF，所以这也涵盖了。第四条原则是关于链接到其他数据集，这正是 reconciliation 将为我们做的事情。

言归正传，让我们开始 reconciliation。像 *Recipe 1 – reconciling values with Freebase* 一样，转到 **Categories** 下拉菜单并导航到 **Reconcile | Start reconciling...**，这将在你的屏幕上打开 reconciliation 对话框。这次，选择左侧你已在上一条 recipe 中安装的 **LCSH reconciliation service**。OpenRefine 现在将尝试一些单元格值，以查看是否能确定其类型。几秒钟后，它建议值类型为 skos:Concept。这确实是正确的，因为所有 **Category** 值实际上都是概念。你可以保留所有设置不变并点击 **Start Reconciling**。

![](img/f5874463a5242c6e8e1824fdc2a73ac7_88_0.png)

当 reconciliation 完成后，你会注意到三件事。首先，OpenRefine 创建了两个 facets，帮助我们找到匹配和未匹配的行。其次，你会在 **Category** 列顶部看到一个绿色条，指示有多少单元格值已被 reconciliation。第三，单元格值显示方式不同；有些是蓝色的，其他则有新选项。

![](img/f5874463a5242c6e8e1824fdc2a73ac7_89_0.png)

让我们先关注显示为蓝色的已 reconciliation 值。你看到的越多，reconciliation 就越成功（因此列顶部的条会越绿）。它们是蓝色的，因为它们实际上是链接；如果你点击它们，就会进入该术语已被 reconciliation 到的 URL 页面。例如，如果我们点击 **Specimens** 链接，我们将被导向 `http://id.loc.gov/authorities/subjects/sh87006764.html`，这确实是美国国会图书馆主题词表中 Specimens 的条目。这就是我们创建 Linked Data 的方式；而不是仅仅字符串 Specimens，对计算机来说看起来像九个随机字符，OpenRefine 将该值与一个 URL 进行了 reconciliation。该 URL 可被机器解释，因为该 URL 又链接到其他 URL，有助于定义其含义。

当你看到黑色未 reconciliation 的值时，对于该特定术语你运气不佳。它们不是链接，因此 OpenRefine 无法自动找到匹配的 URL。未 reconciliation 的值有两种情况；首先是找到了一些建议，但 OpenRefine 无法找到正确的一个。在这种情况下，它会为你列出几个替代方案。如果其中任何一个替代方案是正确的，你可以点击单个复选标记，这将仅用所选术语替换当前单元格值，或点击双重复选标记，这将替换所有具有相同值 的单元格。请注意替换所有相同值时要小心，因为其中一些可能实际上指的是另一个术语。请注意，你也可以点击建议，这会带你到其对应的网页以获取更多信息。

未 reconciliation 值的另一种情况是 OpenRefine 没有找到任何建议。在这种情况下，你就没什么运气了。也许所讨论的术语在你进行 reconciliation 的数据集中不存在，或者名称不同。无论如何，你唯一能做的就是手动分配一个 URL。为此，点击 **search for match** 链接，这将显示一个对话框，帮助你找到正确的匹配项。

