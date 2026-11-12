
默认情况下，Windows 10 OS Hyper-V 主机具有增强会话模式；Windows 2012 R2 和 Windows Server 2016 现已被视为不受支持。以下步骤涵盖增强会话模式：

1. 可从 Hyper-V 设置中打开此功能；在服务器设置下选择增强会话模式策略，如以下屏幕截图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_338_0.png)

图 9.10 – 启用增强会话模式策略设置

2. 接下来，在用户设置下选择增强会话模式，如以下屏幕截图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_339_0.png)

图 9.11 – 启用增强会话模式设置

打开 VMConnect 时，您将看到可以访问和使用的 VM 本地设备资源：

![](img/cf35f46a8dd5f11e58349c1d81611b75_339_1.png)

图 9.12 – 通过 VMConnect 访问本地资源

可以使用命令提示符或 PowerShell 通过以下命令更改 VMConnect 的设置：

```
VMConnect.exe <ServerName> <VMName> /edit
```

## VM 资源组

您可以使用 VM 资源组将 VM 分组，并在组级别而非单个 VM 上大规模执行编排操作。

以下是该功能的 PowerShell cmdlet：

- Add-VMGroupMember
- Get-VMGroup
- New-VMGroup
- Remove-VMGroup
- Remove-VMGroupMember
- Rename-VMGroup

此功能只能通过 PowerShell 实现，因为 Hyper-V 管理器中没有相应选项。

## CPU 组

VM CPU 组允许您轻松地在来宾 VM 之间管理和分配主机 CPU 资源。使用 CPU 组，您可以执行以下操作：

- 创建具有不同分配的 VM 组，这些分配在组内共享主机 CPU 资源。可为不同的 VM 类型创建不同的服务类别。
- 设置特定组以拥有 CPU 资源限制；这将限制组可消耗的主机 CPU 资源量。这可以强制执行该组所需的服务类别。
- 设置组的约束，使其仅在主机特定 CPU 集上运行。这可以在不同 CPU 组成员之间提供 VM 隔离。

Hyper-V 主机计算服务 (HCS) 管理 CPU 组。在此，可以使用 cpugroups.exe 命令行实用工具。

CPU 组可能非常复杂，其内容超出了本节详细说明的范围；请参阅以下有关此功能的 Microsoft 文档：[Microsoft 文档](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-cpugroups)

## Hypervisor 调度程序

Windows Server 2016 引入了新的 *虚拟处理器调度逻辑模式*。Hyper-V 主机如何管理和分配来宾虚拟处理器的工作，由这些 *模式* 或 *调度逻辑类型* 定义。

调度程序类型如下：

- 经典调度程序
- 核心调度程序
- 根调度程序

有关每种调度程序类型的更多详细信息，请参阅以下 Microsoft 文章：[Microsoft 文章](https://docs.microsoft.com/en-gb/windows-server/virtualization/hyper-v/manage/about-hyper-v-scheduler-type-selection)

从 Windows Server 2019 开始，*核心调度程序* 是最大安全态势的默认设置，以确保默认安全性。出于这些安全原因，Microsoft 建议将 Windows Server 2016 中的默认 *经典调度程序* 更改为启用 *核心调度程序*，以防范潜在的恶意 VM。

配置 *核心调度程序* 后，来宾 VM 可以选择使用 **同步多线程** (SMT)，它允许独立的执行线程共享相同的处理器资源。

可以使用以下 PowerShell 命令在来宾 VM 上启用 SMT：

```
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

调度程序通过 *hypervisorschedulertype* BCD 中的条目在主机上配置。您可以使用管理员权限从命令提示符使用以下命令更改调度程序类型：

```
bcdedit /set hypervisorschedulertype "type"
```

此处，"type" 可以是命令中输入的 *classic*、*core* 或 *root* 调度程序之一。

> **注意**
> 撰写本文时，Hyper-V 不支持 *根调度程序*，且不应为 *虚拟化场景* 配置它。

## VM 检查点

Hyper-V 中的每个来宾 VM 可以配置以下检查点之一：

- **生产检查点**：这是可恢复的 VM 时间点映像。它使用 VM 内的备份功能来创建检查点，而不是使用保存状态功能。
- **标准检查点**：这是运行中 VM 的状态、数据和硬件配置捕获。它用于测试和开发用例。

每个 VM 的检查点可以启用或禁用。请按照以下步骤启用检查点：

1. 在 **Hyper-V 管理器** 中，右键单击 VM 并单击 **设置**。
2. 在 **管理** 部分下选择 **检查点**。
3. 选择 **启用检查点**；要*禁用*它，请清除复选框。
4. 单击 **应用** 保存更改，然后单击 **确定** 完成。

要更改检查点类型，请按照以下步骤操作：

1. 在 **Hyper-V 管理器** 中，右键单击 VM 并单击 **设置**。
2. 在 **管理** 部分下选择 **检查点**。
3. 选择 **生产** 或 **标准** 检查点。
4. 可选，您可以选择在不同位置存储检查点；使用 **检查点文件位置** 部分并将其设置为 *必需*。
5. 单击 **应用** 保存更改，然后单击 **确定** 完成。

新计算机的默认值为 *生产*。

## NIC 联机

网络接口卡 (NIC) 联机是第三方硬件供应商提供的一种技术，它使用多个网络适配器来聚合带宽。如果网络适配器失败，将提供流量故障转移，以避免连接和流量流失。

但是，Microsoft 不为该硬件供应商的技术提供支持渠道，也未为实施此技术的各个硬件供应商提供配置信息；您的硬件供应商应为您的场景提供配置协助。

请参阅以下有关 Hyper-V 中 NIC 联机的官方 Microsoft 支持策略：[官方 Microsoft 支持策略](https://docs.microsoft.com/en-us/troubleshoot/windows-server/virtualization/support-policy-nic-teaming)

## Hyper-V 副本

Hyper-V 副本是 Hyper-V 角色中的内置功能。它允许您在 Hyper-V 主机之间复制 VM 以支持高可用性目标；它会创建复制到脱机 VM 的 VM 活动副本。数据可以配置为每 30 秒、5 分钟或 30 分钟同步一次，以满足您的恢复点目标 (RPO) 要求。

主机和辅助主机服务器可以位于同一物理位置，也可以通过 WAN 链路进行复制。Hyper-V 副本支持独立和群集 Hyper-V 主机。Hyper-V 主机不依赖 Active Directory。

按照以下步骤配置 Hyper-V 副本：

1. 配置 Hyper-V 主机：
   - 至少需要两个主机，每个主机上有一个或多个 VM。
   - 在主机的 *Hyper-V 设置* 中，在 **复制配置** 部分选择 **将此计算机启用为副本服务器**。
   - 为 HTTP 选择 **使用 Kerberos** 或为 HTTPS 使用 **基于证书的身份验证**。
   - 要允许副本服务器允许来自任何已验证主服务器的 VM 复制流量，请选择 **允许来自任何已验证服务器的复制**。要仅接受来自特定选定主服务器的 VM 复制流量，请选择 **允许来自指定服务器的复制**。
   - 单击“确定”完成。

2. 配置防火墙。必须允许 VM 复制流量跨网络传输；您应配置 Windows 防火墙和任何第三方防火墙。

3. 启用 VM 复制。对于要复制的每个 VM，请执行以下操作：
   - 在 **Hyper-V 管理器** 中，右键单击 VM，并通过单击 **启用复制** 打开 **启用复制向导** 区域。按需完成向导的步骤。详细步骤可在以下 Microsoft 文档中找到：[Microsoft 文档](https://docs.microsoft.com/en-gb/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)。

4. 执行故障转移。您可以运行 *测试故障转移*、*计划内故障转移* 或 *非计划内故障转移*。

Hyper-V 副本的默认配置仅保留来自主服务器的最新复制的单个恢复点。可以存储最多 *24 个每小时恢复点* 的其他恢复点。可以创建 *崩溃一致* 和 *应用一致* 恢复点。
