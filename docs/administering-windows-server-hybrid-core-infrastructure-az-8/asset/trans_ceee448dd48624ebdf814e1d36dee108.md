

## 练习 – 使用 PowerShell 实现 Hyper-V 角色

在本练习中，我们将使用 PowerShell 实现 Hyper-V 角色。本练习将使用 Windows Server 2019 第 2 代虚拟机，并在服务器上本地安装它。

按以下步骤开始：

- 1. 使用管理员帐户登录本练习的 Windows Server，并启动以管理员身份运行的 PowerShell 会话。

- 2. 运行以下命令在本地将 Hyper-V 安装到此服务器：

![](img/cf35f46a8dd5f11e58349c1d81611b75_353_0.png)

图 9.22 – PowerShell 安装

- 3. 安装将开始：

![](img/cf35f46a8dd5f11e58349c1d81611b75_353_1.png)

图 9.23 – 正在进行安装

- 4. 服务器将重启；Hyper-V 角色现在将出现在“服务器管理器” > “仪表板”下：

![](img/cf35f46a8dd5f11e58349c1d81611b75_354_0.png)

图 9.24 – 已安装 Hyper-V 服务器角色

至此，我们已完成本练习。本练习教会了我们如何在 Windows Server 上使用 PowerShell 实现 Hyper-V 角色。在下一个练习中，我们将了解如何创建来宾虚拟机。

## 练习 – 创建来宾虚拟机

在本练习中，我们将创建来宾虚拟机。按以下步骤开始：

- 1. 使用管理员帐户登录本练习的 Windows Server 并打开 Hyper-V 管理器。
- 2. 单击“操作”窗格中的“新建”，然后单击“虚拟机...”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_355_0.png)

图 9.25 – 创建 VM

- 3. 在“新建虚拟机向导”区域，单击“下一步”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_355_1.png)

图 9.26 – 新建虚拟机向导 – 开始之前

- 4. 本练习中，我已选择所有默认选项。您可以根据需要选择每个选项。您可以阅读以下 Microsoft 文档来查看每个选项及其默认值：https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v#options-in-hyper-v-manager-new-virtual-machine-wizard.

- 5. 在“**摘要**”屏幕上，确认选择后单击“**完成**”：

### 新建虚拟机向导

### 完成新建虚拟机向导

您已成功完成新建虚拟机向导。您即将创建以下虚拟机。

说明：

- 开始之前
- 指定名称和位置
- 指定代
- 分配内存
- 配置网络
- 连接虚拟硬盘
- 安装选项
- 摘要

| 名称 | 新建虚拟机 |
|---|---|
| 代 | 第 1 代 |
| 内存 | 1024 MB |
| 网络 | 未连接 |
| 硬盘 | C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks\New Virtual Machine.vhdx |
| 操作系统 | 将在稍后安装 |

要创建虚拟机并关闭向导，请单击“完成”。

图 9.27 – 新建虚拟机向导 – 摘要

- 6. 在“**虚拟机连接**”窗口中，选择“**操作**” | “**启动**”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_357_0.png)

图 9.28 – 启动 VM

- 7. 在“虚拟机连接”窗口中，选择“操作” | “连接...”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_357_1.png)

图 9.29 – 连接到 VM

至此，我们已完成本练习。本练习教会了我们如何创建来宾虚拟机。现在，让我们总结本章。

## 摘要

本章内容有助于您发展有关使用 Hyper-V 进行虚拟化的知识和技能，并涵盖了 AZ-800 管理 Windows Server 混合核心基础结构：管理 Hyper-V 和来宾虚拟机。

首先，您了解了虚拟化以及如何实现和管理 Hyper-V，并使用受保护结构安全解决方案保护工作负载。我们以动手练习结束，帮助您进一步发展技能。

您通过所提供的信息获得了新技能，并将知识超出考试目标，为面向现实世界日常混合环境的角色做好准备。

在下一章中，您将了解如何实现和管理 Windows Server 容器。

## 进一步阅读

本节提供其他学习参考和其他考试信息的链接：

- Microsoft Certified: Windows Server Hybrid Administrator Associate: https://docs.microsoft.com/en-us/learn/certifications/windows-server-hybrid-administrator/
- Exam AZ-800: Administering Windows Server Hybrid Core Infrastructure: https://docs.microsoft.com/en-us/learn/certifications/exams/az-800
- Exam AZ-800: Study Guide: https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWKI0r
- Microsoft Learn: https://docs.microsoft.com/en-us/learn/paths/manage-virtualization-containers-hybrid-environment/

## 技能检查

通过回答以下问题，检查您在本章学到的内容：

- 1. 与传统的物理计算模型相比，虚拟化计算模型有哪些优势和价值？
- 2. 什么是 Hyper-V？
- 3. 实现 Hyper-V 的一些硬件要求是什么？
- 4. 在 Server Core 上实现 Hyper-V 而非完整 GUI 操作系统安装有哪些好处？
- 5. 支持哪些来宾操作系统类型？
- 6. 两个主要网络组件是什么？
- 7. 三种虚拟网络交换机类型是什么，它们在功能上有何不同？
- 8. 什么是嵌套虚拟化？
- 9. 嵌套虚拟化的一些要求是什么？
- 10. 列出 Hyper-V 管理器中可用的至少六个功能。
- 11. 我们可以通过哪些方式管理 Hyper-V 主机和来宾虚拟机？
- 12. 什么是 VM 代版本？
- 13. 什么是 VM 配置版本？
- 14. 第 1 代和第 2 代 VM 设置有什么区别？
- 15. 支持哪些 VHD 磁盘格式，它们有何不同？
- 16. 有哪些不同的 VHD 磁盘类型，这些不同类型的使用场景是什么？
- 17. 什么是动态内存，如何配置？
- 18. 什么是 Hyper-V 集成服务？
- 19. 什么是离散设备分配？
- 20. 增强会话模式提供哪些功能？
- 21. VM 资源组和 CPU 组有什么好处？
- 22. 三种调度器类型是什么？
- 23. 解释两种 VM 检查点类型。
- 24. 如何提供高可用性？
- 25. 解释受保护结构的组件和操作。

## 10 实现和管理 Windows Server 容器

在上一章中，我们添加了在 Windows Server 上实现和管理 Hyper-V 的技能。

本章涵盖 AZ-800 管理 Windows Server 混合核心基础结构：创建和管理容器的内容。

本章内容并非旨在涵盖容器及其创建、操作和编排的所有方面；内容范围仅限于考试技能大纲。

在本章中，我们将了解容器化的概念，介绍 Docker，并了解 Windows Server 容器。我们将以一些动手练习结束，帮助您进一步发展技能。

本章包含以下主题：

- 容器简介
- Docker 简介
- 保护 Windows Server 容器
- Windows Server 容器网络
- 创建 Windows Server 容器映像
- 管理 Windows Server 容器映像
- 动手练习

除本章列出的主题外，本章的目标是将您的知识超出考试目标，为面向现实世界日常混合环境的角色做好准备。

## 容器简介

在本节中，我们将介绍**容器**的概念，并了解它们与**虚拟机 (VM)**的关系。

作为一种概念，容器能够实现标准化且隔离的可部署计算资源单元，这些单元轻巧且可移植。

容器围绕封装构建，涉及将应用程序的代码及其依赖项打包，以便可以无缝部署到开发或生产环境中，并获得可重复、可预测且一致的结果。

容器的好处和价值在于，它们是*自包含的计算单元*，轻巧且可移植。

在下一节中，我们将了解容器与 VM 之间的关系。

## 比较容器和 VM

要了解容器模型，我们将简要将其与虚拟化 (VM) 放在一起看待。

与 VM 类似，容器是计算资源单元；它们的目标相同，即托管和执行代码。但是，VM 带有大量开销；它们非标准、体积大、资源密集、整体式、脆弱、难以移动且不敏捷。相比之下，容器可被视为*反 VM*；其特性正好相反，即它们是标准、轻巧、体积小、资源占用少、可移植、更加敏捷、响应迅速且启动时间更短。它们非常适合数字化转型场景和现代化数据中心工作负载。

使用容器交付代码执行的方法几乎被认为更接近应用程序交付模型，而不是虚拟化模型。

这引出了下一个思考领域，即容器通过虚拟化软件资源（即操作系统）来交付代码执行。这与虚拟化形成鲜明对比，在虚拟化中，VM 虚拟化硬件资源。

在以下图表中，我们可以将容器视为与 VM 成比例的计算单元，并将物理服务器视为计算单元：

