

## 为运维而构建

为了说明这一点，我们可以看看对基本工作负载执行简单备份的典型步骤：

- 1. 暂停实例。
- 2. 分离 Cinder 卷（根卷和任何其他非根卷）。
- 3. 为每个实例创建实例快照，并将其存储在 Glance 中以便日后检索。
- 4. 使用 Cinder 备份上述所有卷，并将它们放入对象存储（Swift/Ceph）中。
- 5. 记录这些副本，因为 OpenStack 不会为你跟踪它们。
- 6. 恢复实例。

这些步骤命令密集、耗时、需要自定义脚本，而且由于包含手动文档记录步骤，因此容易出错。这些步骤也可能不符合组织现有的内部备份策略。

这种手动备份操作方式已经过时且功能有限。任何企业级备份解决方案都应做到无影响、能够进行增量备份、可针对每个工作负载进行定制，并且在工作负载分布在多个实例上时能够跨多个实例进行备份。此外，租户应能够轻松备份自己的工作负载，并在提交到存储之前进行测试，同时能够在需要时恢复和重放备份。工作负载备份软件还应提供灾难恢复组件，以便在发生灾难时，备份和复制的资源可在不同的地理位置使用，以防任何一个地点遭到破坏。

在考虑使用 OpenStack 原生快照的工作负载备份解决方案时，开源和商业选项都很少。一个开源选项是 Raksha (https://wiki.openstack.org/wiki/Raksha)，它是后来成为 TrilioVault (http://www.triliodata.com/about/) 的 OpenStack 项目版本，TrilioVault 是一个专为 OpenStack 设计的数据保护、备份和灾难恢复商业产品。Trilio Data 采用了前面提到的工作负载备份快照流程，并设计了一个灵活且可扩展的解决方案。使用 TrilioVault，快照的完整备份和增量备份都可以存储在各种存储上，包括 NFS、Swift 和其他第三方阵列。恢复也是 GUI 驱动的，简化了原本需要由一组自制脚本驱动的备份和恢复操作。TrilioVault 还提供了数据保留、保护和数据完整性等附加功能。

此外，传统的基于文件的备份解决方案可以在 OpenStack 内部运行，并可用作备份即服务（BaaS）产品。虽然我们无法涵盖所有备份即服务选项，但下表比较了当今企业客户使用的一些软件中的常见选项。这种 BaaS 模式通常由公有云和私有云提供商使用，为客户提供备份服务。这些服务的定价基于容量、带宽、客户端数量等成本模型。云备份服务与其他云服务一样，利用共享的私有云基础设施，并且对底层基础设施进行了抽象。BaaS 既可以供云内的租户使用，也可以供希望备份到云中的外部主机使用。

| 功能与常见要求 | Zmanda | Cloudberry | Commvault Simpana | Community Amanda（开源） |
|---|---|---|---|---|
| 基于文件的备份 | 是 | 是 | 是 | 是 |
| 客户端-服务器架构 | 是 | 两者均可 | 是 | 是 |
| 可用数据格式 | 原生转储和/或 GNU tar | Swift 对象存储 | 原生转储/SCSI | 原生转储和/或 GNU tar |
| 去重 | 否 | 是 | 是 | 否 |
| 远程站点容灾（DR） | 是 | 是 | 是 | 否 |
| 加密和压缩 | 是 | 是 | 是 | 是 |
| 增量备份 | 是 | 否 | 是 | 是 |
| LDAP 集成 | 是 | 变通方案 | 是 | 变通方案 |
| 许可 | 年度或多年度 | 年度、永久 | 年度 | 免费 |
| Linux 客户端 | 是 | 否 | 是 | 是 |
| MS Exchange | 是 | 是 | 是 | 变通方案 |
| MS SharePoint | 是 | 变通方案 | 是 | 变通方案 |
| MS SQL | 是 | 是 | 是 | 变通方案 |
| 操作 GUI | 是 | 是 | 是 | 否 |
| 报表 | 是 | 功能有限 | 是 | 否 |
| 恢复到不同位置 | 是 | 是 | 是 | 是 |
| 恢复到单个数据库 | 是 | 是 | 是 | 否 |
| 指定保留时间 | 是 | 是 | 是 | 是 |
| Windows 客户端 | 是 | 是 | 是 | 是 |

## 总结

在本章中，我们讨论了对于 OpenStack 云运维至关重要的主题。我们介绍了监控、规划和备份云的技术性方法；这仅仅是开始。网上有很多优秀资源可以帮助你运维 OpenStack 云；不过，我建议读者从 OpenStack 运维和架构指南入手。它们分别位于 http://docs.openstack.org/ops/ 和 http://docs.openstack.org/arch/。这些指南将提供所有必要的基础信息，帮助你在任何云之旅中成功运维。

在下一章中，你将学习如何集成现有的业务和运维流程，例如使用 Jenkins 进行身份管理集成和单元测试。

## 参考资料

- http://docs.openstack.org/arch/
- http://openstack-in-production.blogspot.com/2013/10/log-handling-and-dashboards-in-cern.html
- https://www.mirantis.com/validated-solution-integrations/fuel-plugins
- http://www.slack.com
- http://rally.readthedocs.io/en/latest/index.html
- http://developer.openstack.org/
- http://www.nagios.com
- https://github.com/rakesh-patnaik/nagios-openstack-monitoring

## 6

## 集成平台

我们合作过的一位 OpenStack 架构师喜欢说：“没有任何 OpenStack 实现是一座孤岛”。每个部署都会与其周围的传统 IT 基础设施集成，从身份管理系统到审计系统再到计费系统。OpenStack 标准化的 API 使这些集成相对简单。本章将介绍 OpenStack 实现中可用的集成点以及许多常见的集成模式。

在本章中，我们将详细探讨以下几点：

- **身份管理（IdM）** 集成
- 置备工作流
- 计量和计费

这三种不同的平台集成展示了 OpenStack 的三种主要集成模式。将 Keystone 与 Active Directory 等 IdM 系统集成是相对规范化的——正如 Cinder 设计用于与存储阵列集成一样，Keystone 设计用于与 IdM 系统集成。Keystone 与 IdM 系统之间的契约主要隐藏在项目代码中。

另外两种集成模式展示了与 OpenStack 系统进行外部交互的两种不同方式。外部置备工作流通过其 REST API 驱动 OpenStack 系统。它们要么直接与 Neutron、Cinder、Nova 和 Glance API 交互，要么利用 Heat 编排 API 在 OpenStack 部署中创建、修改和删除对象。相比之下，计量、计费和审计系统通过与系统的输出交互来工作，它们要么监听 OpenStack 消息总线上发生的事件，要么从 Event API 消费事件。

## IdM 集成

在 2014 年的一篇博客文章中，OpenStack 开发者*内森·金德（Nathan Kinder）*著名地（且令人信服地）论证了 **Keystone 不是身份认证服务**。在这篇文章中，Kinder 描述了将 `Keystone` 服务部署在 Apache HTTP 服务器之后、使用原生模块执行身份认证的部署模式。Kinder 在文中提出了两个论点：

- 大多数人在使用 OpenStack 时会使用某种外部身份认证系统
- 参考身份认证系统（SQL 插件）不具备我们从身份认证服务所期望的任何功能

我们自身的经验证明了这两个论点；我们合作的大多数组织所着手处理的第一个集成就是 IdM 服务的集成。

## OpenStack 中的身份认证与授权

了解 OpenStack 中的身份认证和授权机制是很有帮助的。每次对 OpenStack API 服务的调用都通过一个 bearer token 进行授权，该 token 由 `Keystone` 服务获取并验证。下图总结了这一交互过程：

![](img/2f752555f69992669defa8b25e6dcee9_141_0.png)

