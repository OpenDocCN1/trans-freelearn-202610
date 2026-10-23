

根据进入 Intel 数据文件的威胁情报源的质量，Intel 日志可以是非常明确的失陷指标（IOC），或者至少是网络中非常可疑活动的指示器。

我们可以按照以下说明将 Zeek Intel 日志添加到我们的**失陷检测**仪表板：

1. 导航到我们的**失陷检测**仪表板（**Kibana | 仪表板 | 失陷检测**），并点击仪表板屏幕左上角的**编辑**。
2. 点击**新建**按钮添加一个新组件（可视化）。
3. 选择`Data Table`可视化类型，然后搜索并添加`*:so-*`数据源（这是所有日志的数据源）。
4. 输入`event.dataset:intel`搜索词，仅筛选出通知日志。点击**刷新**。
5. 添加一个`Split Rows`数据桶，将**聚合**设置为**词条**，并选择`intel.sources.keyword`作为**字段**。
6. 将**大小**选项设置为50，然后点击**更新**。
7. 我们将添加源 IP 地址，以与 Intel 通知消息一同显示。为此，请添加另一个`Split Rows`数据桶，将**聚合**设置为**词条**，并选择`source.ip`作为**字段**。
8. 将**大小**选项设置为50，然后点击**更新**。
9. 点击**保存**，将组件保存为`Breach Detection - Intel Logs Summary`。

接下来，我们将使用 Sysmon 日志查看可疑进程。

## 可疑进程和文件创建

在 Windows 机器上，创建文件和启动进程是非常常见的操作。然而，有些指标会在创建进程或创建（保存）文件时发出恶意行为警报。其中一个指标是可执行文件启动或保存的位置。通过过滤掉这些操作的大多数常见位置，我们可以生成一个显示异常可执行文件路径（位置）的视图。

按照以下说明创建一个可视化，以帮助我们精确定位可疑的进程创建：

1. 导航到我们的**失陷检测**仪表板（**Kibana | 仪表板 | 失陷检测**），并点击仪表板屏幕左上角的**编辑**。
2. 点击**新建**按钮添加一个新组件（可视化）。
3. 选择数据表可视化类型，然后搜索并添加 Security Onion - Sysmon 数据源。
4. 添加一个拆分行数据桶，将聚合设置为词条，并选择 `process.executable.keyword` 作为字段。
5. 将大小选项设置为50，然后点击更新。
6. 我们将添加源计算机名称，以与进程事件条目一同显示。为此，请添加另一个拆分行数据桶，将聚合设置为词条，并选择 `winlog.computer.keyword` 作为字段。
7. 将大小选项设置为50，然后点击更新。

以下截图展示了具体操作：

![](img/c8ffa9959721097701584fcc95101362_391_0.png)

图9.54 – 练习6：可疑进程创建 – 所有日志

此时，我们已经创建了一个视图，显示环境中所有端点上创建的每个进程的位置（image_path）摘要。其中大多数是合法操作。接下来我们要过滤掉进程应该启动的所有常见位置，例如 c:\windows\system32、c:\Program Files 等。让我们按以下步骤进行：

8. 在可视化编辑页面中输入以下代码作为搜索查询：

```
NOT process.executable: "c:\\windows\\system32\\*" AND
NOT process.executable: "c:\\Program Files*" AND NOT
process.executable: "c:\\windows\\Microsoft.NET\\*" AND
NOT process.executable: "C:\\ProgramData\\Microsoft\\Windows Defender\\*" AND NOT process.executable: "c:\\windows\\syswow64\\*" AND NOT process.executable: "c:\\Windows\\SystemApps\\*" AND NOT process.executable: "c:\\windows\\WinSxS\\*" AND NOT process.executable: "c:\\windows\\servicing\\*" AND NOT process.executable: "c:\\windows\\softwaredistribution\\*"
```

这是一个不错的起始列表，列出了 Windows 可执行文件的常见启动位置。随着时间推移，你可能会遇到其他位置，可以在此处添加它们。

9. 点击**刷新**，并使用**过滤掉值**按钮排除任何其他明显已知的安全可执行文件（例如 c:\windows\explorer.exe）。这样你就可以在后续微调视图。

10. 剩余内容应当是启动自异常位置可执行文件的进程的精简输出，如下截图所示：

![](img/c8ffa9959721097701584fcc95101362_392_0.png)

图9.55 – 练习6：可疑进程创建 – 筛选后视图

从 c:\windows\temp 位置启动的进程总是可疑的，但并不总是恶意的！

11. 点击**保存**，将组件保存为 Breach Detection - Suspicious Image Paths。

接下来，我们将添加 PowerShell 脚本块日志记录，以显示我们环境中值得关注的 PowerShell 活动。

## 可疑的 PowerShell 命令

PowerShell 是管理 Windows 的一种非常方便且……嗯，强大的方式。许多攻击者在改编已知工具时，都会围绕这种脚本语言来构建。这使攻击更加隐蔽（因为无需运行外部可执行文件），同时也非常方便，因为所有现代 Windows 版本默认都安装了 PowerShell。

在练习2 – 使用 Wazuh 进行 PowerShell 脚本块日志记录中，我们将 PowerShell 日志记录功能添加到了安全监控工具包中。现在，我们将了解如何利用该功能来检测 PowerShell 中潜在危险的命令。为此，我们将添加一个仅显示可能有害命令的数据表。按照以下说明创建数据表：

1. 导航到我们的**失陷检测**仪表板（**Kibana | 仪表板 | 失陷检测**），并点击仪表板屏幕左上角的**编辑**。
2. 点击**新建**按钮添加一个新组件（可视化）。
3. 选择数据表可视化类型，然后搜索并添加 `*:so-*` 数据源。
4. 添加一个拆分行数据桶，将**聚合**设置为词条，并选择 `winlog.event_data.scriptBlockText.keyword` 作为**字段**。
5. 将**大小**选项设置为50，然后点击**更新**。
6. 我们将添加源计算机名称，以与脚本块事件条目一同显示。为此，请添加另一个拆分行数据桶，将**聚合**设置为词条，并选择 `winlog.computer.keyword` 作为**字段**。
7. 将**大小**选项设置为50，然后点击**更新**。
8. 这会生成按计算机名称分组的 PowerShell 脚本块日志的完整列表。我们只需筛选出值得关注的条目。点击**添加筛选器**按钮，将**字段**选项设置为 `winlog.event_data.scriptBlockText`，并将**运算符**选项设置为 `is one of`。
9. 现在，一次添加一个值，完成后点击**保存**：

| 术语 | 用途 |
|---|---|
| HKLM registry | - 用于检测与注册表的交互 |
| etsn | - 用于检测远程访问尝试 |
| Enter-PSSession | - 用于检测远程访问尝试 |
| Icm attempts | - 用于检测远程命令执行 |
| Invoke-Command attempts | - 用于检测远程命令执行 |
| Powersploit framework | - 用于检测 PowerShell 渗透测试框架（PowerSploit）的使用 |
| Mimikatz | - 用于检测黑客工具 Mimikatz 的使用 |
| Powercat framework | - 用于检测 Powercat 的使用 |

同样，这只是让你起步的一个列表，所以请做一些研究，找出 PowerShell 攻击中最常使用的命令，然后将它们添加到此处，如下所示：

![](img/c8ffa9959721097701584fcc95101362_394_0.png)

图9.56 – 练习6：可疑的 PowerShell 命令

10. 点击保存，将组件保存为 Breach Detection - Suspicious PowerShell Commands。

PowerShell 的启动方式也可能暴露恶意意图或预谋的滥用行为。下表总结了一些可疑的 PowerShell 调用（启动）方式：

| 参数 | 变体 | 参数用途 |
|---|---|---|
| -noprofile | -nop | 跳过加载 profile.ps1 并避免日志记录 |
| -encoded | -e -en -enc | 允许用户运行经过编码的 PowerShell 脚本 |
| -ExecutionPolicy bypass | -ep bypass, -exp bypass, -exec bypass | 绕过任何执行策略 |
| -windowStyle hidden |  | 阻止创建窗口；可能产生误报 |
| -version 2 | -v 2, -version 2.0 | 强制使用 PowerShell 版本 2 |

检测 PowerShell 引擎的可疑调用可以揭示恶意意图。我们将添加一个数据表，列出包含这些可疑命令行参数的所有进程创建日志。这些数据所在的日志由我们之前在练习1 – 使用 Wazuh 添加 Sysmon 日志记录中部署的 Sysmon 日志引擎提供。按照以下步骤将数据表添加到仪表板：

1. 导航到我们的**失陷检测**仪表板（**Kibana | 仪表板 | 失陷检测**），并点击仪表板屏幕左上角的**编辑**。
2. 点击**新建**按钮添加一个新组件（可视化）。
3. 选择**数据表**可视化类型，然后搜索并添加 `Security Onion Sysmon` 数据源。
4. 添加一个 `Split Rows` 数据桶，将**聚合**设置为**词条**，并选择 `process.command_line.keyword` 作为**字段**。
5. 将**大小**选项设置为50，然后点击**更新**。
6. 我们将添加源计算机名称，以与 PowerShell 调用事件条目一同显示。为此，请添加另一个 `Split Rows` 数据桶，将**聚合**设置为**词条**，并选择 `winlog.computer.keyword` 作为**字段**。
7. 将**大小**选项设置为50，然后点击**更新**。

