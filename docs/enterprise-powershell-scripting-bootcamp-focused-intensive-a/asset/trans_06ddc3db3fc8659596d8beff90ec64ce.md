
此输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_91_0.png)

此示例演示如何在系统上输出当前非内置用户。你首先定义一个名为 $users 的数组。然后你使用 Get-WmiObject 并传入 Win32_Process 类作为参数，并将结果放入 $processes 中。然后你创建一个 foreach 循环，并遍历 $processes 变量中的每个 $process。你接着使用 $process.GetOwner() 方法获取迭代进程的所有者，并将 .User 属性放入 $procuser 变量中。然后你声明一个 switch 语句来评估 $process.GetOwner().User 属性。首次评估针对的是内置账户。如果账户为 NETWORK SERVICE、LOCAL SERVICE、SYSTEM 或 $null，则会将 $continue 设置为 "Skip User"。如果不匹配这些内置用户，则会使用默认值，将 $continue 变量设置为 "Report User"。

退出 switch 语句后，你使用 If 语句评估 $continue 变量，如果值等于 "Report User" 则继续执行。此步骤会将 $procuser 变量添加到 $users 数组中。foreach 循环完成后，你将为每个非内置用户列出大量用户。为了将列表精简为单个或唯一用户，你将 $users 变量通过管道传递给 Get-Unique cmdlet。结果是系统上唯一非内置用户的列表。运行此示例的系统上有三个用户：DWM-1、bblawat 和 LocalJoe。

## User profiles

用户配置文件是企业中的常见挑战。由于它们在登录时自动生成，且没有内置的自动删除本地配置文件的功能，系统饱受旧配置文件账户困扰。旧配置文件不仅占用系统数据，还会泄露环境中其他活动账户的信息。

PowerShell 提供了检索用户配置文件信息的能力。要从系统检索用户配置文件，可以使用调用 Win32_UserProfile 类的 Get-WmiObject cmdlet。检索到用户配置文件对象后，你可以使用多个属性来收集有关配置文件的的信息。.SID 属性是配置文件中用户的 security identifier。虽然 SID 是一个唯一的用户名，但你可以使用 .NET System.Security.Principal 类轻松将其转换。你首先通过调用 New-Object cmdlet 并使用 System.Security.Principal.SecurityIdentifier 类创建新的 .NET 对象，将用户的 SID 作为参数。然后你从设置为 [System.Security.Principal.NTAccount] 类的 .Translate() 方法中检索值。.NET 类将从 SecurityIdentifier 转换到 NTAccount，从而输出用户名。

要将用户的 security identifier 转换为用户名，可以执行以下操作：

```powershell
$sid = "S-1-5-18"

$usersid = New-Object System.Security.Principal.SecurityIdentifier("$SID")

$usersid.Translate( [System.Security.Principal.NTAccount] ).Value
```

此输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_92_0.png)

此示例演示如何正确地将 SID 转换为用户名。你首先定义 "S-1-5-18" 的 security identifier 并将其放入 $sid 变量中。然后你使用 New-Object cmdlet 调用 System.Security.Principal.SecurityIdentifier 类，并使用 $sid 参数并将其设为等于 $SID。这会将 SID 对象放入 $usersid 变量中。然后你在 SID 对象上调用 $usersid.Translate() 方法，并指定 [System.Security.Principal.NTAccount] 作为参数。最后，你在转换后的数据上调用 .Value 属性，即可获得用户名的值。在此示例中，你了解到 SID S-1-5-18 被转换为了 NT AUTHORITY\SYSTEM。

Win32_UserProfile 类中另一个常用的属性是 LastUseTime 属性。LastUseTime 提供了配置文件上次使用时间的洞察。此时间戳采用分布式管理任务组（DMTF）格式，通常不是便于阅读的用户友好格式。为了将此值简化为不同的时间格式，可以使用 [Management.ManagementDateTimeConverter] 类的 ToDateTime() 方法，并将 LastUseTime 作为参数。运行此方法后，你将成功把值转换为用户友好的格式。

要将 LastUseTime 转换为用户友好的格式，可以执行以下操作：

```powershell
$profile = Get-WmiObject Win32_UserProfile | Where {$_.SID -eq "S-1-5-18"}
$lastusetime = $profile.LastUseTime
$lastusetime
[Management.ManagementDateTimeConverter]::ToDateTime($lastusetime)
```

此输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_93_0.png)

此示例演示如何将 DMTF 格式的值转换为用户友好的格式。你首先使用 Get-WmiObject cmdlet 并调用 Win32_UserProfile 类。然后你将此值通过管道传递给 Where {$_.SID -eq "S-1-5-18"} 并将此配置文件对象放入 $profile 变量中。然后你检索 .LastUseTime 属性并将其放入 $lastusetime 变量中。然后你调用 $lastusetime 以在屏幕上显示 DMTF 格式的时间。在此示例中，DMTF 为 20170216051704.765000+000。最后，你调用 [Management.ManagementDateTimeConverter] 类并使用 $lastusetime 作为参数调用 ToDateTime() 方法。此命令的输出是用户友好的值 Wednesday, February 15, 2017 11:17:04 PM。

要确定用户配置文件的 SID、用户名和年龄，可以执行以下操作：

```powershell
$profiles = Get-WmiObject Win32_UserProfile
foreach ($profile in $profiles) {
    $currentdate = Get-Date
    $lastusetime = $profile.LastUseTime
    $lastusetime = [Management.ManagementDateTimeConverter]::ToDateTime($lastusetime)

    $age = [math]::Round(($currentdate - $lastusetime).TotalDays)

    $sid = $profile.SID
    Try {
        $usersid = New-Object System.Security.Principal.SecurityIdentifier("$SID")
        $username = $usersid.Translate( [System.Security.Principal.NTAccount]).Value
    }
    Catch {
        Write-Host "There was an error translating SID value $sid to a username. Account may not exist."
        $username = "(Deleted Account)"
    }
    Write-host "User with name $username and SID $sid last logged in $lastusetime. ($age Days Old)"
}
```

此输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_94_0.png)

此示例演示如何检索用户配置文件的 SID、用户名和年龄。你首先使用 Win32_UserProfile 类作为参数使用 Get-WmiObject cmdlet 并将 UserProfile 对象存储在 $profiles 变量中。然后你使用 $profile 作为迭代器、$profiles 作为数据集的 foreach 循环。对于配置文件老化过程，你调用 Get-Date cmdlet 并将值设置在 $currentdate 变量中。然后你获取 $profile.LastUseTime 属性的当前实例，并将其放入 $lastusetime 变量中。

要计算年龄，首先必须将 $lastusetime 转换为友好格式。为此，你使用 $lastusetime 变量作为参数使用 [Management.ManagementDateTimeConverter]::ToDateTime() 方法，并将结果放入 $lastusetime 变量中。这会用标准时间格式值覆盖之前的 DMTF 格式值。然后你通过从 $lastusetime 中减去 $currentdate 变量并选择 .TotalDays 属性来计算年龄。由于返回的值包含小数，你使用 [math]::Round() 方法对 TotalDays 属性结果执行 .net 四舍五入计算。这会返回一个日期年龄的单个数字，并设置在 $age 变量中。

要计算用户名，你调用当前实例 $profile.SID 属性并将其设为等于 $sid 变量。然后你执行 Try{ 语句以捕获 SID 上任何 .NET 转换错误。在 Try 语句内部，你使用 New-Object cmdlet、System.Security.Principal.SecurityIdentifier() 类和 $SID 参数获取 SID 对象。然后 SID 对象返回到 $usersid 变量。然后你使用设置为 [System.Security.Principal.NTAccount] 类对象的 $usersid.Translate() 方法将 SID 转换为 NTAccount。值转换后，你调用 .Value 属性将结果放入 $username 变量中。

如果 SID 或用户不存在，你执行 Catch { 语句来捕获错误。如果发生这种情况，你使用 Write-Host cmdlet 向显示器写入 "There was an error translating SID value $sid to a username. Account may not exist"。然后你将 $username 变量设置为 "(Deleted Account)"。

作为最后一步，你使用 Write-Host cmdlet 向屏幕写入 "User with the name $username and SID $sid last logged in $lastusetime. ($age Days old)"。处理此脚本后，此示例显示系统上内置的、本地的和域配置文件。
