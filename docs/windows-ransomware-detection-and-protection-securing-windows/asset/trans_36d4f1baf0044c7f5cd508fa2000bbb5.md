

撰写本书时，最新版本为 7.2，可在 https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.2 上找到。

其次，我们应在 PowerShell 引擎上启用一些额外的日志记录机制以监控可疑活动。这要求我们收集端点的事件日志，以便能够监控 PowerShell 的异常使用情况。这些日志应使用 SIEM 工具或产品（如 Microsoft Sentinel）收集到集中式日志服务中。还应注意，Defender for Endpoint 和其他 EDR 产品也能提供对 PowerShell cmdlet 和模块使用情况的洞察。

在 PowerShell 中，我们有以下日志机制：

- **引擎生命周期日志** – 这允许 PowerShell 记录设备上 PowerShell 会话的启动和终止。如果运行的 PowerShell 版本高于 5，则还会记录命令行参数。默认情况下，引擎生命周期日志已启用，可在事件查看器中的 Applications and Services Logs\Microsoft\Windows\PowerShell\Operational 日志下访问。
- **模块/管道日志** – 从 PowerShell 3.0 版本开始，可以按模块或全局方式将管道事件记录到 Windows 事件日志中。此功能可通过组策略或使用 Endpoint Manager 启用。
- **脚本块跟踪** – 从 5.0 版本开始，可以将详细信息（包括运行的代码）记录到事件日志中。此输出可提供系统所采取操作的洞察。

现在，默认情况下，PowerShell 应设置为仅允许签名脚本。然而，需要注意的是，存在大量绕过脚本执行策略的方法，因此这只是定义 PowerShell 安全性的一个小部分。

许多勒索软件攻击也利用 PowerShell 下载额外有效载荷以及使用诸如 invoke-webrequest 之类的命令。

Invoke-WebRequest 是一个 PowerShell cmdlet，允许您向网页或 Web 服务发送 HTTP 和 HTTPS 请求。它可用于下载网页内容、从 Web 服务器下载文件以及向 Web 服务器上传数据。

在勒索软件的上下文中使用时，它可用于从命令和控制服务器下载并执行恶意软件有效载荷或脚本，从而使攻击者能够远程控制受损系统。此外，它还用于从受害者网络中窃取敏感数据。

就 PowerShell 而言，NSA 和 CSI 还提供了额外的指导，可在此处找到：https://media.defense.gov/2022/Jun/22/2003021689/-1/-1/1/CSI_KEEPING_POWERSHELL_SECURITY_MEASURES_TO_USE_AND_EMBRACE_20220622.PDF。

PowerShell 日志的配置可使用组策略或使用 Microsoft Intune 进行。

在 Intune 中部署 PowerShell 设置是通过端点门户完成的，在 **Devices | Configuration profiles, Create Profile** 下，配置文件类型为 **Templates**。从那里，选择 **Administrative Templates**，然后从列表中选择 **Windows PowerShell**，如下图所示。

然后，如下图所示定义策略：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_136_0.png)

图 4.14 – Endpoint Manager 中的 PowerShell 配置

这可确保使用 * 作为通配符记录所有模块，并且 PowerShell 将监控脚本块并将执行策略设置为仅允许签名脚本。

应注意，启用仅签名脚本可能会导致许多组件停止工作，因为并非所有供应商都会对其 PowerShell 模块进行签名。

## SMB 协议

SMB 协议也曾在勒索软件攻击中使用，其中利用了协议旧版本中的漏洞来获取访问权限。

这尤其适用于 SMB v1，微软希望完全将其移除。在 Windows 11（以及从 Windows 10 版本 1709 起）中，它默认处于禁用状态，但仍可通过添加/删除功能进行安装。

然而，它仍存在于旧版本的 Windows 上；因此应将其卸载。您可以通过使用 Windows PowerShell 运行以下命令来检查该功能是否存在：

```
Get-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

此外，您可以运行以下命令从操作系统中禁用该功能：

```
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

当然，您可能拥有不支持较新版本的老旧文件服务器或 NAS 服务，但微软提供了大量文档和故障排除提示，可在此处阅读：https://docs.microsoft.com/en-us/windows-server/storage/file-server/troubleshoot/smbv1-not-installed-by-default-in-windows。

关于使用 Endpoint Manager 等服务集中禁用 SMBv1，这可通过自定义 OMA-URI 配置完成，因为我们目前没有 UI 选项。这是通过使用 MSSecurityGuide/ConfigureSMBv1ClientDriver OMA-URI（仅适用于 Windows 10 和 Windows 11 操作系统）完成的。

在 Intune 中部署自定义 OMA-URI 设置是通过端点门户完成的。在 Devices | Configuration Profiles | Create Profile 下，选择 Profile type 下的 Templates，然后选择 Custom，如下图所示。然后单击 Create：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_137_0.png)

图 4.15 – Intune 中的自定义配置设置

这将打开自定义配置文件向导，允许我们定义要添加的设置。为此配置配置文件提供一个名称，例如 SMBClientsettings，然后转到 Configuration settings 部分并单击 Add。

然后在不同的对话框下输入以下内容：

- **Name:** ConfigureSMBV1ClientDriver
- **OMA-URI:** ./Vendor/MSFT/Policy/Config/MSSecurityGuide/ConfigureSMBv1Server
- **Data type:** String
- **Value:** <enabled/>
  <data id="Pol_SecGuide_SMB1ClientDriver" value="4"/>

现在，单击 **Save | Next**。然后，我们需要定义一个分配组。这是指我们旨在应用 OMA-URI 设置的机器组。然而，禁用 SMBv1 应在基础设施内的所有机器上执行。它应应用于所有设备，可通过使用 **Add all devices** 选项完成。

这将在此策略适用的操作系统上禁用 SMBv1 客户端驱动程序。应注意，它仅适用于注册到 Endpoint Manager 的设备；未由 Endpoint Manager 管理的服务器应手动操作。这也是为了确保环境中没有依赖旧 SMB 版本的遗留设备或服务。

## LOLBAS

**LOLBAS** 是 **Living Off the Land Binaries and Scripts** 的缩写，指非恶意性质且本地于操作系统的二进制文件。通常，这些可能是 Microsoft 签名的二进制文件，例如 certutil，或诸如 WMIC 之类的功能，可用于各种攻击。

您可以在 https://lolbas-project.github.io/ 上找到 LOLBAS 二进制文件的更新列表。

挑战在于防御这些二进制文件在攻击中的被利用，因为其中许多二进制文件是操作系统的核心部分。一件重要的事情是监控这些二进制文件在我们端点上的使用情况，这可通过几种方法完成。

第一个选项是使用 Sysmon，它是 Microsoft Sysinternals 套件的一部分。Sysmon 的问题在于我们需要定义要监控哪些二进制文件，这可能需要花费大量时间。幸运的是，有人创建了一个预定义模板用于 Sysmon，可监控所有这些二进制文件，可从 https://github.com/SwiftOnSecurity/sysmon-config 下载。

此外，您需要从 https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon 下载 Sysmon，并使用以下命令与 XML 文件一起运行：

```
sysmon.exe -accepteula -i sysmonconfig-export.xml
```

