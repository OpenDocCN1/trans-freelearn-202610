

3. 这些代码合并会自动触发 CI/CD 管道。 通过 Azure DevOps 门户返回 Pipelines。 单击 electroniz_code_cicd_pipeline。 现在，单击列表中的最新条目。 您应该会看到有关自动部署的以下确认：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_461_0.png)

图 12.74 – 自动管道运行的确认

注意自动运行已正确识别、创建并部署了 Lakehouse_Database_Scripts 阶段。

> 重要
> 部署原子代码更改使我们能够更快地发现和修复故障。 CI/CD 促进了一种透明的代码集成和部署方法，已知这种方法可以提高效率并增强客户满意度。

本章就到此为止。 使用前述步骤，我们成功地使用 CI/CD 管道自动化了基础设施创建和代码交付。 我们还学习了如何在团队环境中实施 CI/CD 生命周期。 从现在开始，Electroniz 可以安全地依赖这些管道，以最小的努力和风险交付其环境中的未来代码添加和更改。

## Summary

在组织力求以更少资源做更多事情的时代，自动化正迅速获得大量关注。 随着 CI/CD 的不断发展和壮大，它正成为现代数据工程师最关键的技能之一。 在大多数情况下，数据工程师的高成本只有在他们的技能组合包含自动化时才有意义。

在许多方面，采用 CI/CD 等自动化实践被证明是救命稻草。 不仅自动化减轻了数据工程师的大量工作负担，而且通过可预测地执行重复迭代来降低成本。 除此之外，CI/CD 中内置的审批和快速失败机制可确保团队的责任感和协作性。 如果明智使用，采用自动化可以确保代码和基础设施组件的可预测和无缝交付。

这是本书的最后一章。 我必须承认，在过去的 12 章中，我们涵盖了很多内容。 我们踏上了数据之旅——这是一段充满许多曲折的旅程。 我很高兴的是，最终我们成功克服了这些障碍并顺利走向成功。 我们的旅程始于收集和存储数据，然后在湖屋的各个层中对数据进行策划和聚合。 我们还学习了如何设计、开发、监控和自动部署数据管道。 在最后，我们通过提供一些建议来涵盖数据工程师面临的一些挑战，说明如何克服这些挑战。 但总的来说，最后一切都是值得的。

每一字节的数据都有一个故事要讲，我相信我们准确、安全、高效地讲述了这个故事。

## Packt>

Packt.com

订阅我们的在线数字图书馆，即可完全访问超过 7,000 本书籍和视频，以及帮助您规划个人发展和推进职业生涯的行业领先工具。 有关更多信息，请访问我们的网站。

## Why subscribe?

- 花更少的时间学习，花更多的时间编码，使用来自 4,000 多名行业专业人士的实用电子书和视频
- 使用专为您打造的技能计划来提高学习效果
- 每月免费获得一本电子书或视频
- 完全可搜索，便于快速访问重要信息
- 复制、粘贴、打印和书签内容

您知道 Packt 提供每本出版书籍的电子书版本，文件格式为 PDF 和 ePub 吗？ 您可以在 packt.com 升级到电子书版本，如果您是印刷书籍客户，您有权享受电子书副本的折扣。 如需更多详细信息，请联系 customercare@packtpub.com。

在 www.packt.com，您还可以阅读大量免费技术文章，注册多种免费新闻通讯，并在 Packt 书籍和电子书上获得独家折扣和优惠。

## Other Books You May Enjoy

如果您喜欢这本书，您可能对 Packt 的其他书籍感兴趣：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_464_0.png)

## Data Engineering with Python

Paul Crickard

ISBN: 9781839214189

- 了解数据工程如何支持数据科学工作流程
- 发现如何从文件和数据库中提取数据，然后对其进行清理、转换和丰富
- 为处理不同文件格式以及关系型和 NoSQL 数据库配置处理器
- 了解如何实现数据管道和仪表板以可视化结果
- 使用暂存和验证在数据进入仓库之前检查数据
- 使用执行验证并处理失败的暂存区域构建实时管道
- 掌握在生产环境中部署管道的方法

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_465_0.png)

## Azure Data Engineering Cookbook

Ahmad Osama

ISBN: 9781800206557

- 使用 Azure Blob 存储存储大量非结构化数据
- 对 Cosmos Table API 执行 CRUD 操作
- 使用 Azure SQL Database 实现弹性池和业务连续性
- 使用 Azure Synapse Analytics 摄取和分析数据
- 开发 Data Factory 数据流以从多个来源提取数据
- 管理、维护和保护 Azure Data Factory 管道
- 使用 Azure Stream Analytics 和 Data Explorer 处理流数据

## Packt is searching for authors like you

如果您有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。 我们已与数千名开发人员和技术专业人士合作，就像您一样，帮助他们与全球技术社区分享他们的见解。 您可以进行一般申请，申请我们正在招聘作者的特定热门主题，或提交您自己的想法。

## Share Your Thoughts

现在您已经读完，我们很想听听您的想法！ 如果您从 Amazon 购买了本书，请点击此处直接前往本书在 Amazon 的评论页面分享您的反馈，或在您购买的网站上留下评论。

您的评论对我们和技术社区非常重要，将帮助我们确保提供优质内容。

## Index

### A

访问控制 25
访问控制列表 (ACLs) 25
ACID 事务 49, 163
聚合数据
  关于 102
  在金层中验证 268-273
聚合管道 104
Amazon Kinesis 44
Amazon Redshift Spectrum 51
Ambari 58
分析单元 71
Ansible 369
Apache Flink 40, 41, 44
Apache Hadoop 40
Apache Hive 41
Apache Kafka 44
Apache Spark 40, 66, 164, 206
Apache Spark Streaming 41, 44
Apache Storm 39, 41
应用程序编程接口 (APIs) 16, 23, 83
ARM 模板
  创建 369, 370
  使用 Azure CLI 部署 378
  使用 Azure 门户部署 373-378
  使用密钥部署 379-383
参数 370
资源 372
变量 371
人工智能 (AI) 工程师 23
授权 26
自动故障检测 63
自动暂停机制 74
Azure
  数据工程 53-55
  数据处理服务 65
  数据存储服务 60
Azure Active Directory 60
Azure Active Directory 集成 66
Azure Blob 存储
  关于 60, 66, 67, 99
  数据，组织 61
Azure CLI 378
Azure 客户端 373
Azure Cloud Shell 111
Azure Cosmos DB 64, 66
Azure Databricks 66, 67, 70, 101, 166, 206, 322
Azure Databricks SQL Analytics 68
Azure Databricks 工作区 68
Azure Data Factory
  关于 66, 67, 69, 83, 100
  103, 104, 209, 432
  数据控制活动 70
  数据移动活动 70
  数据转换活动 70
Azure Data Factory 服务，组件
  控制流 70
  数据流 70
  摄取 70
  监控 70
  调度 70
Azure Data Lake Analytics
  关于 71
  阶段 71
Azure Data Lake Storage 100, 102, 103
Azure Data Lake Storage
  Gen 2 61, 62, 66, 67, 266, 372
Azure Data Share
  关于 76, 335
  原地共享 76
  基于快照的共享 76
Azure DevOps 393, 397
Azure DevOps 组织
  创建 397-405
Azure 事件网格 111, 155
Azure 事件中心命名空间 155
Azure Event Hubs 44, 63, 67, 103
Azure HDInsight 66, 70
Azure Key Vault 216, 253, 379
Azure Kubernetes Service (AKS) 59
Azure 管理的数据工程
  服务 (PAAS) 59
Azure Monitor 66
Azure Pipelines 104, 397
Azure 门户
  关于 373
  用于部署 ARM
  模板 373-378
Azure Purview 75, 352
Azure Repos 397
Azure 资源管理器 (ARM)
  关于 59, 104
  用于基础设施部署 369
Azure 资源
  清理 194, 240, 365, 386
  准备 164-174
  为数据聚合准备
  管道 246-250
  为数据策划准备
  管道 207-215
Azure 服务管理器 (ASM) 369
Azure SQL 379
Azure Stream Analytics
  关于 72
  案例场景 72
Azure Stream Analytics 集群 72
Azure Stream Analytics 作业 72
Azure Synapse 66, 67, 101,
  102, 249, 253, 379
Azure Synapse Analytics
  关于 51, 73
  数据处理服务 73, 74
Azure Synapse Link 74
Azure Synapse 无服务器 SQL 池 265
Azure Traffic Manager 64
Azure 虚拟机 (Azure VMs)
  功能 57
B
Bash 111
批量收集 38
批量摄取管道
  构建 119-141

