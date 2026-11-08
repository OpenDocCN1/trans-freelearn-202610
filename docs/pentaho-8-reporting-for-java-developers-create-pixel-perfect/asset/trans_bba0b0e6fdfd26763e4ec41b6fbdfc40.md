
```
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libserializer-8.1-
SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libserializer -
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libxml-8.1-
SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libxml -
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/legacy-charts-8.1-
SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -
DartifactId=pentaho-reporting-engine-legacy-charts -Dversion=8.1-
SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/classic-extensions-xpath-
8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -
DartifactId=pentaho-reporting-engine-classic-extensions-xpath -
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/classic-extensions-pmd-
8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -
DartifactId=pentaho-reporting-engine-classic-extensions-pmd -
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/classic-extensions-kettle-
8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -
DartifactId=pentaho-reporting-engine-classic-extensions-kettle -
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/classic-extensions-
mondrian-8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -
DartifactId=pentaho-reporting-engine-classic-extensions-mondrian -
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/classic-extensions-olap4j-
8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -
DartifactId=pentaho-reporting-engine-classic-extensions-olap4j -
Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

## 编译和运行项目

现在你的 Java 项目已更新到最新的 Pentaho Reporting 版本，并且本地 Apache Maven 仓库中包含了可用的 nightly build，你可以像往常一样构建它，从终端运行 `mvn clean package` 命令。使用此变通方法，你将能够尝试 Pentaho Reporting 的最新 nightly build，其中包含最新的增强功能和特性。

## 从社区获取帮助

作为开源项目，Pentaho Reporting 拥有一个由个人和组织组成的社区，他们通过回答问题、提供翻译、提交和修复错误、编写文档，当然还有贡献代码来参与贡献。为了确保你能找到所需资源以参与 Pentaho Reporting 社区，以下是一些在线场所和工具列表，帮助你入门。

## 提问与帮助他人

今天，Pentaho Reporting 社区中有两种主要的沟通方式。第一种也是最广泛使用的方式是 Pentaho Reporting 论坛。这些论坛位于 http://forums.pentaho.org/ 下的主分类 Pentaho Reporting 中。你可以搜索和阅读论坛讨论，或注册一个账号提问。

另一种主要沟通方式是 Pentaho 的 IRC 频道，由 Pentaho 社区成员设立。该频道位于 FreeNode IRC (https://freenode.net/) 上，频道名为 ##pentaho。如果你没有 IRC 客户端，可以使用 FreeNode WebChat (http://webchat.freenode.net) 加入频道。大量活跃的 Pentaho 社区成员可随时解答技术问题。

## 在线文档

在阅读本书时，你已经看到 Pentaho 拥有一个专门的文档网站，可在 https://help.pentaho.com/ 访问。在该文档网站上，你可以找到有关最新版本的所有可用内容，以及与之前版本相关的内容。

Pentaho 托管了一个 wiki，其中包含 Pentaho 各项目的社区文档。你可以在 http://wiki.pentaho.com 找到该 wiki，其中包含指向 Pentaho Reporting 文档的链接，以及指导特定用例的技术提示。作为一个 wiki，社区成员可以为任何 Pentaho 项目贡献自己的技术提示或文档。

Pentaho 还在 http://reporting.pentaho.org 托管了一个专门面向 Pentaho Reporting 社区的子域名，其中包含有关最新版本和下载链接的信息。此外，Pentaho 开源项目的最新版本 Javadoc 在线托管于 http://javadoc.pentaho.com。Pentaho Reporting Javadoc 可在 http://javadoc.pentaho.com/reporting/ 找到。

## 提交错误并查看待办事项

要提交错误或查看 Pentaho Reporting 的待办事项，你可以访问 Pentaho 的 JIRA 错误跟踪系统 http://jira.pentaho.com。Pentaho Reporting 被分为两个主要项目：Pentaho Reporting Engine 和 Pentaho Report Designer。每个项目都包含一个路线图链接，其中列出了 Pentaho 和开源社区正在处理的优先活动列表。

## 贡献代码

如果你想为 Pentaho Reporting 添加新功能或修复错误，Reporting Engine 和 Report Designer 的源代码很容易访问。Pentaho 在 GitHub 仓库 https://github.com/pentaho/pentaho-reporting 中维护 reporting 项目。Reporting engine 的核心源代码位于 `engine` 子文件夹中，而 Report Designer 位于 `designer` 子文件夹中。通过访问 https://github.com/pentaho/pentaho-reporting/releases，你可以使用浏览器浏览数十个版本和版本变更。

Pentaho 收到的一种贡献类型是翻译的消息包。这些消息包位于 reporting 项目中，可通过在每个项目中搜索 `messages.properties` 文件找到。例如，`engine` 项目中就有许多此类文件。

## 企业支持

Pentaho 提供 Pentaho Reporting 的企业版，其中包括直接客户支持，以及额外的功能，例如 Pentaho Business Analytics Platform 中的企业审计。如果你在企业工作并希望购买企业许可证，可以通过访问 http://www.pentaho.com 直接联系 Pentaho。订阅客户可以访问 Pentaho，其中包含有关 Pentaho 所有产品的许多文章和企业文档。Pentaho 与大量合作伙伴合作提供咨询服务，可能包括从设置你的报表服务器到编写企业所需的自定义功能等一切内容。

## 成为专家后该怎么做

你是否看到了 Pentaho Reporting 的强大之处？而且在多年之后它仍然免费提供。你能想象有多少开发人员投入了多少努力才达到这种成熟度吗？得益于像 Pentaho Reporting 这样的工具，你节省了多少宝贵的时间？你将从论坛、聊天、公开文档学习中，或从真正的专家撰写的博客文章中学到多少东西？

现在可能是你从这一切中受益的时候了，但不要忘记，一旦你成为 Pentaho Reporting 专家，就要回馈他人。

像别人帮助你一样帮助他人。这让社区变得有价值。

## 总结

在本章中，你学习了如何使用 Pentaho Report Designer 以及 Java 项目中的 Pentaho 依赖项来构建和使用 Pentaho Reporting nightly build。在本书的最后一个主题中，你了解了作为 Pentaho Reporting 的新开发者和专家，如何查找文档、资源、支持、结识社区等所有方式。

感谢你抽出时间阅读本书。我希望你现在对 Pentaho Reporting 感到自在，并对使用它尝试新项目感到兴奋。祝你愉快！
