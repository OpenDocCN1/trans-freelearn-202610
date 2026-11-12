

## 升级现有 DC 的操作系统

DC 的底层操作系统可从 Windows Server 2012 R2 升级到 Windows Server 2022，升级过程仿佛未安装 AD DS 角色。由于其作为 DC 运行，因此没有针对其设置的任何差异、已知问题或限制。

本节结束了本章三个技能领域中的第二个：*创建 AD DS*，我们在其中查看了规划和部署信息。在下一节中，我们将查看本章三个技能领域中的最后一个：*管理 AD DS*。

## 管理 Active Directory 域服务

本节将介绍管理 AD DS 及其使用的工具。我们将查看** Active Directory 管理中心**、**远程服务器管理工具**、**Windows 管理中心**和 **PowerShell**，以及 **AD 模块**和其他附加管理工具。

**Active Directory 管理中心 (ADAC)** 是基于 PowerShell 的 GUI，适用于 Windows Server（不适用于 Windows Server Core）。

可使用此工具执行以下任务：

- 通过单个工具实例管理多个域
- 在目录中搜索对象
- 创建和管理目录对象，如用户、组、计算机和 OU
- 管理动态访问控制
- 创建和管理精细密码策略
- AD 回收站操作

此工具替代了以前通过 **Microsoft 管理控制台 (MMC)** snap-in 工具（称为 **Active Directory 用户和计算机**）提供的功能。

有关 ADAC 的详细信息，请参阅 https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/adac/active-directory-administrative-center。

现在，让我们看看可用于 Azure AD DS 托管域的管理工具。

### RSAT

RSAT 允许你通过 GUI 远程管理服务器；其中包含一组 AD DS 工具。在引入 WAC 之前，这是主要的工具控制台，我们将在下一节中介绍 WAC。

这些工具的控制台可在 Windows 10/11 和 Windows Server 上使用。在 Windows 10/11 中，这些工具现在包含在操作系统中，而非单独下载，该下载是通过 **可选功能** 设置添加的。

有关 RSAT 的详细信息，请参阅 https://docs.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/remote-server-administration-tools。

### WAC

此基于浏览器的管理工具可以下载并在本地安装于 Windows 10/11 和 Windows Server 上。也可以直接通过 Azure 门户访问，因此无需下载或本地安装，类似于 CloudShell 需要在本地安装 PowerShell。

要在本地安装 WAC，必须确保网络允许所需端口；Windows 10 独立模式的默认端口是 6516。Windows Server 网关模式为 TCP 443。两者均可更改。

有关 WAC 的详细信息，请参阅 https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/overview。

### 带 AD 模块的 PowerShell

这是使用 GUI 管理 AD DS 的替代方案。你可以通过提供一组 cmdlet 的 AD 模块使用 PowerShell 命令。

如果希望在客户端/桌面操作系统（如 Windows 10/11）上的本地 PowerShell 安装中使用该模块，则该模块是 RSAT 的一部分，你需要下载并安装它。

有关 AD 模块的详细信息，请参阅 https://docs.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps。

### MMC snap-in 工具

MMC 是包含称为 snap-in 的工具集合的 GUI 控制台。以下 snap-in 工具可用于管理 AD DS，其中大部分都不言自明：

- **Active Directory 用户和计算机** 允许你执行日常任务来管理用户、组和计算机等对象；已由 ADAC 取代，并提供附加功能
- **Active Directory 站点和服务** 允许你创建和管理站点、子网、复制和相关服务
- **Active Directory 域和信任** 允许你创建和管理域和林信任
- **Active Directory 架构 snap-in** 允许你查看和修改架构

有关 MMC 的详细信息，请参阅 https://docs.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/what-is-microsoft-management-console。

本节介绍了各种 AD DS 管理工具。在下一节中，我们将介绍一些 AD DS 的监视和故障排除工具。

## 监视和故障排除工具

在本节中，我们将介绍一些 AD DS 的监视和故障排除工具。

### 性能监视工具

Windows Server 包含用于监视性能和分析服务操作的以下本机内置工具：

- 性能监视器 – 目录复制代理 (DRA) 计数器
- 资源监视器
- 任务管理器
- 事件查看器

这些工具可以帮助你分析和识别这些系统资源的任何过度使用和耗尽情况。它们将帮助你找到由瓶颈引起的任何系统性能问题的根源和根本原因。系统一次只能受到一个瓶颈的影响；该瓶颈可能存在于 CPU、内存、磁盘或网络中。你应该逐一处理每个瓶颈，然后再处理下一个。

### Repadmin

此工具可帮助你查看服务运行状况并诊断 DC 之间的复制问题。它允许你查看复制拓扑、手动创建复制拓扑并强制执行复制。当服务器上安装了 AD DS 角色时即可使用，它也作为 AD DS 工具的一部分包含在 RSAT 工具中。

有关 Repadmin 的详细信息和语法，请参阅 https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc770963(v=ws.11)。

### dcdiag

此工具将分析 AD DS DC 的运行状况状态。当服务器上安装了 AD DS 角色时即可使用，它也作为 AD DS 工具的一部分包含在 RSAT 工具中。

有关 dcdiag 的详细信息和语法，请参阅 https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731968(v=ws.11)。

### netdom

此工具允许你管理 AD DS 信任；它还可以将计算机加入域、管理计算机帐户、查询域信息（例如哪些 DC 保持 FSMO 角色）等。当服务器上安装了 AD DS 角色时即可使用，它也作为 AD DS 工具的一部分包含在 RSAT 工具中。

有关 netdom 的详细信息和语法，请参阅 https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc772217(v=ws.11)。

在本节中，我们介绍了一些 AD DS 的监视和故障排除工具。在下一节中，我们将完成一个动手练习，以巩固本章涵盖的一些概念。

## 动手练习

为了通过实用技能支持你的学习，让我们学习如何创建本章中查看的一些服务。你将学习如何在 Windows Server 上安装 AD DS。

### 开始使用

要开始此动手练习，你需要访问运行 Windows Server 2012 Standard/Datacenter 或更高版本的物理或虚拟机。

为此练习，我们可以在 Azure 中使用嵌套虚拟化环境，这意味着不需要本地硬件。但是，为了帮助你学习和演示，我们将在 Azure 环境中使用 IaaS VM 完成此练习，在该环境中我们具有创建所需资源的适当访问权限。

你可以在 https://azure.microsoft.com/free 创建免费 Azure 帐户。此免费 Azure 帐户提供以下内容：

- 12 个月的免费服务
- 200 美元信用额度，可用于探索 Azure 30 天
- 25 多项始终免费的服务

如果将使用 Azure IaaS VM 作为 DC，则建议每个 VM 应该附加一个数据磁盘以存储 AD 数据库、日志文件和 SYSVOL。或者，也可以将它们安装在学习用途提供的默认路径上。但是，生产场景中不应这样做。

