

给定文档的内容元素列表可能由任意组合的 Content Transfer 和 Content Reference 元素组成。

Content Transfer 元素除了实际的内容位之外还有一些有趣的属性。RetrievalName 属性不被 CE 使用，而是作为下载应用程序使用的简单文件名的提示（例如，VenusAndAdonis.html）。它通常由上传应用程序设置为原始文件名（如果已知）。ContentType 属性是两种内容元素共有的属性，并且同样通常由上传应用程序设置，标识内容的类型，可用作操作系统在下载后查找打开内容的合适应用程序的提示。如果上传应用程序未为 ContentType 提供值，CE 会根据 RetrievalName 文件扩展名使用一个相当简单的猜测算法。最好由上传应用程序显式设置这两个属性，但并非所有应用程序都这样做。

将内容元素属性 ContentType 与文档属性 MimeType 对比。这些属性性质相似但并不完全相同。如果在 checkin 之前应用程序未提供值，MimeType 是基于内容元素的内容类型值计算得出的。对于只有一个内容元素的文档，这两个属性的值将相同，但对于具有多个内容元素的文档，MimeType 属性将指示复合类型。为方便起见，所有 ContentType 值的列表可在多值属性 Document.ContentElementsPresent 中获得。

## 内容元素编号

由于内容元素列表实际上是一个列表，每个元素在列表中都有一个隐式位置编号。编号从 0 开始还是从 1 开始取决于所使用的编程语言，但对用户而言它只是一系列项目，因此实际位置编号很少对用户界面目的很重要。每个内容元素在其名为 ElementSequenceNumber 的属性中都有一个与其关联的显式编号。该编号与特定的物理内容元素相关联；它不是位置编号。它永远不会更改，也永远不会被重用（在该文档内），无论在文档 checkin 之前内容元素列表被更改多少次。

| Position | ESN |
|---|---|
| 0 | 114 |
| 1 | 376 |
| 2 | 248 |

例如，参考上表。如果您从三个内容元素的列表开始，它们将在内容元素列表中隐式位于位置 0、1 和 2（或 1、2 和 3）。假设为了举例，它们分别具有 ElementSequenceNumber 值 114、376 和 248。如果您删除中间的内容元素，您仍然会在位置 0 和 1（或 1 和 2）有元素，但 ElementSequenceNumber 值将为 114 和 248。如果您在这两个元素之间插入一个新元素，您将恢复到原始的位置编号列表，但该内容元素的 ElementSequenceNumber 值将不是 376。它将是一个从未在该文档中用过的新编号。

## 版本控制

在将内容放入和取出存储库之后，内容管理最重要的下一个功能可能是版本控制。版本控制在其最基本的定义中是指能够在跟踪先前版本的同时更新内容。CE 的版本控制值得探索的细节很多。

> 什么是修订？虽然 version 在 CM 中是一个具有明确且不同的技术含义的术语，但 revision 这个术语使用得不那么正式，并且语义经常重叠。它有时用于指内容任何更新，包括作者在一次 checkout/checkin 周期中对文档所做的多次更新。它有时也用于指对文档的一组经过编辑批准的更改，该文档在修订过程中可能经历了几次 checkout/checkin 周期。我们的建议是始终寻找上下文以澄清所使用的 revision 的特定含义。在任何情况下，都不要假设 revision 和 version 是同义词。

在 CE 中，随着文档被反复版本化，每个新版本都是其自身的一个 Document 对象。它具有独立的身份、安全性、属性值和内容。换句话说，如果有人拥有特定文档的 ID，该 ID 将始终引用完全相同的文档版本。（通常通过路径引用文档时并非如此，但我们稍后会谈到。）

如果每个文档的每次编辑都是存储库中漂浮的单独对象，那就不是特别方便，而在 CE 中并非如此。该逻辑文档的所有单独版本都统一在一个名为 Version Series 的集合对象中。Version Series 有自己的 ID。属性 VersionSeries.Versions 是通过版本控制相关联的文档的有序集合。事实上，Version Series 对象本身似乎没有任何属性。它的所有属性都直接来自或从其包含的文档的属性中合成。Version Series 对象的访问控制取自当前文档。

作为说明，这里是 XT 对一个经历了几次 checkout/checkin 周期版本的文档的描绘。在阅读本节其余部分中的版本控制概念时，您可能希望参考此图。

| Document: | Once Upon a Chapter (Version 1.0, Released) |
|  | Title ▲ | Version Status | Major Version | Minor Version | Modified By | Modified On |
|  | Once Upon a Chapter | Reservation | 1 | 4 | madi | 10/10/10 9:41 PM |
|  | Once Upon a Chapter | In Process | 1 | 3 | madi | 10/10/10 9:41 PM |
|  | Once Upon a Chapter | Superseded | 1 | 2 | wyatt | 10/10/10 9:40 PM |
|  | Once Upon a Chapter | Superseded | 1 | 1 | meg | 10/10/10 9:39 PM |
|  | Once Upon a Chapter | Released | 1 | 0 | meg | 10/10/10 9:37 PM |
|  | Once Upon a Chapter | Superseded | 0 | 2 | madi | 10/10/10 9:36 PM |
|  | Once Upon a Chapter | Superseded | 0 | 1 | wyatt | 10/10/10 9:34 PM |

让我们回顾一下：所有 Document 对象也是 Version Series 对象的一部分。随着文档的版本化，新版本会自动成为同一个 Version Series 对象的一部分。每个文档版本和 Version Series 本身都有单独的身份。从任何 Document 对象，您都可以导航到 Version Series 对象，从 Version Series 对象您可以导航到该 Version Series 中的任何 Document 对象。（有快捷方式可让您直接从一个 Document 对象导航到同一 Version Series 中的另一个 Document 对象。）

## Checkout 和 checkin

CE 中的版本控制过程使用 checkout/checkin 模型。也就是说，希望制作文档新版本的人首先 checkout 该文档。该操作创建一个新的文档版本（请记住，它是一个完全不同的对象），标记为 reservation。正是 reservation 版本实际被更新，然后最终被 checkin。相比之下，被 checkout 的文档版本被称为 reserved。

对于一个 Version Series 来说，同时只能有一个 checkout 操作，并且 checkout 总是在最近创建的文档版本上进行。如您所见，checkout 充当对 Version Series 的一种锁定，以防止另一个调用者也执行 checkout。第一次 checkout 必须得到解决，要么通过 reservation 的后续 checkin，要么通过取消 checkout。（取消 checkout 是一个 API 人工产物，也被带入了许多应用程序。事实上，CE 服务器中并没有这样的操作。您可以通过对 reservation 文档版本执行普通的删除来取消 checkout。CE 会在您删除 reservation 时自动更新 checkout 记账。）

