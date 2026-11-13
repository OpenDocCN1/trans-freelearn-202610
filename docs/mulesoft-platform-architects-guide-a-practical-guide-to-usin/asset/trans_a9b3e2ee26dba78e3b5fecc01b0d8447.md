
- Microsoft Azure Red Hat OpenShift
- Red Hat OpenShift Dedicated
- Red Hat OpenShift on IBM Cloud
- 自管式 Red Hat OpenShift 版本（Performance Plus、OCP 和 Kubernetes Engine）

我们不会深入探讨 Runtime Fabric 作为 Red Hat OpenShift 中 Kubernetes 操作符的架构，但如果您计划安装 Red Hat OpenShift Runtime Fabric 服务，它支持上述平台。

以下是在 Red Hat OpenShift 上设置 Runtime Fabric 的高级架构：

![图 11.26 - Red Hat OpenShift 上的 Runtime Fabric](image/B19922_11_26.jpg)

图 11.26 - Red Hat OpenShift 上的 Runtime Fabric

有关 Red Hat OpenShift 上 Runtime Fabric 的更多详细信息，请参阅“延伸阅读”部分提供的链接。

# 概要

本章介绍了 Runtime Fabric 提供的功能，以及它如何与虚拟机/裸金属服务器、自管式 Kubernetes 和 Red Hat OpenShift 配合使用。我们还涵盖了各种组件，例如入口、最后一公里安全、令牌化服务、部署策略、零停机部署，以及可扩展性、容错性和高可用性等非功能性需求。在下一章中，我们将解释如何在您的数据中心设置 Mule 运行时，以便您可以运行 Mule 应用程序。

## 延伸阅读

要了解本章涵盖主题的更多信息，请查看以下资源：

- Runtime Fabric 概览：[https://docs.mulesoft.com/runtime-fabric/latest](#B19922_11.xhtml#_idTextAnchor263)
- Runtime Fabric 先决条件（裸金属/虚拟机）：[https://docs.mulesoft.com/runtime-fabric/1.13/install-prereqs](#B19922_11.xhtml#_idTextAnchor263)
- Runtime Fabric 许可证密钥安装：[https://docs.mulesoft.com/runtime-fabric/1.13/install-manual](#B19922_11.xhtml#_idTextAnchor263)
- 持久性网关：[https://docs.mulesoft.com/runtime-fabric/2.3/persistence-gateway](#B19922_11.xhtml#_idTextAnchor308)
- Red Hat OpenShift 上的 Runtime Fabric：[https://docs.mulesoft.com/runtime-fabric/latest/install-openshift](#B19922_11.xhtml#_idTextAnchor297)

# 第 12 章：部署到您自己的数据中心

在本章中，我们将讨论如何在您自己的数据中心运行、管理和部署 MuleSoft 应用程序。我们还将探讨 Mule 运行时在您自己的数据中心运行 MuleSoft 应用程序时提供的不同特性和功能，以及在本地 Mule 运行时上运行和部署应用程序的不同平台架构方法。

在本章中，我们将涵盖以下主题列表：

- 在本地设置 Mule 运行时的硬件和软件要求
- 从控制平面管理 Mule 服务器
- Anypoint Private Cloud Edition
- Anypoint 集群和服务器组
- 高可用性和灾难恢复
- Mule 运行时有哪些不同的设置方式？

## 技术要求

- 访问 Anypoint Platform – 可在此处创建 30 天免费试用账户：[https://anypoint.mulesoft.com/login/signup](https://anypoint.mulesoft.com/login/signup)
- 下载 MuleSoft Anypoint Studio（访问 [https://www.mulesoft.com/lp/dl/anypoint-mule-studio](https://www.mulesoft.com/lp/dl/anypoint-mule-studio)）

### 硬件要求

以下是在 Mule 运行时上运行 MuleSoft 应用程序的最低要求，但您可以根据延迟和性能要求进行扩展：

- 2 GHz CPU 或虚拟化环境中的 1 个虚拟 CPU
- 1 GB 内存
- 4 GB 存储空间

MuleSoft 支持 x64 和 x86 架构，但目前不支持 ARM。

### 软件要求

MuleSoft 可以在任何启用了受支持 JDK 的 **Java Runtime Engine (JRE)** 的操作系统上运行。

## 为什么选择本地 Mule 运行时？

本地 Mule 运行时被那些对隐私和安全性有高要求、不允许在其组织数据中心之外建立任何设施的组织广泛使用。在这种情况下，您可以利用本地 Mule 运行时。

使用本地 Mule 运行时，您可以完全控制运行 Mule 应用程序的运行时平面和底层基础设施。您需要完全负责管理底层基础设施、网络、负载均衡器等。

让我们了解一下选择本地 Mule 运行时时，MuleSoft 和客户各自的责任：

- 客户负责设置和配置 Mule 运行时，因为客户对本地实例拥有完全控制权。
- Mule 运行时将由 MuleSoft 提供。
- 客户负责管理和配置底层基础设施、负载均衡器、服务器安全、网络等。
- 运行时平面将由客户管理，控制平面将由 MuleSoft 管理。如果您不选择 Anypoint Platform **Private Cloud Edition (PCE)**，Anypoint Platform 可以作为控制平面。使用 Anypoint Platform PCE，您可以在自己的数据中心设置控制平面。

### 在本地 Mule 运行时中运行应用程序

以下是在本地 Mule 运行时中运行 Mule 应用程序的注意事项：

- 本地 Mule 运行时允许您在单个 Mule 运行时中运行多个应用程序。
- 您可以在同一个 Mule 运行时中部署应用程序的多个版本。
- Mule 运行时允许您使用域跨多个应用程序共享配置。
- 运行时支持集群，允许您将服务器分组在一起形成一个集群，该集群可作为一个单元运行。
- 为了在应用程序的多个实例之间对 HTTP 工作负载进行负载均衡，需要设置外部负载均衡器。
- 运行时支持服务器组以分布式模式运行应用程序，但应用程序之间完全隔离。当您想要在所有服务器上部署应用程序时，这非常有用。

让我们谈谈如何实现非功能性需求。

### 高可用性

默认情况下，Mule 运行时集群可确保高可用性。如果集群中的任何服务器因计划内停机或故障而不可用，则集群中的其他节点将继续处理现有事件或消息。我们将在本章后面进一步讨论 Anypoint 集群。

### 可扩展性

本地 Mule 运行时可以水平和垂直扩展。要实现水平扩展，可以向集群或组中添加或移除更多安装了 Mule 运行时的服务器。向现有集群或组添加更多服务器会自动将集群或组中现有服务器上可用的所有应用程序部署到新添加的服务器上。

垂直扩展可以通过向现有服务器添加更多计算能力（如内存和存储）来实现。

### 负载均衡器

为了启用 HTTP 工作负载，需要在服务器组或集群前面设置外部负载均衡器。这将确保请求在负载均衡器后面的所有服务器之间进行负载均衡。它将提高性能并为 HTTP 工作负载启用高可用性。

![图 12.1：本地 Mule 运行时 HTTP 负载均衡](image/B19922_12_1.jpg)

图 12.1：本地 Mule 运行时 HTTP 负载均衡

在图 12.1 中，我们有三台安装了 Mule 运行时的服务器，它们使用外部负载均衡器对 HTTP 工作负载进行了负载均衡。

在本节中，我们已经了解了本地 Mule 运行时的各种特性，例如高可用性、可扩展性和负载均衡。

在接下来的章节中，我们将介绍本地 Mule 运行时的各种重要概念，例如 Anypoint 集群、服务器组和域项目。

## Anypoint 集群
