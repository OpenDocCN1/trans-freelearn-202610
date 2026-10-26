

我们可选的第二个选项是 **自动匹配高置信度的候选项**。对账服务还会返回一个匹配分数，用来表示其对匹配结果的信心程度。自动匹配选项默认是开启的，这意味着如果分数足够高，OpenRefine 就会假定它做出了正确的选择并将其声明为匹配。对于其他单元格值（或如果你取消了复选框），你将需要手动确认匹配。我们建议保留此选项的勾选，因为它可以为你节省相当多的工作。

第三个也是最后一个选项允许你向对账服务发送额外数据。匹配不再基于单一列值，而是可以考虑不同的列。为此，你需要指明属性名称。对于 Powerhouse Museum 数据集，并不存在此类其他相关列。但如果你有一个包含国家名称和国家代码的数据集，将两者都发送给对账服务可能会有所帮助。

在勾选了 **不对特定类型进行对账** 后，点击 **开始对账**。对账过程可能需要一些时间，因为 OpenRefine 现在会要求对账服务为每个唯一的单元格值查找一个 URL。窗口顶部的黄色信息栏会告诉你已完成多少。

![](img/f5874463a5242c6e8e1824fdc2a73ac7_82_0.png)

这可能是你去喝杯咖啡或在电脑上做另一项任务，让对账在后台运行的好时机。当过程完成后，你会注意到 OpenRefine 已自动为你创建了 **判断** 和 **最佳候选分数** 两个分面。

![](img/f5874463a5242c6e8e1824fdc2a73ac7_82_1.png)

**判断** 分面允许你将数据集划分为已完成对账的行和未完成的行，以及 **Categories** 单元格为空值因此不是对账候选的行。你在这里看到的内容可能会让你感到惊讶，分面中只有 **(blank)** 和 **none** 两个过滤器。已对账的行在哪里？

事实证明，不幸的是，使用 Freebase 进行对账并未成功，没有找到任何匹配项。这是否意味着我们做错了什么？不，我们没有做错，只是我们数据库中的术语似乎并不是 Freebase 所涵盖的那种主题。这是否意味着 Freebase 不是对账的好选择？完全不是，只是碰巧我们的数据集不太合适。你在其他数据集上可能会有更好的运气，所以一定要试试。

让我们通过使用 Remove All 按钮删除分面并在 Undo / Redo 菜单中后退一步来撤销这次不成功的对账。（你可以通过转到 Categories 下拉菜单 | Reconcile | Facets 来恢复它们。）接下来的食谱将向你展示如何尝试其他对账服务。

![](img/f5874463a5242c6e8e1824fdc2a73ac7_83_0.png)

> 有时你只是想尝试使用某个服务进行对账，看看它是如何工作的。这是否意味着每次都要经历整个对账过程？如果巧妙地使用分面，就不必如此！我们倾向于先在数据集的一小部分上尝试对账，然后再决定是否要在所有行上运行它。为了做出或多或少的随机选择，我们可以在 Record ID 列上添加一个文本过滤器并输入一个数字，例如 4。假设所有 ID 中的数字分布相等，这将选择大约 10% 的行。如果这仍然太多，只需不断添加数字，直到行数减少到几百行。例如，413 对我们来说就奏效了。

## Recipe 2 – 安装扩展

虽然 OpenRefine 可以开箱即用地添加新的对账服务，但这些只是底层以某种方式工作的服务。然而，还有其他类型的服务，如果你想使用它们，你需要在 OpenRefine 中添加一些功能形式的扩展。因此，在向你展示如何添加这些服务之前，我们首先必须解释如何安装 OpenRefine 扩展。并非所有扩展都提供对账服务，因此本食谱描述的是通用安装程序。截至撰写时，有多个 OpenRefine 扩展可用，包括以下几点：

- 由 Digital Enterprise Research Institute (DERI) 提供的 RDF 扩展，它增加了对 RDF 导出和与 SPARQL 端点对账的支持。我们将在下一个食谱中解释这两个术语。
- 由本书作者之一编写的 Named-Entity Recognition (NER) 扩展，它允许你从全文字段中提取 URL。本章最后一个食谱将详细介绍该扩展的使用。

与 OpenRefine 一样，所有扩展都是免费提供的。可用扩展的最新列表维护在 https://github.com/OpenRefine/OpenRefine/wiki/Extensions，也包括它们的下载位置。由于扩展种类繁多，安装它们看起来可能很棘手。然而，本食谱将向你展示安装任何扩展的技术。

首先，你需要找到扩展需要安装的路径。这取决于你的操作系统，但 OpenRefine 可以帮助你找到它。在起始页底部，有一个名为 Browse workspace directory 的链接。

![](img/f5874463a5242c6e8e1824fdc2a73ac7_84_0.png)

如果你点击此链接，你将进入 OpenRefine 应用程序文件夹（至少在 Windows 和 Mac 上；Linux 请继续阅读）。在此文件夹中，你需要创建一个名为 extensions 的新文件夹（如果它还不存在）。这将是你需要添加扩展的路径。对于不同的操作系统，它应该如下所示：

- Windows: C:\Documents and Settings\(your username)\Application Data\OpenRefine\extensions 或可能在 Local Settings 文件夹中 C:\Documents and Settings\(your username)\Local Settings\Application Data\OpenRefine\extensions
- Mac OS X: /Users/(your username)/Library/Application Support/OpenRefine/extensions 或从你的主文件夹开始，在 /Users/(your username)/Library/Application Support/OpenRefine/extensions
- Linux: /home/refine/webapp/extensions

下一步是下载一个扩展并将其放置在该文件夹中。例如，让我们安装 RDF 扩展。从 http://refine.deri.ie/ 下载扩展；这将为你在下载文件夹中获取一个 ZIP 文件。以你习惯的方式解压此 ZIP 归档的内容。根据你使用的软件，可能会生成额外的文件夹，但我们感兴趣的文件夹名为 rdf-extension。

将此文件夹移动到我们之前找到的 OpenRefine 的 extensions 文件夹中。在 extensions 文件夹中，你现在应该看到一个名为 rdf-extension 的文件夹，其中包含其他文件夹，例如 images、MOD-INF、scripts 和 styles。

现在重新启动 OpenRefine 以使扩展生效。仅仅关闭浏览器窗口是不够的；你必须真正关闭 OpenRefine 应用程序本身，通过在屏幕底部右键或 control-点击其图标并选择 Close。然后，像往常一样重新启动 OpenRefine 并打开一个项目。当项目加载后，右上角出现一个新的 RDF 按钮，这表明安装成功：

![](img/f5874463a5242c6e8e1824fdc2a73ac7_85_0.png)

如果你看不到该按钮，请确保正确的扩展文件夹已放置在正确的位置，并且 OpenRefine 已正确重新启动。

你也可以对其他扩展重复本食谱中的步骤。在接下来的食谱中，我们将使用 RDF 扩展，因此请确保安装它。在后面的食谱中，我们也将需要 NER 扩展，所以趁此机会也安装它吧。

## Recipe 3 – 添加对账服务

本食谱需要你已安装 RDF 扩展。如果没有，请查看上一食谱。如果你已安装，你可能会想知道 RDF 和 SPARQL 这些术语的含义，因为它们在整个扩展中都被使用。这马上就会变得清晰。

