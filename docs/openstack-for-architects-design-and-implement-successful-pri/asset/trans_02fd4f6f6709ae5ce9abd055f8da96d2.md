

正如我们在前面的输出中所看到的，我们拥有由前一个命令创建用户的审计轨迹，以及项目和资源 ID。 这些数据以及来自其他服务的所有数据组合起来，将为该用户在此云中的交易以及此云中的任何其他交易提供 7 W 审计轨迹。 可以使用 Ansible、Puppet 或 Chef 等开源工具为 Ceilometer、日志文件创建查询自动化，或者让 Ceilometer 将此事件数据发送到类似 IBM 的 QRadar http://www-03.ibm.com/software/products/en/qradar 的系统，该系统根据 IBM 的说法：

> “[A] 安全情报平台，它将传统的网络安全信息和事件管理（SIEM）和日志管理功能与网络行为异常检测（NBAD）、漏洞评估和管理、风险分析和模拟以及取证数据检查相结合。 它通过与其他产品/应用程序/服务/资产/端点集成来消耗事件、流、资产和漏洞信息以及网络拓扑（在客户端的环境中本地或在云中），使用户能够从许多不同的角度和视角查看、分析、理解和报告其环境中发生的一切。”

QRadar 作为 Ceilometer 数据的消费者与 OpenStack 集成。 然后它将平台中的 CADF 格式数据用于分析。

### 日志聚合和分析

正如我们在上一节中所看到的，我们拥有由演示命令创建用户的审计轨迹，以及项目和资源 ID。 这些数据以及来自其他服务的所有数据组合起来，将为该用户在此云中的交易以及此云中的任何其他交易提供 7 W 审计轨迹。 然而，由于我们现在拥有审计数据，我们可以开始寻找安全攻击。 其中一些攻击可能如下：

- 对 OpenStack 进行暴力攻击的实时安全警报
- 检测对虚拟机、卷或镜像的恶意或未经授权的访问
- 意外或故意的云服务中断
- 对 OpenStack 中所有用户活动的合规性报告

这些类型的攻击都可以通过日志内省来检测。 正如我们在 *第 5 章，构建以运营* 中讨论的那样，有一套 OpenStack 工具已成为日志传输、格式化和内省的事实标准。 构成这一强大组合的三个工具是 Elasticsearch（搜索模块）、Logstash（日志传输工具）和 Kibana（可视化引擎和仪表板）。 虽然我们在 *第 5 章，构建以运营* 中主要从运营角度讨论了这些工具，但这些相同的工具可用于实时检测前面列出的所有攻击。 该堆栈的主要优势如下：

- 用 Java 编写的开源搜索服务器
- 用于索引多种异构数据
- 通过索引进行实时搜索
- 输出 JSON 的 REST API Web 界面

在此领域的最佳架构实践是使用 Logstash 以分布式方式将所有包含 CADF 数据的日志文件集中到运行 Elasticsearch 和 Kibana 的安全主机上。 然后配置 Logstash 以索引高度规范化的 CADF 日志，并创建限制以在阈值被突破时发出警报。

例如，如果 CADF 日志检测到在短时间内来自同一 IP 地址的数千次 Keystone 身份验证失败事件，那么很可能是暴力攻击，并且值得向安全和网络团队发出警报。

使用 Kibana，团队可以搜索审计日志并确定攻击发生的时间有多长，是否有任何有效用户通过同一子网登录，或进行任何数量的其他分析。

还有相当多的其他 SIEM 平台可与 OpenStack 集成，并且可以摄取日志文件或 Ceilometer 馈送，其中一些列于此处：

- Symantec Security Information Manager
- Splunk
- HP/Arcsight
- Tripwire
- NetIQ
- Quest Software
- Enterprise Security Manager

虽然开源和商业解决方案都具有出色的功能集，但每个企业都需要自行评估 SIEM 解决方案。 然而，有了前面的信息，管理员可以感到有能力为 OpenStack 启用 CADF 事件日志记录，并为任何平台提供其满足企业审计要求所需的数据。

### 总结

在本章中，您学习了一些关于 OpenStack 安全的整体方法。 这不仅包括运行在 OpenStack 之上的应用程序，还包括构成平台的项目以及基础操作系统环境。 您了解到企业 OpenStack 安全不仅仅是加密、日志记录或修补，而是一种更以企业为基础的方法，其中所有解决方案必须与其他解决方案和谐工作。 有时这种互操作性可以通过配置来实现，有时则需要根据安全要求添加额外的开源或商业级企业软件。 然而，一个非常重要的观点是认识到 OpenStack 并没有比其竞争平台更不安全，并且有策略和工具可用于使 OpenStack 符合大多数企业安全策略。

### 参考

- https://security.openstack.org/ossalist.html
- https://review.openstack.org/#/c/299025/
- https://wiki.openstack.org/wiki/Vulnerability_Management
- https://www.openstack.org/assets/survey/April-2016-User-Survey-Report.pdf
- http://www.pulpproject.org/
- http://libvirt.org/remote.html#Remote_certificates
- http://www-03.ibm.com/software/products/en/qradar
- https://github.com/openstack/pycadf/tree/master/etc/pycadf

## 8 结论

在我们开始撰写本书的一年半以来，OpenStack 已经发生了足够的改变，以至于我们不得不至少重写一次大部分代码示例。 我们希望我们在前几章中概述的方法将帮助云架构师在软件生态系统快速变化的情况下开发和部署使用 OpenStack 的私有云。 在本最后一章中，我们将讨论 OpenStack 中的一些新兴趋势以及如何为贵组织制定其采用基础设施即服务(Infrastructure as a Service)的路线图。

### OpenStack 中的新兴趋势

OpenStack 在其短暂的历史中演变的最有趣的事情之一是围绕核心计算、网络和存储服务出现的大量项目。 截至 OpenStack 的 Newton 版本，“大帐篷”中有近六十个项目。 这些项目可以大致分为两类——提供额外基础设施组件的项目，以及管理 OpenStack 本身的安装、配置和生命周期的项目。 这第一组项目通常模仿 Amazon Web Services 中的类似物，并提供更完整的“堆栈”服务，可用于应用程序部署。 第二组项目包含配置管理代码，例如我们在前几章中用于部署 OpenStack 的 Puppet 模块，以及由其他服务使用的通用服务和库。

### 向上迁移堆栈

一些自动化额外基础设施组件的项目已被广泛采用，以至于被纳入商业供应商的 OpenStack 发行版中。 这些包括诸如 Trove，它提供数据库即服务，以及 Sahara，它提供数据处理即服务。 这些项目利用 Nova 计算服务部署准备好的 Glance 镜像实例，为最终用户通过 OpenStack API 提供更高级别的资源，例如数据库。 然后这些资源可以像任何其他 OpenStack 资源一样通过 API 或通过 Heat 编排服务进行自动化。

Trove 是核心原始服务之外较为成熟的 OpenStack 服务之一。 Trove 允许开发人员为其应用程序配置给定数据库的实例，而无需设计或指定数据库实例的底层计算和存储。 Trove 大部分由公司 Tesora 开发，该公司为 OpenStack 的大多数主要社区和企业发行版提供经过认证和支持的 Trove 组件版本。 目前，Trove 支持配置一系列关系数据库，如 MySQL 和 Oracle，以及 NoSQL 数据库，如 MongoDB 和 Redis。

Sahara 在 OpenStack 中提供大数据即服务，主要通过自动部署 Hadoop 集群。 Sahara 允许用户创建数据处理集群，定义作业模板，然后在集群上执行这些作业。 Sahara 具有大量与外部服务的集成点，例如用于名称解析的 Designate 和用于证书管理的 Barbican。 作业可以从外部或内部 HDFS 卷或 OpenStack 中的 Swift 或 Manila 提取数据。 Sahara 目前支持 Apache Hadoop、Hortonworks 数据平台、Apache Spark、MapR 和 Cloudera Hadoop 作为集群类型。 服务支持传统的 Hadoop 和 Spark 作业。

Designate 和 Barbican 是提供扩展基础设施自动化能力以完善应用程序堆栈的服务的示例，但不提供实例。 此类别中另一个流行的项目是 Zaqar，它提供消息即服务。 这些服务对于私有云环境中的开发人员和运营人员都极具吸引力。 开发人员不再需要担心应用程序架构中基础设施组件背后的细节，而运营人员可以放心，开发人员以标准化方式使用这些组件，易于排除故障和扩展。

