# Java 开发者的 Apache Spark 2.x：释放 Apache Spark 的数据处理和分析能力，首选语言 Java





前言作者：
Sumit Gupta
Big Data 工程总监，Sapient Global Markets

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





## 为什么订阅？

- 对Packt出版的每一本书全文可搜索
- 复制、粘贴、打印并收藏内容
- 按需获取，可通过Web浏览器访问

## 客户反馈

感谢您购买这本Packt图书。在Packt，质量是我们编辑流程的核心。为了帮助我们改进，请在本书的Amazon页面上留下诚实的评论：https://www.amazon.com/dp/1787126498。

如果您希望加入我们的常客评论员团队，可以发送电子邮件至 customerreviews@packtpub.com。我们会赠送免费电子书和视频给常客评论员，以换取他们宝贵的反馈。请帮助我们不断改进产品！

# 1. Spark简介
## 大数据的维度
## 什么让Hadoop如此革命？
## 定义HDFS
## NameNode
## HDFS I/O
## YARN
## YARN中的应用程序提交流程处理
## MapReduce概述
## 为什么选择Apache Spark？
## RDD - Spark的头等公民
## RDD上的操作
## 惰性求值
## RDD的优势
## 探索Spark生态系统
## Spark 2.X中的新增功能
## 参考资料
## 总结

# 2. 再探Java
## 为什么在Spark中使用Java？
## 泛型
## 创建自己的泛型类型
## 接口
## 接口中的静态方法
## 接口中的默认方法
## 如果一个类实现了两个具有相同名称和签名的默认方法的接口，该怎么办？
## 匿名内部类
## Lambda表达式
- 函数式接口
- Lambda表达式语法
## 词法作用域
- 方法引用
- 理解闭包
## 流
- 生成流
- 中间操作
- 使用中间操作
- 终端操作
- 使用终端操作
- 字符串收集器
- 集合收集器
- Map收集器
- 分组
- 分区
- 匹配
- 查找元素
## 总结

# 3. 让我们开始使用Spark
## Spark入门
## Spark REPL（也称为CLI）
## 使用Spark shell进行一些基本练习
## 检查Spark版本
## 创建和过滤RDD
## RDD上的词频统计
## 查找整数RDD中所有偶数的和
## 统计文件中的单词数量
## Spark组件
## Spark Driver Web UI
## 作业
## 阶段
## 存储
## 环境
## 执行器
## SQL
## 流处理
## Spark作业配置与提交
### Spark REST APIs
## 总结

# 4. 理解Spark编程模型
## Spark你好
## 前提条件
## 常见的RDD转换操作
- Map
- Filter
- flatMap
- mapToPair
- flatMapToPair
- Union
- Intersection
- Distinct
- Cartesian
- groupByKey
- reduceByKey
- sortByKey
- Join
- CoGroup
## 常见的RDD动作操作
- isEmpty
- collect
- collectAsMap
- count
- countByKey
- countByValue
- Max
- Min
- First
- Take
- takeOrdered
- takeSample
- top
- reduce
- Fold
- aggregate
- forEach
- saveAsTextFile
- saveAsObjectFile
## RDD持久化与缓存
## 总结

# 5. 处理数据与存储
## 与外部存储系统的交互
## 与本地文件系统的交互
## 与Amazon S3的交互
## 与HDFS的交互
## 与Cassandra的交互
## 处理不同的数据格式
## 纯文本和特殊格式文本
## 处理CSV数据
## 处理JSON数据
## 处理XML数据
## 参考资料
## 总结

# 6. 集群上的Spark
## 分布式模式下的Spark应用程序
## 驱动程序
## 执行器程序
## 集群管理器
## Spark独立模式
## Spark独立集群的安装
## 启动Master
## 启动Slave
## 停止Master和Slave
## 在Spark独立集群上部署应用程序
## 客户端模式
## 集群模式
## 有用的作业配置
## 有用的集群级配置（Spark独立模式）
## Yet Another Resource Negotiator (YARN)
## YARN客户端
## YARN集群
## 有用的作业配置
## 总结

# 7. Spark编程模型 - 高级
## RDD分区
## 再分区
## Spark如何计算带有shuffle的转换操作的分区数量（宽依赖转换）
## 分区器
## Hash分区器
## Range分区器
## 自定义分区器
## 高级转换操作
- mapPartitions
- mapPartitionsWithIndex
- mapPartitionsToPair
- mapValues
- flatMapValues
- repartitionAndSortWithinPartitions
- coalesce
- foldByKey
- aggregateByKey
- combineByKey
## 高级动作操作
- 近似动作操作
- 异步动作操作
- 杂项动作操作
## 共享变量
## 广播变量
- 广播变量的属性
- 广播变量的生命周期
- 使用广播变量的Map端连接
## 累加器
## 驱动程序
## 总结

# 8. 使用Spark SQL
## SQLContext和HiveContext
## 初始化SparkSession
## 使用SparkSession读取CSV
## DataFrame和Dataset
## SchemaRDD
## DataFrame
## Dataset
## 使用编码器创建Dataset
## 使用StructType创建Dataset
## 统一的DataFrame和Dataset API
## 数据持久化
## Spark SQL操作
## 无类型Dataset操作
## 临时视图
## 全局临时视图
## Spark UDF
## Spark UDAF
## 无类型UDAF
## 类型安全的UDAF：
## Hive集成
## 表持久化
## 总结

# 9. 使用Spark Streaming进行近实时处理
## Spark Streaming简介
## 理解微批处理
## Spark Streaming作业入门
## 流来源
- fileStream
- Kafka
## 流转换操作
- 无状态转换
- 有状态转换
## 检查点
## 窗口操作
## Transform操作
## 容错性与可靠性
- 数据接收器阶段
- 文件流
- 高级流来源
- 转换阶段
- 输出阶段
## 结构化流处理
- 用例回顾
- 结构化流处理 - 编程模型
- 内置输入源和接收器
  - 输入源
  - 内置接收器
## 总结

# 10. 使用Spark MLlib进行机器学习分析
## 机器学习简介
## 机器学习概念
## 数据类型
## 机器学习工作流
## 管道
## 特征向量操作
- 特征提取器
- 特征转换器
- 特征选择器
## 总结

# 11. 学习Spark GraphX
## GraphX简介
## 属性图简介
## GraphX API入门
- 使用顶点和边RDD
- 从边开始
- EdgeTriplet
## 图操作
- mapVertices
- mapEdges
- mapTriplets
- reverse
- subgraph
- aggregateMessages
- outerJoinVertices
## 图算法
- PageRank
  - 静态PageRank
  - 动态PageRank
- 三角形计数
- 连通分量
## 总结
