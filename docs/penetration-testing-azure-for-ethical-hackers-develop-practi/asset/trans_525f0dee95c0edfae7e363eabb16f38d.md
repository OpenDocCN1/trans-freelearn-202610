

作为扩展的执行和日志记录的一部分，它们会在 VM 上存储扩展配置文件和日志。这些文件可以被解析以获取敏感信息，包括凭据。此类别的主要罪魁祸首之一是域加入扩展，它最终可能会存储用于将 VM 加入域的凭据。

> **重要说明**
>
> 注意域加入凭据。它们不仅可能记录在 Run Command 扩展日志中，而且在未正确关闭的 PowerShell ISE 会话中也可能被看到漂浮着。
>
> 作为 VM 上的默认本地管理员帐户，您可以打开 PowerShell ISE 并查看是否有任何脚本被自动恢复。作者见过许多情况，管理员会通过在 ISE 中执行的脚本将 VM 加入域，但由于关闭不当，凭据可以在下一次使用本地管理员用户登录的 ISE 会话中被恢复。

作为 VM 上的本地管理员，我们可以使用 VM 上的证书来解密由 VM 扩展存储的受保护设置。为了从扩展中提取这些数据点，我们将采取以下步骤：

- 1. 以本地管理员身份在 Windows VM 上打开一个 PowerShell 会话。
- 2. 从 MicroBurst 工具包导入 `Get-AzureVMExtensionSettings` 函数： `https://github.com/NetSPI/MicroBurst/blob/master/Misc/Get-AzureVMExtensionSettings.ps1`。
- 3. 运行 `Get-AzureVMExtensionSettings` 函数并观察结果。

运行此脚本后，您应该希望在输出中看到多个 `ProtectedSettingsDecrypted` 实例，其中包含敏感的扩展信息。如果您之前按照命令执行技术进行操作，您还应该在结果中看到之前发出的命令。

> **重要说明**
>
> 由于 Jake Karnes 在解释允许我们读取这些文件的核心问题方面做得更好，请查看他的博客，了解该问题的来源和利用方法： `https://blog.netspi.com/decrypting-azure-vm-extension-settings-with-get-azurevmextensionsettings/`。

现在我们已经介绍了提取凭据的内存和磁盘上选项，让我们来看一下使用 Azure 磁盘导出的更静态类型的分析。

### 利用磁盘导出和快照导出功能

与本地 VM 一样，Azure VM 也有虚拟磁盘附加用于数据存储。至少，Azure VM 有一个操作系统磁盘附加，但它也可以附加额外的數據磁盘。这些磁盘可能包含渗透测试人员可以在后续攻击阶段利用的有用信息。

Azure 平台有一个名为磁盘导出的功能，可用于生成临时公共共享访问签名（SAS）URL 以下载 VM 磁盘。此功能的目的在于使管理员和开发人员更轻松地将磁盘从一个 Azure 区域复制到另一个区域，例如灾难恢复的情况。此功能也可以被攻击者利用来将数据外泄到组织之外！

要从 Azure 门户使用此功能，您可以转到 Azure 中的磁盘刀片，选择一个磁盘，单击磁盘导出选项，然后单击图 5.28 中突出显示的生成 URL 按钮：

![](img/4fa338e84e0a590967a142dd798ba4e2_218_0.png)

图 5.28 – 磁盘导出的 URL 生成

需要注意的是，磁盘导出功能只能用于未附加到正在运行的 VM 的磁盘。如果磁盘附加到正在运行的 VM，我们可以先创建磁盘的快照，然后使用类似的快照导出功能（图 5.29）生成临时 SAS URL，我们可以使用它从互联网下载快照：

![](img/4fa338e84e0a590967a142dd798ba4e2_219_0.png)

图 5.29 – 快照 URL 功能

由于多种原因，这些磁盘和快照最终可能会存储在存储帐户中。当这些存储帐户中的文件被公开可用时，这会产生更大的影响。

> **重要说明**
> 下载 VHD 文件时，请记住文件的大小。来自 Azure 门户的典型 HTTP 网络下载可能难以处理大型磁盘。此外，您可能希望使用具有足够磁盘空间的 Azure VM 来复制 VHD 文件。通过使用 Azure 后台网络将文件复制到 VM 磁盘，您可能会在下载过程中节省大量时间。

如前所述，下载 Azure 虚拟磁盘可能会产生显著的带宽影响，因此在实践中可能不是最有趣的练习。在渗透测试中，当您能够访问磁盘上的所有文件和密码哈希时，您的耐心可能会得到回报。此外，我们甚至可以在 Hyper-V 中启动 VHD 文件。

需要说明的另一项是磁盘使用的加密。如果磁盘使用客户管理的密钥加密（请参阅 **Encryption** 选项卡），您将需要从密钥库获取密钥来解密它。这超出了本书的范围，但在您处理磁盘时需要记住这一点。

### 动手练习 – 使用 PowerZure 外泄 VM 磁盘

在本练习中，我们将利用 Azure VM 的磁盘导出功能来外泄未附加或保留的 VM 磁盘。以下是我们将在本练习中完成的任务：

- 1. 在您的渗透测试 VM 上打开 PowerShell。
- 2. 使用以下命令通过身份验证连接到 Azure。系统提示时，输入 `contributoruser` 帐户的凭据，该帐户是在本章前面场景设置脚本中创建的：

```
PS C:\> Connect-AzAccount
```

- 3. 在 PowerShell 控制台中，使用以下命令导入 PowerZure 模块：

```
PS C:\> cd C:\Users\$env:USERNAME\PowerZure
PS C:\> Import-Module .\PowerZure.ps1
```

以下是命令的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_220_0.png)

图 5.30 – 将 PowerZure 导入会话

- 4. 使用以下命令获取所有未附加 VM 磁盘的列表：

```
PS C:\> Get-AzDisk | Where-Object {$_.DiskState -ne "Attached"} | Select Name, DiskState, Encryption
```

以下是命令输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_221_0.png)

图 5.31 – 可供导出的磁盘

命令的输出应返回所有当前未附加到正在运行的 VM 的托管磁盘及其加密状态。在您的输出中，您应该会看到一个以 winvm02 开头的磁盘；请记下磁盘名称，因为它将在下一步中使用。

- 5. 在 PowerShell 控制台中，使用以下命令生成用于导出上一步中识别的磁盘的公开可访问 URL。用上一步中识别的磁盘名称替换 `<DISK_NAME_FROM_STEP_4>` 占位符：

```
PS C:\> Get-AzureVMDisk -DiskName <DISK_NAME_FROM_STEP_4>
```

以下是命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_221_1.png)

图 5.32 – 磁盘导出 URL 生成

Get-AzureVMDisk 是 PowerZure 模块，它利用磁盘导出功能生成下载 VM 磁盘的链接。您应该从命令输出中获得一个 URL。请记下此 URL，因为它将在下一步中使用。

- 6. 这最后一步是完全可选的。如果您想在这里节省时间、带宽和磁盘空间，您可以暂停练习，并在未来测试中遇到磁盘时记住该技术。除了对磁盘进行取证以查找文件和哈希之外，在本练习的这一点上我们对磁盘并没有太多可做的。

## 6 在 PaaS 服务上利用 Contributor 权限

在上一章中，我们探讨了如何使用 Contributor 角色来利用 Azure 资源进行以 **Infrastructure as a Service (IaaS)** 为中心的场景。我们重点关注如何利用 Contributor 角色的凭据将权限提升到 Azure RBAC 所有者角色、搜寻其他凭据以及从虚拟机外泄数据。在本章中，我们将探讨如何为 **Platform as a Service (PaaS)** 场景实现类似目标。本章的一般概念会有一些重叠，但我们将重点关注这些概念在应用于 PaaS 场景时有何不同。

