

API 从根本上将架构讨论从异步消息传递转变为同步会话式集成。它将范式从数据生产转移到了消费模式。

许多较新的 `商用现成系统 (COTS)` 在设计之初就考虑到了这一点，为客户提供某种形式的集成接口，以访问其系统内的流程和数据。基于云的 `SaaS` 系统（如 Salesforce、Fiserv 和 Workday）也通过强大的 `API` 暴露其流程和数据。其他 `SaaS` 系统则构建了 `SOAP` Web 服务接口来访问其数据。

`EDI` 事务持续简化 `B2B` 合作伙伴集成。许多业务合作伙伴通过提供专用 `API` 来丰富合作伙伴体验，允许合作伙伴开发自己的解决方案，而非强迫他们使用其 Web 门户。

不幸的是，此列表未涵盖企业内数百个遗留系统。这些系统通常作为孤岛开发，未考虑与其他系统集成。能够构建安全、可靠且高效的 `API` 以释放这些遗留系统中的数据，是向企业交付现代集成架构的重要第一步。

MuleSoft 平台通过提供构建统一访问方式所需的服务和组件，支持这种集成方法，无论数据源是基于云的 `SaaS`、`B2B`、`COTS` N 层设计软件，甚至是遗留大型机软件。这些 `API` 的发布反过来又增加了其在新旧开发工作中的可重用性。对 `API` 部署的细粒度控制（包括防火墙、端口安全、证书、`SSL` 和策略），使架构师能够以安全、合规的方式共享对系统数据的访问，符合治理、协议、策略，甚至在某些情况下符合法律法规。

能够使用低代码、`无代码` 工具设计和开发这些 `API`，也有助于 IT 更高效地向业务交付集成解决方案。特别是当业务用户和 IT 团队缺乏开发复杂、容错、高性能、可扩展的应用集成解决方案所需的技能时。

已故伟大的蒂娜·特纳写过一首名为“*We don’t need another hero*”（我们不需要另一个英雄）的歌，当我问其他架构师：我们需要另一个集成平台架构吗？这便浮现在脑海中。我们将在本书的其余部分开始回答这个问题。我们将更详细地研究 MuleSoft 的平台架构。我们将深入探讨 MuleSoft 的资源、加速器、设计方法和部署策略。我们将审视平台的运维和管理，它们与设计图和代码审查一样，都是架构师的责任。

简而言之，本书的其余部分将作为您了解 MuleSoft 平台架构以及采用 `API` 方法应对企业应用集成现代挑战的个人指南。

## 摘要

本章首先提供了 MuleSoft 的概述，并探讨了什么构成了 `iPaaS`。

随后，本章回顾了集成的历史，详细阐述了架构方法如何从点对点演进到 `SOA` 和 `REST`。

接着，本章介绍了 MuleSoft 中可用的构建模块、功能和服务。简要考察了 `控制平面` 和 `运行时平面`，以及支持两者不同配置和托管选项的方案。

然后，本章审视了集成面临的当前挑战。它探讨了企业在数字化转型中遇到的问题，以及 IT 在努力保持系统运行的同时，又要演进或转型系统以满足业务不断增长的需求时所面临的制约。

最后，本章探讨了 `API` 总体上以及 MuleSoft 平台特别是如何对于交付现代集成解决方案至关重要，在支持可重用性的同时，以安全的方式释放系统数据。

在下一章中，我们将更详细地考察基础组件和底层架构。

## 问题

1.  Anypoint Platform 运行在哪两个操作平面上？

2.  运行时平面有哪四种托管选项？

3.  能否说出使用 Runtime Fabric 托管运行时平面时不可用的两项功能？

4.  `iPaaS` 与其他集成方法有何不同？与点对点？与 `RPC`？与 `SOA`？

5.  您或您的组织面临过哪些问题，使得集成成为一项挑战？

## 答案

1.  `控制平面` 和 `运行时平面` 是 Anypoint Platform 运行的两个平面。

2.  `CloudHub`、`CloudHub 2.0`、`Runtime Fabric (RTF)`、`独立服务器` 是目前托管 MuleSoft 运行时平面的选项。

3.  在 `RTF` 中运行时，`Object Store v2` 和 `Anypoint DataGraph` 不可用。

4.  `iPaaS` 是一套全面的云服务，支持集成开发和治理，连接本地应用程序和数据与基于云的流程、服务、应用程序和数据。点对点涉及一个应用程序直接深入另一个应用程序以直接收集数据。`RPC` 是一种点对点架构，涉及在另一个应用程序中调用远程调用并获取数据。`SOA` 是一种面向服务的架构，没有全球公认的标准，它通过预定义服务提供对象交互的方法。

5.  您的答案应根据您组织的经验而异。我见过拥有旧大型机系统的组织，这些系统对过夜批处理数据进行复杂处理。这当然会影响实时查看数据的能力。

## 延伸阅读

*   Gartner 术语表：[https://www.gartner.com/en/information-technology/glossary/information-platform-as-a-service-ipaas#:~:text=Integration%20Platform%20as%20a%20Service%20(iPaaS)%20is%20a%20suite%20of,individual%20or%20across%20multiple%20organizations](https://www.gartner.com/en/information-technology/glossary/information-platform-as-a-service-ipaas#:~:text=Integration%20Platform%20as%20a%20Service%20\(iPaaS\)%20is%20a%20suite%20of,individual%20or%20across%20multiple%20organizations)。

*   *Architectural Styles and the Design of Network-based Software Architectures* by Thomas Fielding [https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

*   Enterprise Integration Patterns, Addison-Wesley 2004, pg 3, Gregor Hohpe, Bobby Woolf

*   Christensen, Clayton M. The Innovator's Dilemma: When New Technologies Cause Great Firms to Fail. Boston, MA: Harvard Business School Press, 1997

# 第 2 章：平台基础组件与底层架构

MuleSoft 平台不断扩展和演进，为架构师和设计师提供越来越多的能力、特性和功能。一些变化很微妙，而另一些则引入了全新的能力。这甚至还不包括触手可用的所有新连接器。

例如，仅在过去几年中，我们就有了以下变化：

*   容器化运行时 – 基于 `Kubernetes` 的运行时引擎
*   Composer 流程 – 由 Salesforce 平台驱动的 `无代码` 集成和开发能力
*   API Community Manager – 一个用户界面驱动的门户，用于管理使用组织 `API` 的内部和外部开发人员
*   API Experience Hub – 下一代 Community Manager，集成了 `Anypoint Exchange` 和 `Salesforce Experience Cloud`
*   DataWeave 模块 – 支持使用数组、二进制、日期、数学、加密等进行开发的附加功能库

