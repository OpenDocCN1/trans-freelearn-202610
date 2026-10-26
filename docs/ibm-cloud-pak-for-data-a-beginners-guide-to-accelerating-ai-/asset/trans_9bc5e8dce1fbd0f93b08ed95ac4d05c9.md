
## 多云战略与 Cloud Satellite

混合多云支持是 Cloud Pak for Data 的基础支柱，而能够在任意位置（私有云或任何公有云）部署和运行的功能一直是 Cloud Pak for Data 的关键差异化优势之一。在本章中，我们将进一步了解所支持的公有云和私有云、Cloud Pak for Data 即服务的演进，以及 IBM 在第三方云上支持托管服务的战略。我们将探讨 IBM 多云支持背后的业务和技术概念，并简要介绍 IBM Cloud Satellite。这将帮助客户利用 IBM 技术制定有效的多云战略。

我们将讨论以下主要主题：

- IBM 的多云战略
- 支持的部署选项
- Cloud Pak for Data 即服务
- IBM Cloud Satellite
- 面向多云未来的数据编织

### IBM 的多云战略

IBM 是少数从第一天起就拥抱混合多云战略的技术供应商之一，这一点从 IBM 支持的部署选项中可见一斑。能够部署到任何位置是 Cloud Pak for Data 的一个关键差异化优势。虽然软件和系统部署选项最受客户欢迎，但从长远来看，Cloud Pak for Data 即服务才是 IBM 的战略方向。Cloud Pak for Data 的即服务托管版本于 2020 年在 IBM Cloud 上推出，现在通过 Cloud Satellite 支持在第三方云上运行，我们将在本章后面详细讨论。

即服务的价值主张在于，它让客户无需安装、管理或更新，即可实现数据收集、组织、分析及 AI 注入方式的现代化。本质上，你可以获得一体化数据与 AI 平台（即 Cloud Pak for Data）的所有优势，而无需承担管理基础设施或软件的额外负担。

![](img/50e1dac0750b69f2b8e8efff000eb13b_140_0.png)

图 6.1 – IBM 的混合多云战略

随着对软件即服务（SaaS）的需求不断增长，Cloud Pak for Data 即服务已成为 IBM 的战略重点。此外，IBM 推出了 Cloud Satellite，以便在本地和第三方云上交付 SaaS。其目标是让客户拥有灵活的自主选择权，使他们能够在所选的基础设施上使用软件和 SaaS，例如本地私有云（x86、Power、Z）或 IBM Cloud、AWS、Azure 等公有云。接下来，我们将介绍支持的部署选项。

### 支持的部署选项

Cloud Pak for Data 提供三种部署选项：软件、系统和即服务，让客户在选择部署和运行软件的位置以及 Cloud Pak for Data 的管理方式方面拥有很大的自主权：

- Cloud Pak for Data **软件**可运行于任何私有云或公有云，包括 AWS、Azure 和 GCP。同时支持托管式 **IBM Cloud 上的 OpenShift（ROKS）**、托管式 **AWS 上的 OpenShift（ROSA）** 和托管式 **Azure 上的 OpenShift（ARO）**。
- Cloud Pak for Data **系统**是一个真正的即插即用、一体化企业数据与 AI 平台，包含所有必需的硬件和软件组件。
- Cloud Pak for Data *即服务*通过 **IBM Cloud Satellite** 在 IBM Cloud 和 AWS 等第三方云上运行。

对于尚不能接受公有云或更倾向于非 IBM 云的企业客户，可用选项如下：

- **私有云**：Cloud Pak for Data 软件随附嵌入式 Red Hat OpenShift，使客户能够在自己的数据中心部署和运行云原生数据与 AI 平台。另外，客户也可以选择 Cloud Pak for Data 系统，这是一款预装硬件和软件的一体化设备，针对端到端数据与 AI 工作负载进行了优化。
- **第三方云**：Cloud Pak for Data 软件支持在所有主流公有云上运行，包括 AWS、Azure 和 GCP，既可以直接运行在它们的云基础设施上，也可以运行在它们的托管式 OpenShift 服务上，例如 **AWS 上的 Red Hat OpenShift（ROSA）** 或 **Azure Red Hat OpenShift（ARO）**。

### 托管式 OpenShift

Red Hat 已在所有主流云供应商（包括 IBM Cloud、AWS、Azure 和 GCP）上推出托管式 OpenShift 服务。客户可以运行自己的工作负载（包括 Cloud Pak for Data 工作负载），而无需担心 OpenShift 的维护、管理和升级，这对非技术人员来说并非易事。

IBM Cloud Pak for Data 目前可在三种主要的托管式 OpenShift 服务上使用并获得支持，分别是 IBM Red Hat ROKS、Amazon ROSA 和 Microsoft ARO。托管式 OpenShift 的主要价值主张包括四个关键点：

- **节省时间**：Red Hat 负责管理 OpenShift，让客户可以专注于数据和 AI 工作负载。此外，交付管道已预先集成用户友好的管理工具，简化了管理和运维 Cloud Pak for Data 所需的工作。
- **一致性**：基于 Red Hat OpenShift 的多云可随处运行，包括公有云和本地环境，混合支持使部署方式更灵活。此外，它让客户能够专注于一种跨越所有部署选项的 Kubernetes 发行版（即 OpenShift），并在该领域积累技能。
- **可移植性**：Red Hat OpenShift 工作负载可在任何主流云供应商上以一致的方式运行，为客户提供所期望的选择和灵活性。更重要的是，这使客户能够在需要时将工作负载从一个云供应商迁移到另一个云供应商。
- **可扩展的 Red Hat OpenShift**：无论部署在哪里，都可以通过同一个 Red Hat 账户控制版本和许可。

以下是提供商作为托管式 OpenShift 服务一部分所管理的所有项目：

- 自动预置和配置基础设施（计算、网络和存储）。
- 自动*安装和配置 OpenShift*，包括高可用性跨可用区配置。
- 自动升级所有组件（操作系统、OpenShift 组件和集群内服务）。
- 对操作系统和 OpenShift 进行安全补丁管理。
- 自动执行 OpenShift 组件和工作节点的故障恢复。
- OpenShift 配置的自动扩展。
- 自动备份 OpenShift 核心 ETCD 数据。
- 与云平台的内置集成——监控、日志记录、Key Protect、IAM、Activity Tracker、Storage、COS、Security Advisor、Service Catalog、Container Registry 和 Vulnerability Advisor。
- 内置负载均衡器、VPN、代理、网络边缘节点、私有集群和 VPC 功能。
- 内置安全性，包括镜像签名、镜像部署强制和硬件信任。
- 24/7 全天候全球**站点可靠性工程（SRE）**，用于维护环境健康并提供 OpenShift 支持。
- 全球 SRE 团队在 IBM Cloud 基础设施、Kubernetes 和 OpenShift 方面拥有丰富的经验和技能，因此可以*更快地解决问题*。
- 为你的 OpenShift 环境提供自动合规性（HIPAA、PCI、SOC2 和 ISO）。
- 一键扩容。
- 在**多可用区区域（MZR）**中自动进行多可用区部署，并与**云互联网服务（CIS）**集成以实现跨可用区流量路由。
- 自动执行操作系统性能调优和安全加固。

### AWS 快速入门

在 AWS 上，Quick Start 指的是可自动部署工作负载的模板（脚本）——此处即 Cloud Pak for Data。Quick Start 会在 3 小时或更短时间内启动、配置并运行在 AWS 上部署 Cloud Pak for Data 所需的计算、网络、存储以及所有其他相关 AWS 基础设施。对于有兴趣在 AWS 上运行 Cloud Pak for Data 的客户来说，这是一个重要的价值主张。它通过自动化节省了时间（消除了手动安装配置所需的许多步骤），并帮助客户在设计上遵循 AWS 最佳实践。

![](img/50e1dac0750b69f2b8e8efff000eb13b_143_0.png)

图 6.2 – AWS 上的 Cloud Pak for Data – 价值主张

