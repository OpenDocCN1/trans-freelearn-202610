
# MuleSoft Government Cloud 概述

## Anypoint VPC 和 VPN

**Anypoint VPC**：这允许您创建一个受您控制的虚拟网络，就像 MuleSoft Government Cloud 内部的一个私有隔离区域。VPC 允许您创建一个虚拟专用网络，其中包含应用程序和 API 等资源，这些资源与 Government Cloud 中的所有其他资源完全隔离。

**Anypoint VPN**：VPN 扩展了专用网络，允许资源发送和接收数据，就如同它们直接连接到专用网络一样。Anypoint VPN 保证您的 Anypoint VPC 与本地网络之间通过公共互联网传输的所有数据的安全性和加密性。

通过使用 Anypoint VPC 将应用程序数据与公共视图隔离，增加了一层额外的安全性。Anypoint VPN 安全地将您的本地网络连接到您的 VPC，使您能够从专用网络访问信息，而无需担心数据在传输过程中被拦截。通过提供安全、私密的数据入口和出口点，Anypoint VPC 和 VPN 协同工作以提高系统安全性和隐私性。

它们提供的优势包括改进的网络性能、数据安全性以及更多的网络自定义选项。

这些工具在 Government Cloud 的背景下尤为重要，因为它们解决了政府组织固有的更高安全性和合规性要求。

## MuleSoft Government Cloud 中的专用负载均衡器

Government Cloud 环境包含一个专用负载均衡器，这是一种基于云的服务，可将传入的网络流量分发到 Government Cloud 中的多个 MuleSoft 应用程序工作器。这确保了负载在环境中均匀分布，从而促进有效的资源利用、最佳性能和更高的应用程序可用性。

以下是在 MuleSoft Government Cloud 中拥有专用负载均衡器的主要优势：

*   **性能提升**：通过均匀分发网络流量，负载均衡器有助于防止任何单个应用程序工作器成为性能瓶颈。
*   **可靠性和可用性提高**：当操作工作器出现问题时，负载均衡器会将流量重新路由到其他正常运行的服务器，以便您的应用程序保持可用。
*   **安全套接字层 (SSL) 终止**：由单独的负载均衡器提供 SSL 终止支持。这意味着负载均衡器处理与用户会话的 SSL 握手，从而为后端应用程序工作器释放资源并提高系统性能。
*   **支持扩展**：专用负载均衡器在可扩展环境中也能有效工作，随着实例投入服务或退出服务，动态地跨多个实例分发网络负载。
*   **自定义域**：您还可以配置路由规则，允许使用专用负载均衡器将流量定向到特定域或多个域。

在经常托管任务关键型应用程序的 Government Cloud 中，使用专用负载均衡服务器对于维持高可用性和性能尤为重要。

Government Cloud 中的专用负载均衡器公开两个 A 记录，以接受来自本地专用网络和公共网络的流量。其中一条记录是内部的（`internal-<lb-name>.lb-gprod-rt.anypointdns.net`），另一条是外部的（`<lb-name>.lb-gprod-rt.anypointdns.net`）。

## MuleSoft Government Cloud 控制平面

MuleSoft Government Cloud 支持作为符合 FedRAMP 标准的控制平面的 Anypoint Platform。控制平面是您设计、部署、共享和管理 API 及应用程序的地方。

控制平面包含以下组件：

*   Anypoint Design Center，用于设计 API 规范
*   Anypoint Management Center，用于管理用户、API 和集成
*   Anypoint Exchange，一个用于发布和共享 API 规范、API 和集成的市场
*   Anypoint Runtime Manager，用于将 API 和集成部署到云或数据中心

## MuleSoft Government Cloud 运行时平面

API 和 Mule 应用程序在 Anypoint Platform 的运行时平面组件上部署并可供用户使用。Mule 运行时服务器、Anypoint 连接器和支持服务是运行平面的一部分。

## MuleSoft Government Cloud 中的独立 Mule 支持

MuleSoft Government Cloud 部署允许将 Mule 运行时引擎实例部署到本地环境中（独立 Mule 实例）。独立 Mule 实例及其 MuleSoft 支持不在 FedRAMP 范围内。这意味着客户负责确保与 Salesforce 交换的所有数据符合任何适用的数据保护原则、要求和标准。请查看 MuleSoft 系统安全计划以了解 FedRAMP 范围。

需要 Mule 运行时版本 `4.1.x` 或更高版本以及运行时代理版本 `2.4.9` 或更高版本以符合 FIPS 合规性。独立 Mule 支持使联邦、州和地方政府客户能够在其私有云或本地环境中部署独立 Mule 实例，并使用 MuleSoft Government Cloud 控制平面进行管理。

以下组件不受 MuleSoft Government Cloud 支持：

*   Anypoint Analytics
*   Anypoint Code Builder
*   API Data Gateway
*   API Governance
*   Anypoint Flex Gateway
*   Anypoint Runtime Fabric
*   Anypoint Security
*   Anypoint Service Mesh

## MuleSoft Government Cloud 的部署模型

部署 Government Cloud 有两种选择。第一种选择是全托管模型，即控制平面和运行时平面都在云中。第二种选择是控制平面在云中，而运行时平面在客户的数据中心中。如果控制平面在云中，而运行时平面托管在 MuleSoft 云或客户的数据中心中，您也可以结合使用这两种选择。

### 全托管部署模型

在此部署模型中，控制平面和运行时平面将由 MuleSoft 管理。客户无需担心底层基础设施，且完全符合 FedRAMP 标准。

`Figure 13.1` 展示了全托管部署模型的架构。

![Figure 13.1: The fully managed deployment model](`image/B19922_13_1.jpg`)

Figure 13.1: 全托管部署模型

以下是全托管部署模型的优势：

*   支持高可用性、可扩展性和智能修复机制
*   99.99% 正常运行时间
*   能够管理多个 Mule 运行时版本
*   符合 FedRAMP 标准
*   容错
*   MuleSoft 负责管理运行 MuleSoft 应用程序的底层基础设施
*   应用程序隔离
*   Government Cloud 可以使用 Anypoint VPN 和 VPC 对等连接连接到本地数据中心或私有云

以下是全托管部署模型的主要局限性：

*   连接到本地数据源、服务和资源时可能会遇到延迟

### 混合部署模型

通过混合选项，用户可以在自管理环境中托管 Mule 运行时服务器和相关 API，并使用 MuleSoft 控制平面来管理 API。Mule 运行时服务器可以安装在第三方云中，例如 AWS、Microsoft Azure、GCP、OCI 或其他云基础设施提供商，也可以在物理服务器或虚拟机上运行。

当使用客户托管的运行时时，客户还负责为高可用性、故障转移、集群和负载均衡提供基础设施和框架。

`Figure 13.2` 展示了混合托管部署模型的架构。
