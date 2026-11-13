

Sourav Gulati, Sumit Kumar

前言作者：
Sumit Gupta
Big Data 工程总监，Sapient Global Markets

## Apache Spark 2.x 面向 Java 开发者

使用 Apache Spark 2.x 的 Java API 探索大规模数据

![](img/a77bd9f099223ea139d6d612f2b90712_0_0.png)

![](img/a77bd9f099223ea139d6d612f2b90712_0_1.png)

Sourav Gulati
Sumit Kumar

Packt

伯明翰 - 孟买

Copyright © 2017 Packt Publishing

版权所有。未经出版事先书面许可，本书任何部分不得以任何形式或通过任何手段复制、存储在检索系统中或传输，但关键性文章或评论中引用的简短段落除外。

在编写本书的过程中，已尽力确保所呈现信息的准确性。但是，本书包含的信息是按原样出售的，不提供任何明示或暗示的担保。作者、Packt Publishing 及其经销商和分销商不对由此书直接或间接造成或被指造成的任何损害承担责任。

Packt Publishing 已努力通过适当使用大写来提供书中提及的所有公司和产品的商标信息。但是，Packt Publishing 不能保证此信息的准确性。

首次出版：2017 年 7 月

制作参考：1250717

由 Packt Publishing Ltd. 出版。
Livery Place
35 Livery Street
伯明翰
B3 2PB, 英国。

ISBN 978-1-78712-649-7

www.packtpub.com

## 致谢

- **作者**: Sourav Gulati, Sumit Kumar
- **文字编辑**: Safis Editing
- **审阅者**: Prashant Verma
- **项目协调员**: Nidhi Joshi
- **委托编辑**: Amey Varangaonkar
- **校对**: Safis Editing
- **收购编辑**: Shweta Pant
- **索引编制**: Tejal Daruwale Soni
- **内容开发编辑**: Mayur Pawanikar
- **图形**: Tania Dutta
- **技术编辑**: Karan Thakkar
- **生产协调员**: Arvindkumar Gupta

## 前言

Sumit Kumar 和 Sourav Gulati 是技术布道者，在构思和实施解决方案以及处理涉及大规模、高速度数据的复杂问题方面拥有深厚经验。每次我与他们讨论任何复杂问题陈述时，他们都提供了创新且可扩展的解决方案。

我在 IT 行业拥有超过 17 年的经验，专注于构思、架构设计和实施围绕各种业务领域（如酒店、医疗保健、风险管理和保险）的各种企业级解决方案。

我认识 Sumit 和 Sourav 已有 5 年，他们是开发人员/架构师，曾与我密切合作实施各种复杂的大数据解决方案。从大学时期起，他们就倾向于探索/实现分布式系统。仿佛实现大数据系统相关解决方案还不够，他们还开始与大数据社区分享知识和经验。他们积极为各种博客和技术讲座做出贡献，绝不放过任何帮助同行技术人员的机会。

了解 Sumit 和 Sourav，我并不惊讶他们开始撰写一本关于 Spark 的书，并且我正在为他们的书写前言 - Apache Spark 2.x 面向 Java 开发者。

他们对技术的热情再次结出了你手中这本精彩的书。

这本书是 Sumit 和 Sourav 在 Spark 方面的深厚知识和广泛实施经验的结晶，用于解决涉及大量、快速且多样化数据的现实问题。

虽然关于分布式系统的书籍很多，但 Sumit 和 Sourav 的书弥补了理论与实践之间的显著差距。他们的书提供了全面、详细且创新的技术，用于利用 Spark 及其扩展/API 实施大数据解决方案。这本书对于构想企业级大数据解决方案的从业者，以及渴望使用其 Java API 掌握 Spark 及其扩展的本科生和研究生来说，都是一份宝贵资源。

本书首先介绍 Spark，然后涵盖总体架构以及 RDD、转换和分区等概念。它还详细讨论了各种 Spark 扩展，例如 Spark Streaming、MLlib、Spark SQL 和 GraphX。

每一章专门讨论一个主题，并包含一个涵盖最先进的 Java 工具和软件的示例性案例研究。每一章都是独立的，提供了极大的使用灵活性。伴随网站提供源代码和数据。这对学生和大数据架构师/开发人员来说真是一颗瑰宝，他们可以亲自实践刚学到的方法，或者通过将方法应用于现实场景来加深对方法的理解。

在阅读本书的各个章节时，我想起了 Sumit 和 Sourav 对分布式框架的热情和激情。他们以清晰且同样充满热情的方式传达了本书中描述的概念。我确信，作为读者的您，也会有同样的感受。我肯定会将此书作为我实施解决方案的个人资源，并强烈向我的同行架构师推荐。

Sumit Gupta
Big Data 工程总监，Sapient Global Markets

## 关于作者

Sourav Gulati 在软件行业拥有超过 7 年的经验。他从 Unix/Linux 和 Java 开始职业生涯，随后转向大数据和 NoSQL 领域。他参与过各种大数据项目。他最近还创办了一个名为 Technical Learning 的技术博客。除了 IT 领域，他喜欢阅读神话方面的内容。

Sumit Kumar 是一名在电信和银行业拥有行业洞察力的开发人员。在不同阶段，他曾担任 Java 和 SQL 开发人员，但他发现 shell 脚本既具有挑战性又令人满意。目前，他交付专注于批处理/近实时分析和分布式索引查询系统的大数据项目。除了 IT，他还对人类和生态问题非常感兴趣。

## 关于评审人

Prashant Verma 于 2011 年在 Ericsson 开始其 IT 职业生涯，担任电信领域的 Java 开发人员。几年 Java EE 经验后，他转入大数据领域，并参与过几乎所有流行的大数据技术，例如 Hadoop、Spark、Flume、Mongo、Cassandra 等。他也接触过 Scala。目前，他在 QA Infotech 担任首席数据工程师，致力于利用分析和机器学习解决电子学习问题。

Prashant 还曾担任 Packt 出版的《Apache Spark 2.x 面向 Java 开发者》的技术评审人。

我要感谢 Packt Publishing 给予我评审本书的机会，也感谢我的雇主和家人在我忙于编写本书期间的耐心。

## www.PacktPub.com

如需获取与本书相关的支持文件和下载，请访问 www.PacktPub.com。

您是否知道，Packt 为每本出版的图书提供电子书版本，并提供 PDF 和 ePub 文件？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书客户，您有权获得电子书副本的折扣。如需更多详情，请通过 service@packtpub.com 与我们联系。

在 www.PacktPub.com，您还可以阅读一系列免费技术文章，订阅各种免费通讯，并获取 Packt 图书和电子书的独家折扣和优惠。

![](img/a77bd9f099223ea139d6d612f2b90712_13_0.png)

https://www.packtpub.com/mapt

使用 Mapt 获取最热门的软件技能。Mapt 让您完全访问所有 Packt 图书和视频课程，以及行业领先的工具，帮助您规划个人发展并推进职业生涯。

