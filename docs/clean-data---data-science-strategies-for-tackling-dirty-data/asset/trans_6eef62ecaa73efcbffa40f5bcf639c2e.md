
- 如果可能，提供数据的可视化。
- 解决我们设定的研究问题。我们的流程有效吗？我们成功了吗？

这工作量很大，但我们提前准备得越多、越早开始，就越有可能让我们的数据科学晚宴派对取得成功。

## 第一步 – 关于 Stack Overflow 的提问

为了启动我们的项目，我们需要提出一个相当有趣的问题，该问题需要一些简单的数据分析来回答。我们应该从哪里开始？首先，让我们回顾一下我们对 Stack Overflow 的了解。我们知道它是一个面向程序员的问答网站，可以假设程序员在他们提问和回答的问题中很可能使用了大量源代码、错误日志和配置文件。此外，我们知道，有时在像 Stack Overflow 这样的基于网络的平台上发布这些长文本转储会很尴尬，因为行长度、格式和其他可读性问题。

看到如此多包含令人沮丧的大量文本的问题和答案，我想知道 Stack Overflow 上的程序员是否会通过外部粘贴站点（如 Pastebin 或 JSFiddle）链接到他们的代码或日志文件，例如 http://www.Pastebin.com 是一个网站，您可以在其中粘贴大量文本，例如源代码或日志文件，网站会为您提供一个可以与他人共享的简短 URL。大多数粘贴站点还允许进行源代码语法高亮，而 Stack Overflow 默认不这样做。

在 IRC 和电子邮件中使用粘贴站点非常常见，但 Stack Overflow 呢？一方面，就像在 IRC 或电子邮件中一样，提供链接可以使问题或答案更短，因此其余问题更容易阅读。但另一方面，根据使用的粘贴站点，URL 不保证永远可用。这意味着问题或答案可能因链接腐烂而随着时间的推移失去价值。

像 JSFiddle 这样的工具以一种额外的方式使这个问题复杂化。在像 JSFiddle 这样的交互式粘贴站点上，您不仅可以粘贴源代码并获取其 URL，还可以允许其他人在浏览器中直接编辑和运行代码。这在 Stack Overflow 的问答场景中可能非常有帮助，尤其是在基于浏览器的语言（如 JavaScript）中。然而，链接腐烂的问题仍然存在。此外，对于初学者来说，JSFiddle 比简单的代码转储站点（如 Pastebin）使用起来稍微复杂一些。

![](img/9aa003afabe05fdb07da46ad71325de6_213_0.png)

JSFiddle 有四个窗口，分别用于 HTML、CSS、JavaScript 和结果。

> 在 Stack Overflow 的社区讨论区，关于是否应该使用粘贴站点以及包含粘贴站点链接而没有实际代码的问题或答案的政策，应该如何处理，存在相当多的争论。总的来说，尽管人们倾向于同意粘贴站点可能有用，但他们也认识到保护 Stack Overflow 本身的持久性和实用性很重要。社区已决定应避免发布仅包含链接而没有代码的问题或答案。如果你想回顾该讨论，可以从此链接开始： http://meta.stackexchange.com/questions/149890/。

就我们而言，我们不需要在这场辩论中选边站队。相反，我们可以提出一些简单的数据驱动问题，例如：

- 1. 人们在 Stack Overflow 上使用 Pastebin 和 JSFiddle（以及其他类似的粘贴站点）的频率有多高？
- 2. 他们在问题中使用粘贴站点更多还是在答案中使用？
- 3. 引用粘贴站点 URL 的帖子是否也倾向于包含源代码；如果是，包含多少？

我们可以在收集、存储和清理 Stack Overflow 数据时使用这些问题作为我们的动机。即使最终发现其中一些问题太难或无法回答，记住我们的总体目的将有助于指导我们需要进行哪种清理。把问题放在我们思维的前沿将阻止我们偏离轨道或执行最终会证明是徒劳或浪费时间的任务。

## 第二步 – 收集和存储 Stack Overflow 数据

在撰写本文时，Stack Exchange 以 XML 文件的形式免费提供其整个网站系列（包括 Stack Overflow）的数据供任何人下载。在本节中，我们将下载 Stack Overflow 文件，并将数据导入我们 MySQL 服务器上的数据库。最后，我们将为测试目的创建这些表的几个较小版本。

### 下载 Stack Overflow 数据转储

Stack Exchange 的所有可用数据都可以在 Internet Archive 上下载。截至撰写本文时，2014 年 9 月的数据转储是最新的。每个 Stack Exchange 站点都有一个或多个文件，每个文件都链接到此详细信息页面： https://archive.org/details/stackexchange。

我们只对以下列表中按字母顺序排列的八个 Stack Overflow 文件感兴趣：

|  |  |
|---|---|
| stackoverflow.com-Badges.7z | 77.8 MB |
| stackoverflow.com-Comments.7z | 1.8 GB |
| stackoverflow.com-PostHistory.7z | 9.4 GB |
| stackoverflow.com-PostLinks.7z | 26.4 MB |
| stackoverflow.com-Posts.7z | 5.7 GB |
| stackoverflow.com-Tags.7z | 508.1 KB |
| stackoverflow.com-Users.7z | 100.5 MB |
| stackoverflow.com-Votes.7z | 385.0 MB |

Archive.org 列表显示我们感兴趣的八个 Stack Overflow 文件。

对于列表中的每个文件，右键单击链接并指示浏览器将文件保存到磁盘。

### 解档文件

请注意，每个文件都有 .7z 扩展名。这是一种压缩归档格式。可以使用匹配的 7-Zip 软件或其他兼容软件包进行解压缩和解档。7-Zip 并不是我们在 *第 2 章，基础 – 格式、类型和编码* 中讨论的更常见的归档工具之一，您可能尚未在计算机上安装兼容的解档程序，因此我们可以将其视为我们需要解决的第一个小问题。尝试双击文件以打开它，但如果您没有安装与 .7z 扩展名关联的软件，则需要安装适当的 7-Zip 解档程序。

- 对于 Windows，您可以从其网站下载 7-Zip 软件：
  `http://www.7-zip.org`
- 对于 Mac OS X，您可以下载并安装 The Unarchiver，这是一款免费实用程序，可在 `http://unarchiver.c3.cx` 获取

安装软件后，依次解包每个文件。解压后的文件非常大，因此请确保您有足够的磁盘空间来容纳它们。

> > 在我的系统上 right now，比较压缩和未压缩的文件大小显示，未压缩版本比压缩版本大约大十倍。这些文件也需要几分钟才能解档，具体取决于您正在使用的系统规格，因此请为此步骤预留时间。

### 创建 MySQL 表并加载数据

我们现在有八个 .xml 文件，每个文件都将映射到我们即将构建的数据库中的一个表。要创建数据库和表，我们可以使用 phpMyAdmin 或其他图形工具通过点击来完成，或者我们可以运行 Georgios Gousios 编写的以下简单 SQL，并可在 `https://gist.github.com/gousiosg/7600626` 获取。此代码包含前六个表的 `CREATE` 和 `LOAD INFILE` 语句，但自此脚本编写以来，数据库转储已添加了另外两个表。

要构建新的表结构，我们可以在终端窗口或 shell 中运行 `head` 命令，以检查此文件的前几行。从终端，运行它在最小的 XML 文件 `PostLinks.xml` 上，如下所示：

```
head PostLinks.xml
```

结果的前四行如下所示：

```
<?xml version="1.0" encoding="utf-8"?>
<postlinks>
  <row Id="19" CreationDate="2010-04-26T02:59:48.130" PostId="109" RelatedPostId="32412" LinkTypeId="1" />
  <row Id="37" CreationDate="2010-04-26T02:59:48.600" PostId="1970" RelatedPostId="617600" LinkTypeId="1" />
```

我们新数据库表中的每一行都应对应 XML <row> 行中的一行，并且行行中显示的每个属性代表数据库表中的一列。我们可以在 Tags.xml 文件上执行相同的 head 命令，以查看其应具有的列。以下 SQL 代码将处理另外两个表的 CREATE 语句和 LOAD 语句：

```
CREATE TABLE post_links (
  Id INT NOT NULL PRIMARY KEY,
  CreationDate DATETIME DEFAULT NULL,
  PostId INT NOT NULL,
  RelatedPostId INT NOT NULL,
  LinkTypeId INT DEFAULT NULL
);

CREATE TABLE tags (
  Id INT NOT NULL PRIMARY KEY,
  TagName VARCHAR(50) DEFAULT NULL,
  Count INT DEFAULT NULL,
  ExcerptPostId INT DEFAULT NULL,
  WikiPostId INT DEFAULT NULL
);

LOAD XML LOCAL INFILE 'PostLinks.xml'
INTO TABLE post_links
ROWS IDENTIFIED BY '<row>';

LOAD XML LOCAL INFILE 'Tags.xml'
INTO TABLE tags
ROWS IDENTIFIED BY '<row>';
```

![](img/9aa003afabe05fdb07da46ad71325de6_216_0.png)

请注意，LOAD XML 语法略有更改，以便我们可以将文件保存在本地。如果您的 .xml 文件在本地机器上而不是数据库服务器本身，只需在 LOAD XML 语句中添加 LOCAL 关键字，如前面的代码所示，您就可以引用文件的完整路径。
有关 MySQL LOAD XML 语法的更多信息，请参阅 MySQL 文档中的此处： http://dev.mysql.com/doc/refman/5.5/en/load-xml.html。
