
- 3. 然后你使用 log 函数并传入 "Kill File Detected at $date. Terminating Script." "Y" "Y" 值。
- 4. 这会将 kill 文件终止记录到事件日志和日志文件中（如果已启用）。
- 5. 然后你再次使用 log 函数并传入 "KillFile, Kill File Detected at $date. Terminating Script" 值。
- 6. 这会在 CSV 文件中记录 kill 文件的检测。
- 7. 然后你使用 copy-item cmdlet 将 $scnresults 文件复制到 $csvunc.id 路径，并使用 -Force 参数。
- 8. 你还使用 copy-item cmdlet 将 $scanlog 文件复制到 $csvunc.id 路径，并使用 -Force 参数。
- 9. 最后，你需要使用 exit 命令退出脚本。

在定义函数后，你使用 check-kill 调用该函数。当函数进行评估时，如果文件不存在，则允许脚本继续执行下一个操作。如果文件存在，则会终止并将扫描结果和扫描日志复制到提供的 UNC/文件路径。

## 多台 Windows 服务器扫描脚本函数

在完成整个核心脚本组件的创建后，你开始为脚本定义各个扫描函数。扫描函数将评估系统的多个方面并将所有结果记录到 CSV 文件中。你将创建用于扫描磁盘、计划任务、进程、Windows 服务、软件、用户配置文件、Windows 功能以及包含字符串的文件目录的函数。你还将创建一个用于转换磁盘存储单位的 measure-diskunit 函数。

要创建 measure-diskunit 和 scan-disk 函数，可以执行以下操作：

```
function measure-diskunit { param($diskspace)
    switch ($diskspace) {
        {$_ -gt 1PB} { return [System.Math]::Round(($_ / 1PB),2),"PB" }
        {$_ -gt 1TB} { return [System.Math]::Round(($_ / 1TB),2),"TB" }
        {$_ -gt 1GB} { return [System.Math]::Round(($_ / 1GB),2),"GB" }
        {$_ -gt 1MB} { return [System.Math]::Round(($_ / 1MB),2),"MB" }
        {$_ -gt 1KB} { return [System.Math]::Round(($_ / 1KB),2),"KB" }
        default { return [System.Math]::Round(($_ / 1MB),2),"MB" }
    }
}
```

```
function scan-disks {
    check-kill
    $disks = get-wmiobject win32_logicaldisk
    Foreach ($disk in $disks) {
        $driveletter = $disk.DeviceID
        $freespace = $disk.FreeSpace
        $size = $disk.Size

        if ($freespace -lt 1) { $freespace = "0" }
        if ($size -lt 1) { $size = "0" }

        $freetype = measure-diskunit $freespace
        $convFreeSpc = $freetype[0]
        $funit = $freetype[1]

        $sizetype = measure-diskunit $size
        $convsize = $sizetype[0]
        $sunit = $sizetype[1]

        switch ($disk.DriveType) {
            0 { $type = "Type Unknown." }
            1 { $type = "Doesn't have a Root Directory." }
            2 { $type = "Removable Disk" }
            3 { $type = "Local Disk" }
            4 { $type = "Network Drive" }
            5 { $type = "Compact Disk" }
            6 { $type = "RAM Disk" }
            default { $type = "Unable To Determine Drive Type!" }
        }
        log "DiskConfiguration, Drive $driveletter | Drive Type: $type | Size: $convsize $sunit  | Freespace: $convFreeSpc $funit"
    }
}
```

创建此函数时没有输出可显示。

在此示例中，你创建了一个函数来测量磁盘单位并扫描系统上的单个磁盘配置：

- 1. 你首先通过 param($diskspace) 的参数块定义 measure-diskunit 函数。
- 2. 然后你创建一个 switch 语句来评估 $diskspace 变量。
- 3. 脚本按降序评估 petabytes、terabytes、gigabytes、megabytes 和 kilobytes。
- 4. 如果脚本对某个度量单位返回 True，它将用该单位度量除以该值并将值四舍五入到十分位。
- 5. 它将返回一个包含转换后的磁盘空间和度量单位标签的数组。然后你定义 scan-disks 函数来评估系统上的磁盘。
- 6. 你调用 check-kill 函数来确定脚本应继续还是终止。
- 7. 如果脚本可以继续，你使用 win32_logicaldisk 类来确定每个磁盘的驱动器号、可用空间和磁盘大小。
- 8. 你使用 measure-diskunit 函数来确定度量单位和该单位中的值。
- 9. 然后你使用 switch 语句来评估驱动器类型编号。
- 10. 相应的编号将返回磁盘类型的字符串值。
- 11. 你通过使用 log 函数并传入 "DiskConfiguration, Drive $driveletter | Drive Type: $type | Size: $convsize $sunit | Freespace: $convFreeSpc $funit" 来完成 scan-disks 函数。

## 计划任务函数

要创建用于扫描非内置用户运行的计划任务的函数，可以执行以下操作：

```
function scan-schtasks {
    check-kill
    $schtasks = Get-ScheduledTask
    foreach ($Task in $schtasks) {
        $tskUser = $Task.Principal.UserId
        if($tskUser -eq $null) { $tskuser = "SYSTEM" }
        if (!( $tskUser -match $blstRegEx) ) {
            $tskname = ($task.TaskName).replace(","," ")
        }
    }
}
```

```
$tskpath = $task.TaskPath
log "ScheduledTsksData, Scheduled task with the name of $tskname in the location of $tskpath is running as $tskuser"
}
}
}
```

创建此函数时没有输出可显示。

在此示例中，你创建了一个函数来扫描非内置用户运行的计划任务：

你首先定义 scan-schtasks 函数。

你调用 check-kill 函数来确定脚本应继续还是终止。

如果脚本可以继续，你使用 Get-ScheduledTask cmdlet 来收集所有计划任务。

你评估每个计划任务并确定 Principal.UserID 属性是否与内置用户正则表达式匹配。

如果用户与 $blstRegEx 正则表达式不匹配或 !($tskuser -match $blstRegEx)，你将收集计划任务名称和计划任务路径。

你通过使用 log 函数并传入 "ScheduledTsksData, Scheduled task with the name of $tskname in the location of $tskpath is running as $tskuser" 值来完成该函数。

## Windows 进程函数

要创建用于扫描系统中非内置用户进程的函数，可以执行以下操作：

```
function scan-process {
    check-kill
    $processes = Get-WmiObject win32_process
    foreach ($process in $processes) {
        $procuser = $process.GetOwner().User
        if (!( $procuser -match $blstRegEx)) {
            $procname = $process.Name
            $procdom = $process.GetOwner().Domain
```

```
log "WindowsProcessData, $procname is running with the $procdom\$procuser account."
    }
}
}
```

创建此函数时没有输出可显示。

在此示例中，你创建了一个函数来扫描非内置用户运行的进程：

你首先定义 scan-process 函数。你调用 check-kill 函数来确定脚本应继续还是终止。

如果脚本可以继续，你使用 win32_process WMI 类来获取当前正在运行的进程。

你继续评估每个进程以确定 .GetOwner().User 属性是否与内置用户正则表达式匹配。

如果用户与 $blstRegex 正则表达式不匹配或 !($procuser -match $blstRegEx)，你将收集进程名称、进程用户域和进程用户名。

你通过使用 log 函数并传入 "WindowsProcessData, $procname is running with the $procdom\$procuser account." 值来完成该函数。

## Windows 服务函数

要创建用于扫描系统中非内置用户服务的函数，可以执行以下操作：

```
function scan-services {
    check-kill
    $service = get-wmiobject win32_service
    foreach ($service in $services) {
        $svcAuthUser = $service.StartName
        if (!($svcAuthUser -match $blstRegEx)) {
            $svcdisplay = $service.DisplayName
            log "WindowsServicedata, Service with $svcdisplay name is running with $svcAuthUser account."
```
