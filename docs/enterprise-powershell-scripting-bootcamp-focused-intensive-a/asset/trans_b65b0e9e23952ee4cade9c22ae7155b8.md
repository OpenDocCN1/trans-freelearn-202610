
为了评估计划任务并获取运行计划任务的账户列表，您可以执行以下操作：

```
$users = @()
$schtasks = Get-ScheduledTask
foreach ($Task in $schtasks) {
    $tskUser = $Task.Principal.UserId
    switch ($Task.Principal.UserId) {
"NETWORK SERVICE" { $continue = "Skip User" }
"LOCAL SERVICE" { $continue = "Skip User" }
"SYSTEM" { $continue = "Skip User"}
"$null" { $continue = "Skip User" }
        default { $continue = "Report User" }
    }
    if ($continue -eq "Report User") {

        $users += $tskUser
    }
}
$users | Get-Unique
```

其输出如下图所示：

![](img/a27a138d6586c10902d8e86ab3384834_108_0.png)

本示例展示如何输出运行计划任务的非内置用户账户：

1. 你首先定义一个名为 `$users` 的数组。
2. 然后你利用 `Get-ScheduledTask` cmdlet，将结果放在 `$schtasks` 变量中。
3. 然后你创建一个 `foreach` 循环，遍历 `$schtasks` 变量中的每个 `$Task`。
4. 你接着使用 `$Task.Principal.UserID` 属性收集当前迭代计划任务的 `Principal.UserID`，并将其放入 `$tskUser` 变量中。
5. 然后你声明一个 `switch` 语句来评估 `$Task.Principal.UserID` 属性。最初的评估针对的是内置账户。如果账户为 `NETWORK SERVICE`、`LOCAL SERVICE`、`SYSTEM` 或 `$null`，则会将 `$continue` 设置为 `Skip User`。如果与这些内置用户都不匹配，则会使用 `default` 值，将 `$continue` 变量设置为 `Report User`。
6. 退出 `switch` 语句后，你使用 `If` 语句评估 `$continue` 变量，如果值等于 `Report User` 则继续。此步骤会将 `$tskUser` 变量添加到 `$users` 数组中。

`foreach` 循环完成后，你将为每个非内置用户列出大量用户。为了将列表精简为单个或唯一用户，你将 `$users` 变量通过管道传递给 `get-unique` cmdlet。结果是系统上唯一非内置用户的列表。

该示例运行的系统有两个非默认用户账户：
`POSHDEMO\bblawat` 和 `POSHDEMO\svcSchTasks`。

## Summary

本章中，你学习了如何在 PowerShell 中处理计划任务。你首先学习了如何在系统上获取计划任务。然后你学习了如何设置计划任务触发器、计划任务操作和计划任务设置集。你继续创建了一个计划任务对象并将其注册到系统中。你还学习了如何注销计划任务以将其从系统中移除。然后你练习了更新现有计划任务。本章以学习如何识别用于运行计划任务的任何用户或服务账户的权限而结束。

在下一章中，你将学习如何查询系统以确定磁盘统计信息，例如可用空间、磁盘利用率和驱动器类型判定。

## 7

## Determining Disk Statistics

使用 Windows 服务器扫描脚本评估的另一个重要组件是磁盘统计信息。无论你是扫描以识别配置还是扫描以替换系统，评估完整的磁盘配置都很重要。通过 PowerShell，你能够收集有关物理磁盘布局、逻辑磁盘布局以及插入系统中的可移动设备的信息。这有助于识别 USB 授权密钥设备，甚至是插入系统用于备份的外部硬盘。

Windows 服务器扫描脚本还可以让你查看连接到系统上的所有设备的磁盘大小和可用空间。这些信息在系统磁盘空间不足或驱动器大小不符合公司标准时可能很有帮助。本章将使你能够详细了解系统上的磁盘配置。

本章中，我们讨论以下主题：

- 检索物理和逻辑磁盘信息
- 确定连接到系统的磁盘类型
- 将磁盘字节值转换为兆字节和千兆字节
- 动态确定磁盘度量单位
- 创建磁盘信息脚本以查询系统上的磁盘、转换磁盘大小和可用空间、计算度量单位并识别驱动器类型

> 为了获得本章示例的最佳效果，建议使用具有多个磁盘的系统。你也可以选择插入外部硬盘或 USB 驱动器以模拟额外的存储设备。

![](img/a27a138d6586c10902d8e86ab3384834_110_0.png)

### Disk statistics

无论你是进行容量规划还是计划服务器更换，评估磁盘信息的原因有很多。PowerShell 提供了多种检索磁盘信息的方法。如果想了解系统上物理磁盘的更多信息，可以使用 `get-disk` cmdlet。`get-disk` cmdlet 提供的信息包括 Number、Friendly Name、Serial Number、HealthStatus、OperationalStatus、Total size 和 Partition Style。

![](img/a27a138d6586c10902d8e86ab3384834_111_0.png)

> 要确定 `get-disk` cmdlet 可用的所有属性，可以执行 `get-disk | get-member`。这将显示物理磁盘的所有可用方法和属性。

### Retrieving physical and logical disk information

要检索系统上物理磁盘的磁盘信息，可以执行以下命令：

```
get-disk
```

其输出如下图所示：

![](img/a27a138d6586c10902d8e86ab3384834_111_1.png)

本示例展示如何获取系统上物理磁盘的信息。当你执行 `get-disk` cmdlet 时，该 cmdlet 返回磁盘 Number、Friendly Name、Serial Number、HealthStatus、OperationalStatus、TotalSize 和 Partition Style。该示例运行的系统连接了五个逻辑设备，这在 cmdlet 的输出中有所体现。

当你想确定有关磁盘操作系统使用的更多信息时，可以利用 Windows 管理规范（WMI）。要读取逻辑磁盘信息，可以使用 `get-wmiobject` cmdlet，并将 `-class` 参数设置为 `win32_logicaldisk` 参数。当你查询 `win32_logicaldisk` 类时，可以访问 DeviceID、DriveType、ProviderName、FreeSpace、Size 和 Volume name 属性。

要查询 win32_logicaldisk 类以获取磁盘信息，可以执行以下操作：

```
Get-WmiObject -class win32_logicaldisk
```

其输出如下图所示：

![](img/a27a138d6586c10902d8e86ab3384834_112_0.png)

以下示例展示如何利用 get-wmiobject 查询 WMI 以提供磁盘信息：

1. 你首先使用 `-class` 参数设置为 win32_logicaldisk 参数的 get-wmiobject cmdlet。
2. 执行后，cmdlet 返回 DeviceID、DriveType、ProviderName、FreeSpace、Size 和 VolumeName 属性。

该示例运行的系统连接了六个设备，这在 cmdlet 的输出中有所体现。这与 get-disk cmdlet 不同，WMI 提供程序考虑了空的 CD-ROM/DVD 驱动器。

### The DriveType property

DriveType 属性表示一个数值，对应于一种驱动器类型。该值范围为 0 到 6，具体取决于连接的物理设备类型。DriveType 对应关系如下：

- 0: Unknown device type
- 1: Drive does not have a root directory
- 2: Removable disk (USB key)
- 3: Local disk (hard drive/USB hard drive/virtual drive mount)
- 4: Network drive/mapped drive
- 5: Compact disk (CD/DVD drive)
- 6: RAM drive (memory mapped drive/PE OS drive)

为了快速确定 DriveType 属性的对应关系，可以执行以下 switch 命令：

```powershell
$disks = get-wmiobject win32_logicaldisk
Foreach ($disk in $disks) {
    $driveletter = $disk.DeviceID
    switch ($disk.DriveType) {
        0 { $type = "Type Unknown." }
        1 { $type = "Doesn't have a Root Directory." }
        2 { $type = "Removable Disk (e.g. USB Key)" }
        3 { $type = "Local Disk (e.g. Hard Drive / USB hard drive / Virtual drive mount)" }
        4 { $type = "Network Drive (e.g. Mapped Drive)" }
        5 { $type = "Compact Disk (e.g. CD/DVD Drive)" }
        6 { $type = "RAM Disk (e.g. Memory Mapped Drive / PE OS Drive)" }
        default { $type = "Unable To Determine Drive Type!" }
    }
    Write-host "Drive: $driveletter | Disk Type: $type"
}
```
