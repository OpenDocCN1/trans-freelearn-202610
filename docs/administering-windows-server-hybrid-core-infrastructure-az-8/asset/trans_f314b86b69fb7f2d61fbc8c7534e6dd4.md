

## 为混合 Windows 服务器实施 Azure 自动化

**进程自动化** 是对频繁管理任务的自动化，这些任务可能容易出错、存在配置漂移风险，并消耗与任务交付价值不成比例的时间。在下一节中，我们将介绍此主题。

### 简介

自动化可以通过 **Azure 自动化** 服务提供，并可用于混合工作负载。

要以此方式为非 Azure 混合工作负载实施 Azure 自动化，需要一个 **Azure 自动化帐户**。Azure 自动化 是一项 Azure 云服务，负责自动化 Azure 和非 Azure 平台的配置和管理。

Azure 自动化的基础是 **运行手册**，一组在目标计算机上执行的任务；这些运行手册作为作业执行。名为 **工作线程** 的进程负责在执行运行手册时运行作业。

Azure 自动化依赖于 **混合运行手册工作线程** 功能，该功能允许直接在 Azure Arc 启用的服务器上执行运行手册。混合运行手册工作线程可以采用两种方法部署，如下：

- **基于代理 (v1)**：这会部署 **适用于 Windows 的 AMA/Log Analytics 代理**，并使用 **Log Analytics** 工作区进行报告。
- **基于扩展 (v2)**：这部署为 Azure VM 扩展：**混合运行手册工作线程 VM 扩展**。它不依赖适用于 Windows 服务器的 AMA，也不使用 **Log Analytics** 工作区进行报告。

基于扩展的 _混合运行手册工作线程_ 是获得集成和无缝体验的推荐方法。

有两种运行手册工作线程类型：**系统** 和 **用户**；仅支持 **系统** 类型用于基于扩展的 _混合运行手册工作线程_。

### Azure 自动化更新管理

Azure 自动化 包含 **更新管理**，它提供对 Azure 和非 Azure 计算机（可为虚拟或物理、位于本地、边缘位置或其他云平台）的 OS 更新管理。

**更新管理** 使用自动化帐户，并通过与 Azure Monitor 集成，将更新评估和部署结果数据存储在 Log Analytics 工作区中。必须为每个将使用 **更新管理** 的计算机部署和配置 Log Analytics 代理。注册了 **更新管理** 的计算机不能向 *多个* Log Analytics 工作区报告。可以使用连接到工作区的管理组中 SCOM 代理的系统信息更新来收集信息。

Windows 计算机必须使用 Microsoft Update 向 **Windows Server Update Services (WSUS)** 报告。

启用 **更新管理** 后，会自动向自动化帐户添加一个 *混合运行手册工作线程组*。所有直接连接到 Log Analytics 工作区的计算机都已配置 *系统混合运行手册工作线程*，以支持 **更新管理** 运行手册。

以下是 **更新管理** 支持的 Windows 操作系统：

- Windows Server 2008 R2 (*RTM 和 SP1 标准版*)
- Windows Server 2012
- Windows Server 2012 R2 (标准版/数据中心版)
- Windows Server 2016 (标准版/数据中心版，*不包括 Server Core*)
- Windows Server 2019 (标准版/数据中心版，*包括 Server Core*)

在发布时，*Windows 客户端操作系统* (Windows 7/10/11) 不支持 **更新管理**。Microsoft Endpoint Configuration Manager 是 **Azure 虚拟桌面 (AVD)** 的推荐补丁管理解决方案。

本节介绍了适用于混合服务器的 Azure 自动化，并涵盖了 Azure **更新管理** 功能。下一节将介绍适用于混合 Windows 服务器的 Microsoft Defender for Cloud。

## 为混合 Windows 服务器实施 Microsoft Defender for Cloud

Microsoft Defender for Cloud 是 Microsoft 的 **云安全态势管理 (CSPM)** 和 **云工作负载保护平台 (CWPP)** 解决方案。我们可以使用它来监控和管理本地环境中混合 Windows 服务器的安全态势。

此外，我们可以实施 Microsoft Sentinel，它为加入的混合 Windows 服务器提供云原生 SIEM 和 **安全编排、自动化和响应 (SOAR)** 解决方案。

启用 Defender for Cloud 时，需要将 Log Analytics 代理部署到计算机；代理随后将数据发送到连接的 Log Analytics 工作区。Defender for Cloud 随后可以确定计算机、应用程序、数据和网络的安全态势，并提供安全分数。

对于 Azure VM，Log Analytics 代理由 Defender for Cloud 自动配置。对于本地混合计算机，代理可以手动安装，或通过 Microsoft Endpoint Protection Manager 等解决方案自动安装。

以下是 Defender for Cloud 特定角色：

- **安全读取者**：此角色具有只读权限；可以查看建议、警报、安全策略和安全状态。

- **安全管理员**：此角色具有与 **安全读取者** 角色相同的权限，此外还可以更新安全策略并驳回警报和建议。

Microsoft Sentinel 的职责是从多个不同来源接收安全操作数据，以允许关联和响应。

加入 Microsoft Sentinel 从启用并连接安全数据源开始，有许多内置数据连接器允许数据接收。

Microsoft Sentinel 需要访问 Log Analytics 工作区，并且应该是一个独立且专用的实例，不应与 Defender for Cloud 工作区共享；这称为 *Sentinel 启用的 Log Analytics 工作区*。Sentinel 启用的 Log Analytics 工作区的数据保留期已更改为 90 天。

要启用 Microsoft Sentinel，需要以下访问权限：

- 创建 Sentinel 工作区所在的 Azure 订阅的 **参与者** 访问权限
- 与创建 Sentinel 工作区所在的同一资源组的 **参与者或读取者** 访问权限，应从订阅 **参与者角色** 访问权限继承

本节介绍了实施 Defender for Cloud 和 Microsoft Sentinel。

接下来，我们将完成动手实践部分，以巩固本章中涵盖的一些概念。

## 动手实践

为了支持您的学习并掌握一些实用技能，我们将利用本章前面部分获得的概念和理解，并将其付诸实际应用。

我们将研究以下实践：

- 实践—为 Azure VM 分配访问权限
- 实践—创建 Azure 策略分配
- 实践—设置 Microsoft Defender for Cloud
- 实践—设置 Microsoft Sentinel

### 开始使用

要开始本节，如果您还没有 Azure 订阅，可以在 https://azure.microsoft.com/free 创建免费 Azure 帐户。此免费 Azure 帐户提供以下内容：

- 12 个月免费服务
- USD $200 信用额度，用于 30 天内探索 Azure
- 25 多项始终免费的服务

让我们继续本章的实践。

### 实践 – 为 Azure VM 分配访问权限

本实践将研究如何使用 Azure 门户为 Azure VM 分配访问权限。

要进行此实践，您需要具备以下条件：

- 具有 **所有者** 或 **参与者** 订阅角色的 Azure 门户访问权限
- 用于分配访问权限的 Azure VM 资源

按以下步骤开始：

1. 登录到 Azure 门户：https://portal.azure.com。您也可以使用 Azure 桌面应用：https://portal.azure.com/App/Download。

2. 在搜索栏中键入 虚拟机；从服务列表中单击 **虚拟机**：

![](img/cf35f46a8dd5f11e58349c1d81611b75_429_0.png)

图 13.10 – 搜索虚拟机服务

3. 从 **虚拟机** 边栏中找到本实践的 VM，并单击 VM _名称_ 以打开 **虚拟机** 边栏：

