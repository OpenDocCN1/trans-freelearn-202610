
```
}
}
}
```

创建此函数时没有输出可显示。

> 要获取有关与 Windows 服务交互的详细说明，您可以参考第 5 章《与服务、进程、配置文件和已登录用户交互》。

在此示例中，您创建一个函数来扫描未以内置用户身份运行的 Windows 服务：

- 1. 您首先定义 scan-services 函数。
- 2. 您调用 check-kill 函数以确定脚本应继续还是终止。
- 3. 如果脚本可以继续，您利用 win32_service WMI 类获取有关 Windows 服务的信息。
- 4. 您继续评估每个服务，以确定 .Startname 属性是否与内置用户正则表达式匹配。
- 5. 如果用户未被 $blstRegEx 正则表达式匹配，或 !($svcAuthUser -match $blstRegEx)，您将收集服务的 DisplayName。
- 6. 您通过使用 log 函数并传入 “WindowsServicedata, Service with $svcdisplay name is running with $svcAuthUser account.” 值来完成该函数。

## 已安装软件函数

要创建用于扫描系统上已安装软件的函数，您可以执行以下操作：

```
function scan-software {
    check-kill
    $RegLocations = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*","HKLM:\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*"
    foreach ($reg in $RegLocations) {
        $softwareKeys = get-ItemProperty $reg | Select DisplayName | Sort DisplayName
        foreach ($software in $softwareKeys) {
```
```
if ($software.DisplayName -ne $null) {
    $value = "InstalledSoftware," + $software.DisplayName
    log $value
}
}
}
$progpaths = "\Program Files\","\Program Files (x86)\"
$disks = (Get-WmiObject win32_logicaldisk | where {$_.DriveType -eq "3"}).DeviceID
foreach ($disk in $disks) {
    foreach ($progpath in $progpaths) {
        check-kill
        $progfile = $disk + $progpath
        $test = test-path $progfile
        if ($test) {
            $files = Get-ChildItem -file $progfile -Recurse | where {$_.FullName -like "*.exe"} | Select Fullname
            foreach ($file in $files) {
                $productName = (get-itemproperty $file.FullName).VersionInfo.ProductName
                if (!$productName) { $productName = "Product Name n/a" }
                $productVersion = (get-itemproperty $file.FullName).VersionInfo.ProductVersion
                if (!$productVersion) { $productVersion = "Product Version n/a"}
                $value = "InstalledSoftware," + $file.Fullname + " | Name: $productName | Version: $productVersion"
                log $value
            }
        }
    }
}
}
```

创建此函数时没有输出可显示。

在此示例中，您创建一个函数来扫描系统上已安装的软件：

- 1. 您首先定义 scan-software 函数。
- 2. 您调用 check-kill 函数以确定脚本应继续还是终止。
- 3. 如果脚本可以继续，您填充包含已安装软件显示名称的两个主要注册表项，并将它们放在 $reglocations 变量中。
- 4. 对于这些注册表位置中的每一个，您利用 get-ItemProperty cmdlet 获取已安装软件的显示名称。
- 5. 如果显示名称有值，您构建 “InstalledSoftware,” + $software.DisplayName 字符串并将其放在 $value 变量中。
- 6. 然后您使用 $value 利用 log 函数将各个软件标题记录到 CSV 文件中。

您继续在每个磁盘驱动器上的 \program files\ 和 \program files (x86)\ 目录中评估可执行文件：

- 1. 您利用 win32_logicaldisk WMI 类获取本地磁盘的驱动器号。
- 2. 您对每个磁盘评估 program files\ 和 \program files (x86)\，并递归查询所有以 .exe 扩展名结尾的文件。
- 3. 然后您再次调用 check-kill 函数以确定脚本应继续还是终止。
- 4. 如果脚本可以继续，您查询可执行文件 VersionInfo.ProductName 属性并将其放在 $productname 变量中。
- 5. 如果未找到产品名称，您将 $productname 变量替换为 “Product Name n/a”。
- 6. 您继续获取 VersionInfo.ProductVersion 属性并将其放在 $productversion 变量中。
- 7. 如果未找到产品版本，您将 $productversion 变量替换为 “Product Version n/a”。
- 8. 然后您创建 “InstalledSoftware,” + $file.Fullname + " | Name: $productName | Version: $productVersion" 字符串并将其放在 $value 变量中。
- 9. 然后您使用 $value 利用 log 函数将各个软件可执行产品的产品名称和产品版本记录到 CSV 文件中。

## 用户配置文件函数

要创建用于扫描系统配置文件以识别用户配置文件的函数，您可以执行以下操作：

```
function scan-profiles {
check-kill
$profiles = get-wmiobject Win32_UserProfile
    foreach ($profile in $profiles) {
        $currentdate = Get-Date
        $lastusetime = $profile.LastUseTime
        $lastusetime = [Management.ManagementDateTimeConverter]::ToDateTime($lastusetime)
        $age = [math]::Round(($currentdate - $lastusetime).TotalDays)
        $sid = $profile.SID
        Try {
            $usersid = New-Object System.Security.Principal.
SecurityIdentifier("$SID")
            $username = $usersid.Translate( [System.Security.Principal.
NTAccount] ).Value
        }
        Catch {
            log "There was an error translating SID value $sid to a username. Account may not exist." "y"
            $username = "(Deleted Account)"
        }
        log "UserProfileData, User with name $username and SID $sid last logged in $lastusetime. ($age Days Old)"
    }
}
```

创建此函数时没有输出可显示。

在此示例中，您创建一个函数来扫描系统上的用户配置文件：

- 1. 您首先声明 scan-profiles 函数。
- 2. 您调用 check-kill 函数以确定脚本应继续还是终止。
- 3. 如果脚本可以继续，您利用 win32_userprofile WMI 类从系统中收集用户配置文件。
- 4. 然后您评估系统上每个配置文件的上次使用时间。
- 5. 您计算配置文件上次使用以来的天数。
- 6. 您继续识别配置文件的 .SID 属性并将其存储在 $SID 变量中。
- 7. 您尝试将 $SID 变量翻译为用户名。如果用户名翻译正确，它将把用户名放在 $username 变量中。
- 8. 如果翻译失败，try...catch 方法将处理错误并使用 log 函数以及 “There was an error translating SID value $sid to a username. Account may not exist.” “Y” 值。
- 9. 这将在启用时将错误消息记录到日志文件。
- 10. 如果 SID 无法翻译，脚本还将 $username 变量设置为 (Deleted Account)。
- 11. 最后，您使用 log 函数并传入 “UserProfileData, User with name $username and SID $sid last logged in $lastusetime. ($age Days Old)” 值。根据配置文件的数量，CSV 文件中将记录多个值。

## Windows 功能函数

要创建用于扫描已安装 Windows 功能的函数，您可以执行以下操作：

```
function scan-features {
    check-kill
    $crntFeatures = Get-wmiobject win32_serverfeature -ErrorAction SilentlyContinue -ErrorVariable err
    if ($err) { log "ServerFeatureInfo, Cannot get server feature information from WMI. System may not be a server or access is denied to WMI." }
    foreach ($feature in $crntFeatures) {
```
```
$featurename = $feature.Name
    log "ServerFeatureInfo, $featurename feature is installed on the system."
}
}
```

创建此函数时没有输出可显示。

在此示例中，您创建一个函数来查询系统上已安装的 Windows 功能：

- 1. 您首先通过定义 scan-features 函数开始。
- 2. 您调用 check-kill 函数以确定脚本应继续还是终止。
- 3. 如果脚本可以继续，您通过利用 win32_serverfeature WMI 类获取系统上当前安装的所有功能。
- 4. 如果获取服务器功能时出现错误，您使用 log 函数并传入 “ServerFeatureInfo, Cannot get server feature information from WMI. System may not be a server or access is denied to WMI.” 值。
- 5. 如果您能够获取功能信息，脚本将继续评估系统上安装的每个功能。
- 6. 您获取功能的 .Name 属性并将其放在 $featurename 变量中。
- 7. 最后，您使用 log 函数并传入 “ServerFeatureInfo, $featurename feature is installed on the system.” 值。

## 扫描文件和文件夹函数

要创建用于扫描目录中包含特定字符串的文件的函数，您可以执行以下操作：

```
function scan-directory {
    $errors = @()
    foreach ($directory in $scnloc) {
        check-kill
        $content = get-childitem $directory.id -Include $srextlist -recurse -ErrorAction SilentlyContinue -ErrorVariable +errors | select-string -Pattern $srtermsRegEx -ErrorAction SilentlyContinue
        if ($errors) {
            foreach ($err in $errors) {
```
