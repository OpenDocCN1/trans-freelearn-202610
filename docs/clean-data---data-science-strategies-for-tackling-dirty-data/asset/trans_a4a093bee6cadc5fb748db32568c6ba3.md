
- 其次，GitHub 有一些相当严格的文件大小限制。它将每个项目（仓库）的限制列为 1GB，每个文件的限制列为 100MB。我发布的大多数数据文件单独来看都小于该限制，但由于我每年多次发布许多时间序列文件，我将不得不为它们创建多个仓库。在这种方案下，每次我发布新文件时，都必须评估是否接近文件大小限制。这很快就会成为一个大麻烦。

简而言之，GitHub 本身建议使用网络托管解决方案来分发文件，特别是如果它们很大或以数据库为导向。如果您决定在 GitHub 上托管，请务必注意不要发布包含用户凭据的文件。这包括发布数据库系统的用户名和密码、Twitter 的身份验证密钥和密钥，或任何其他个人详细信息。由于 GitHub 本质上是一个 Git 仓库，错误将永远存在，除非仓库本身被删除。如果您发现自己确实犯了错误并将个人详细信息发布到 GitHub，您必须立即取消所有当前账户的身份验证并重新创建所有密钥和密码。

## 记录您的数据

一旦人们能够访问数据，最好是在此之前，他们就需要知道自己得到的是什么。对您来说，记录数据可能感觉像是事后才想到的，但对您的用户来说却极其重要，因为他们对数据或您对数据所做的所有处理都不熟悉。在本节中，我们将回顾一些可以添加到您的数据包中以使其更易于理解的内容。

## README 文件

简单的 README 文件在计算领域有着悠久的历史。它只是一个文本文件，随软件包一起分发，或存放在包含其他文件的目录中，其理念是用户在开始使用其余软件包或文件之前应先阅读 README 文件。README 文件会向用户提供有关软件包的重要信息，例如谁编写了它以及为什么、安装说明、已知错误以及使用文件的基本说明等。

如果您正在构建数据包，例如装满文本或 SQL 文件的压缩文件，在压缩之前快速轻松地向文件包添加一个 `README` 文件。如果您为文件制作网站或在线目录，在显眼的位置添加 `README` 文件会非常有帮助。以下截图显示了我用来分发我参与的一个项目文件的一个网络目录，名为 **FLOSSmole**。我添加了一个 `README` 目录，其中包含我希望用户首先阅读的所有文件。我在该目录名称前加上了下划线，以便它在列表中按字母顺序始终显示在顶部：

![](img/9aa003afabe05fdb07da46ad71325de6_200_0.png)

网站上的文件目录显示 README 文件位于顶部。

在 `README.txt` 文件中，我为用户提供有关文件的一般和具体说明。以下是我在该目录中为我的数据提供的 `README` 文件示例：

```
README for http://flossdata.syr.edu/data directory

What is this place?
This is a repository of flat files or data "dumps", from the FLOSSmole project.

What is FLOSSmole?
Since 2004, FLOSSmole aims to:
  --freely provide data about free, libre, and open source software (FLOSS) projects in multiple formats for anyone to download;
  --integrate donated data & scripts from other research teams;
  --provide a community for researchers to discuss public data about FLOSS development.
```

FLOSSmole 包含：涵盖 2004 年至今的数十 TB 数据，并随着来自近 10,000 次基于网络的采集操作的数据集而不断增长，每月都在增长。这包括数百万个开源项目及其开发人员的数据。

如果您使用 FLOSSmole 数据，请相应地引用：
Howison, J., Conklin, M., & Crowston, K. (2006). FLOSSmole: A collaborative repository for FLOSS research data and analyses. International Journal of Information Technology and Web Engineering, 1(3), 17-26.

本网站包含什么？
来自各种软件锻造厂和项目的平面文件，均带有日期和时间戳。我们的数据库中还有大量其他数据，这里以平面文件形式不可用。例如，来自各个项目的 IRC 日志和电子邮件。有关这些，请参阅以下内容：

- 1. 直接数据库访问。请使用此链接直接访问我们的 MySQL 数据库：http://flossmole.org/content/direct-db-access-flossmole-collection-available
- 2. FLOSSmole 网站。包括更新、可视化和示例。http://flossmole.org/

此示例 README 文件适用于整个文件目录，但您可以为每个文件或不同目录提供 README 文件。由您决定。

## 文件头

向用户传达信息的另一种有效方式，尤其是在创建文本或 SQL 命令的平面文件时，是在每个文件顶部放置一个标题，解释其格式和用法。常见做法是用类似注释的字符（如 # 或 //）为标题的每一行加上前缀。

文件头中通常包含的一些项目包括：

- 文件的名称以及其所在的包的名称
- 参与创建的人员姓名及其组织和位置
- 发布日期
- 其版本号，或查找文件早期版本的位置
- 文件的用途
- 数据最初来自何处，以及从那时到现在对数据所做的任何更改
- 文件的格式及其组织方式，例如列出字段及其含义
- 文件的使用条款或许可

以下示例显示了我其中一个数据项目分发的一个 TSV 文件的示例头。在其中，我解释了数据是什么以及如何解释文件中的每一列。我还解释了如何引用数据以及共享数据的政策。我们将在本章后面讨论许可和共享的选项：

```
# Author: Squire, M. & Gazda, R.
# License: Open Database License 1.0
# This data 2012LTinsultsLKML.tsv.txt is made available under the
# Open Database License: http://opendatacommons.org/licenses/
# odbl/1.0/.
#
# filename: 2012LTinsultsLKML.tsv.txt
# explanation: This data set is part of a larger group of data
# sets described in the paper below, and hosted on the
# FLOSSmole.org site. Contains insults gleaned from messages sent
# to the LKML mailing list by Linus Torvalds during the year 2012
#
# explanation of fields:
# date: this is the date the original email was sent
# markmail permalink: this is a permalink to the email on markmail
# (for easy reading)
# type: this is our code for what type of insult this is
# mail excerpt: this is the fragment of the email containing the
# insult(s). Ellipses (...) have been added where necessary.
#
# Please cite the paper and FLOSSmole as follows:
#
# Squire, M. & Gazda, R. (2015). FLOSS as a source for profanity
# and insults: Collecting the data. In Proceedings of 48th
# Hawai'i International Conference on System Sciences (HICSS-48).
# IEEE. Hawaii, USA. 5290-5298
#
# Howison, J., Conklin, M., & Crowston, K. (2006). FLOSSmole: A
# collaborative repository for FLOSS research data and analyses.
# International Journal of Information Technology and Web
# Engineering, 1(3), 17-26.
```

如果您预计用户会定期收集您的数据文件，您应在使用标题的注释字符时保持一致。在前面的示例中，我使用了 # 字符。原因在于您的用户可能会编写程序自动下载和解析您的数据，也许将其加载到数据库中或在程序中使用。您对注释字符的一致使用将允许用户跳过标题而不处理它们。

## 数据模型和图表

如果您分发 SQL 文件来构建数据库，或提供对数据库的实时查询访问，您可能会发现诸如 **entity-relationship diagram (ERD)** 之类的可视化图表会真正帮助您的用户。

在我的一些项目中，我喜欢同时提供表格的文本描述，例如前面描述的标题和 README 文件，以及表格及其之间关系的视觉图表。由于我分发的数据库非常庞大，我还会对图表进行着色，并对图表的每个部分进行注释，以指示数据库该部分内部的内容。

以下截图显示了我其中一个大型图表的高级概述。它已缩小以显示 ERD 的大小：

![](img/9aa003afabe05fdb07da46ad71325de6_203_0.png)
