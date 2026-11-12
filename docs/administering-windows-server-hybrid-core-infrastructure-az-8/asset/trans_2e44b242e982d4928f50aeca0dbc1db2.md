

## VM 虚拟硬盘格式

虚拟硬盘（VHD）是物理硬盘的软件表示，就像 VM 的处理器和内存是物理计算机处理器和内存的软件表示一样。您可以创建分区、文件和文件夹，就像可以为物理磁盘创建一样。您将 VM 的操作系统以及任何应用程序和服务安装在这些 VHD 上。可以将多个 VHD（类似于物理计算机）附加到满足存储需求用例。

可使用以下工具创建和管理 VHD：

- Hyper-V 管理器
- Windows 磁盘管理
- PowerShell，使用 New-VHD cmdlet
- 磁盘管理控制台
- 命令行，使用 Diskpart 工具

为 VM 创建新 VHD 时，提供两种 VHD 格式，如下所示：

- VHD：这是旧版 Hyper-V 创建的格式，可创建最大大小为 2,040 GB 的磁盘。
- VHDX：这是较新的格式，可创建最大 64 TB 的磁盘。它支持大块大小以获得更好的性能。

可以使用 Hyper-V 管理器的“编辑虚拟硬盘向导”区域或 PowerShell 中的 Convert-VHD cmdlet 在格式之间转换磁盘。这将创建一个新磁盘并将现有磁盘的内容复制到其中。

## VM 磁盘类型

Hyper-V 提供不同的磁盘类型以满足不同需求：

- 固定大小 VHD：立即分配所有磁盘空间。
- 动态大小 VHD：创建时定义最大大小，并仅根据需要分配空间以进行增长。
- 差异 VHD：这与具有基础配置的父 VHD 链接。更改仅对差异磁盘进行，保留父级上的配置。这可能是基于同一父配置镜像的多个子磁盘配置镜像的用例。
- 传递物理磁盘：这允许您通过主机计算机直接连接到物理硬盘。

可以使用 Hyper-V 管理器的“编辑虚拟硬盘向导”区域在类型之间转换磁盘；应使用相同的磁盘格式。

在本节中，我们研究了实现和管理 Hyper-V VM；我们涵盖了代际版本、配置版本、VM 设置、磁盘格式和类型。现在，让我们来看看 VM 内存。

## VM 内存

可按需从 Hyper-V 管理器的 VM 设置的“硬件”部分配置 VM 的内存，如以下屏幕截图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_333_0.png)

图 9.8 – VM 内存设置

可以启用 VM 使用动态内存。正如其名称所示，这允许您根据需求动态调整大小并分配可用内存给 VM。其好处是 VM 不需要关机，而在没有动态内存的情况下则需要。

启用动态内存时，配置动态内存有四个方面，如下所示：

- **启动 RAM**：这是 VM 启动时拥有的内存量，无论是否启用动态内存。
- **最小 RAM**：这是根据负载将动态分配的最小级别。
- **最大 RAM**：这是根据负载将动态分配的最大级别。
- **内存缓冲区**：这确定将按内存块增长并增加到最大 RAM 的内存分配大小。它表示为配置的 RAM 的百分比。默认值为 20%，这意味着如果总内存为 1,024 MB，则将以 204 MB 的块大小增加内存。

配置 VM 内存的设置如以下屏幕截图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_334_0.png)

图 9.9 – 配置 VM 的内存

动态内存不能与 NUMA 一起使用。必须更新集成服务，因为动态内存需要这些服务来控制此功能。

## Hyper-V 集成服务

Hyper-V 集成服务为 VM 提供操作功能，也称为集成组件。这些服务（组件）允许 VM 与 Hyper-V 主机通信以进行其操作。保持这些服务（组件）更新对于确保 VM 功能和能力的平稳运行非常重要。

您需要在早于 Windows Server 2016 和 Windows 10 的 Hyper-V 主机上安装集成服务。要安装它，请从 Hyper-V 管理器连接到 VM，然后从操作菜单中选择“插入集成服务设置磁盘”。Windows Server 2016 和 Windows 10 或更新的 Hyper-V 主机不需要此操作。

提供以下集成服务：

- Hyper-V 心跳服务：报告 VM 是否正常运行
- Hyper-V 来宾关机服务：允许主机触发 VM 关机
- Hyper-V 时间同步服务：将 VM 时钟与主机同步
- Hyper-V 数据交换服务 (KVP)：在 VM 和主机之间提供元数据交换
- Hyper-V 卷阴影复制请求者：在不关闭 VM 的情况下提供 VM 的 VSS 备份
- Hyper-V 来宾服务接口：提供从主机向 VM 或从 VM 向主机复制文件的接口
- Hyper-V PowerShell 直连服务：在没有网络连接的情况下提供对 VM 的 PowerShell 访问

可以使用 Hyper-V 管理器或 PowerShell 打开和关闭集成服务。

要使用 Hyper-V 管理器，请右键单击 VM 以访问其设置，然后在“管理”部分下选择“集成服务”。

对于 PowerShell，可以使用以下 cmdlet：

```
Enable-VMIntegrationService
Disable-VMIntegrationService
```

要获取 VM 上启用的集成服务列表，请使用以下 PowerShell cmdlet：

```
Get-VMIntegrationService -VMName "VM1"
```

## 离散设备分配

离散设备分配 (DDA) 允许 VM 在可用且受支持时直接访问主机上的物理 PCIe 硬件。支持两个设备类：

- 图形适配器
- NVMe 存储设备

第 1 代和第 2 代 VM 支持 DDA。当设备附加时，以下 VM 功能受到限制且 VM 不可用：

- 动态内存
- VM 保存/还原
- 实时迁移
- 高可用性群集成员

配置 DDA 应执行以下步骤：

- 1. 使用以下 PowerShell cmdlet 关闭自动停止操作：

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

- 2. 使用设备管理器或以下 PowerShell cmdlet 禁用设备：

```
Get-PnpDevice
Disable-PnpDevice
```

- 3. 使用以下 PowerShell cmdlet 从主机分区卸载设备：

```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

- 4. 使用以下 PowerShell cmdlet 将设备分配给 VM：

```
Add-VMAssignableDevice -LocationPath $locationPath
-VMName VMName
```

如果要将设备从 VM 移除并返回到主机，可以从 PowerShell ISE 运行以下命令：

```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName
VMName
```

```
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```

然后可以在主机上重新启用设备。使用设备管理器或以下 PowerShell cmdlet 执行此操作：

```
Get-PnpDevice
Enable-PnpDevice
```

## 增强会话模式

增强会话模式允许将 *打印机、剪贴板、本地驱动器、USB 驱动器，* 等本地计算机资源重定向供来宾 VM 使用。这是通过 **虚拟机连接 (VMConnect)** 启用的。

在 VM 中使用本地资源的要求如下：

- Hyper-V 主机上必须同时启用增强会话模式策略和增强会话模式
- 运行 VMConnect 的计算机必须使用 Windows 8.1/10 和 Windows Server 2012 R2/2016
- VM 必须使用 Windows 8.1/10、Windows Server 2012 R2/2016，并启用远程桌面服务

如果在运行 *VMConnect* 的计算机上可用，则可以重定向以下本地资源：

- 驱动器
- USB 设备
- 支持的即插即用设备
- 智能卡
- 打印机
- 音频
- 显示设置
- 剪贴板

