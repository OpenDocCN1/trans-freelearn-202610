
## 摘要

在本章中，我们详细探讨了构成 Anypoint Platform 的控制平面和运行时平面。我们考虑了需要做出的一些架构决策，以及为这些决策提供依据的业务需求。

我们查看了控制平面和运行时平面的托管位置，并发现了两种控制平台访问的不同方式。我们还考虑了控制平面选项与这些选项对运行时平面的影响之间的关系。

然后我们快速浏览了 Anypoint Platform 中用于设计、管理和编目 API 的核心服务。

在下一章中，我们将了解 Catalyst 和 MuleSoft Knowledge Hub，并探讨如何利用 Catalyst 方法提高 API 项目的效率。

## 问题

1.  运行时平面有哪四种托管选项？
2.  能否说出使用 Runtime Fabric 托管运行时平面时不可用的两项功能？
3.  Anypoint Exchange 用于什么？
4.  在确定控制平面和运行时平面的托管位置时，需要考虑哪些因素？
5.  描述在 Anypoint Platform 中组织资源的最佳实践。

## 延伸阅读

*   MuleSoft 业务组文档：[https://docs.mulesoft.com/access-management/business-groups](https://docs.mulesoft.com/access-management/business-groups)

## 答案

1.  CloudHub、CloudHub 2.0、RTF 和独立服务器是托管 MuleSoft 运行时平面的当前选项。
2.  使用 RTF 时，Object Store v2 和 Anypoint DataGraph 不可用。
3.  Anypoint Exchange 作为可复用资产的目录，包含 API、连接器、加速器、DataWeave、RAML 片段等。
4.  数据法规、数据邻近性和云就绪度。
5.  创建子业务组并与这些组共享资源。为项目中包含的每个测试阶段和/或对应您的测试方法创建一个环境。

# 3. 利用 Catalyst 和 MuleSoft Knowledge Hub

如果您的组织已经拥有数千个 MuleSoft 实施项目，您从中研究了成功与失败的经验，整理了有效和无效的做法，收集了数百份文档并记录了数十个最佳实践，并将所有内容标记、打标签并组织成与产品开发相一致的类别，那么您可能想跳过本章。如果没有，MuleSoft Catalyst Knowledge Hub 中可能有一些值得挖掘的宝贵经验，能让您和您的公司更轻松地交付成功的 MuleSoft API 项目。

MuleSoft 客户和合作伙伴可以访问一项重要的 MuleSoft 资源，该资源有助于为其集成和转型计划提供加速和提升，并为项目开发带来一致性。该资源就是 Catalyst 和 Catalyst Knowledge Hub。简单来说，Catalyst 是 MuleSoft 交付成功企业集成计划的方法。

Catalyst 所描述的方法及其配套框架并非旨在取代任何项目交付方法论。相反，它指导在规划不同业务时应包含的活动类型和步骤。Catalyst Knowledge Hub 包含即用型（可复用）资产、最佳实践和交付方法，这些均源自多年来数千位 MuleSoft 客户的实施经验，且均以业务目标为中心。

在本书中，我们探讨了企业系统集成的一些历史背景，以及 MuleSoft 平台本身是如何从这一背景中发展而来的。我们还了解了平台本身的核心组件和结构。本书的大部分剩余内容侧重于平台技术细节和项目设计细节。Catalyst 有助于为架构工作提供结构和框架，本书其余部分将进一步对此进行描述。

在本章中，我们将涵盖以下主题：

*   Catalyst、其核心原则及其业务
*   利用 Catalyst Knowledge Hub
*   在 **赋能中心 (C4E)** 中寻找价值

## 探索 Catalyst、其核心原则及其业务

我们进行了一项非常不科学的调查，我询问了几位经验丰富的 MuleSoft 专业人士 Catalyst 是什么，以及它为组织提供了什么益处。尽管样本量很小，但结果很有趣。几乎所有人都认为 Catalyst 是一项重要资源。一些人认为它是项目蓝图；其他人认为它是项目方法论；我询问的少数人认为它是模板和工件的存储库，能给他们提供一个良好的起点。所有人对它的看法都不同。

参加 **MuleSoft Certified Platform Architecture (MCPA)** 课程的架构师将在很高层面上接触 Catalyst，在那里它被描述为一个*框架*和一种企业集成交付*方法论*，采用全局视角来交付业务成果、技术和组织赋能。它指向*行动指南*，讲师几乎总会指出 Catalyst Knowledge Hub 中的模板、文档、问卷和其他资产。

因此，从某种意义上说，所有受访者都在正确的轨道上。他们每个人都使用了 Catalyst 的不同部分并从中发现了价值。事实上，作为一个交付框架，Catalyst 旨在被引入并定制，以适应任何特定组织的组织方式。

但让我们更深入地了解一下 Catalyst 是什么、其基础构成、组织方式，以及为什么它可以成为架构师的有用工具。

### 什么是 Catalyst？
