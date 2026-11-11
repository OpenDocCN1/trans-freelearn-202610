
本输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_86_0.png)

本脚本演示如何通过通配符搜索进程并获取进程 ID。你也可以通过调用该服务的进程 ID 来查看同一服务。首先使用 get-process cmdlet 并使用 powersh* 搜索通配符开始。系统将 PowerShell 进程对象返回到 $process 变量中。然后你调用 $process 变量以查看有关 PowerShell 进程的信息。然后你使用指向 $process 变量的 -id 参数利用 get-process cmdlet，引用 .id 的点表示法。这会返回相同的 PowerShell 进程信息，即在第一次搜索结果的进程 ID 中引用的 $process.id。执行此脚本后，你将看到 PowerShell 进程的 Handles、NPM(K)、PM(K)、WS(K)、VM、CPU(s)、Id 和 ProcessName 信息。

要使用通配符获取进程并获取其 FileVersionInfo 信息，可以执行以下操作：

```
$process = get-process powersh*
get-process -id $process.id -FileVersionInfo | format-table -AutoSize
```

本输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_86_1.png)

本脚本演示如何通过通配符搜索进程，然后使用该信息查看文件版本信息。你首先使用 get-process cmdlet 并使用 powersh* 搜索通配符开始。系统将 PowerShell 进程返回到 $process 变量中。然后你调用 get-process cmdlet 并利用指向 $process 变量的 -id 参数，引用 .id 的点表示法。你还调用 -FileVersionInfo 参数以显示有关 PowerShell 进程的详细信息。然后你将结果通过管道传递给带有 -AutoSize 参数的 format-table cmdlet，该参数格式化表格以免截断结果。执行此脚本后，你将看到有关 PowerShell 进程的 ProductVersion、FileVersion 和 FileName 信息。

可能存在需要确定哪个用户或服务帐户正在运行进程的情况。为此，你可以使用带有 -class 参数和 win32_process 参数的 get-wmiobject cmdlet。WMI 进程对象具有多个属性和方法，可让你查看系统上运行的进程的扩展信息。.Name 属性显示名称，而 .GetOwner() 方法则显示有关进程所有者的信息。此方法是一个 .User 属性，显示运行服务的用户，以及 .Domain 属性，提供用户所属的域。

![](img/a27a138d6586c10902d8e86ab3384834_87_0.png)

> 查询系统进程和其他用户的进程需要以提升权限运行的 PowerShell 窗口。你需要使用“以管理员身份运行”参数打开 PowerShell。

要获取系统上运行 PowerShell.exe 的用户，可以执行以下操作：

```powershell
$processes = Get-WmiObject -class win32_process | where {$_.Name -like "powersh*"}

foreach ($process in $processes) {
    $procname = $process.Name
    $procdom = $process.GetOwner().Domain
    $procuser = $process.GetOwner().User
    Write-host "$procname is running with the $procdom\$procuser account."
}
```

本输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_88_0.png)

本示例演示如何确定系统上进程的所有者。你首先通过将 -class 参数设置为 win32_process 调用 get-WmiObject cmdlet 开始。此命令通过管道传递到 where {$_.Name -like "powersh*"}，它将检索所有名称以 Powersh 开头的进程。这些对象随后被放入 $processes 变量中。然后你创建一个 foreach 语句来遍历 $processes 变量中的每个 $process 对象。你使用 $process.Name 属性检索进程名称并将其设置为 $procname 变量。你通过利用 $process.GetOwner() 方法和 .Domain 属性来收集进程所有者域名称。最后，你使用 $process.GetOwner() 方法和 .User 属性检索进程所有者名称。要显示收集的数据，你使用 write-host cmdlet 并显示 $procname is running with the $procdom\$procuser account。在此特定系统上，返回 powershell_ise.exe 以 POSHDEMO\Brenton 帐户运行。 和 powershell.exe 以 POSHDEMO\Brenton 帐户运行。

要启动新进程或调用程序，可以使用 start-process cmdlet。使用此 cmdlet 的正确语法是调用 start-process cmdlet，提供指向要执行项目的所在位置的 -FilePath 参数。然后你可以调用可选的 -argumentList 参数引用执行项目所需的参数，可选的 -verb 参数以调用与文件类型关联的任何动词（如 Edit、Open、Play、Print 和 RunAs），可选的 -NoNewWindow 参数以不在新的 Powershell 控制台中生成命令，以及可选的 -wait 参数以在继续脚本之前等待进程完成。如果不执行 -wait 参数，脚本将继续执行下一步，而不会等待当前步骤成功。启动进程后，进程将获得一个进程 ID，你可以使用其他进程 cmdlet 引用它。

要启动新的记事本进程，可以执行以下操作：

```
start-process -FilePath notepad.exe
$process = get-process notepad*
```

本输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_89_0.png)

本脚本演示如何启动记事本进程并通过通配符搜索记事本进程。你首先使用 start-process cmdlet 并使用引用 notepad.exe 进程的 -FilePath 参数开始。执行后，它将启动 notepad.exe。然后你使用 notepad* 通配符使用 get-process cmdlet。系统将 PowerShell 进程对象返回到 $process 变量中。将 PowerShell 进程对象存储在 $process 变量中可让你根据需要与该对象的属性进行交互。

要停止进程或停止程序，可以使用 stop-process cmdlet。使用此 cmdlet 的正确语法是调用 stop-process cmdlet，提供指向要终止项目的所在位置的 -FilePath 参数。你也可以利用 -processname 参数通过其进程名称停止服务。你还可以使用通配符与 -processname 参数一起使用，以结束与你指定的部分单词相似的进程。你也可以指定 -id 参数通过其进程 ID 终止进程。默认情况下，如果你终止进程，它会提示确认。使用 -force 参数将强制终止进程而不提示用户。

要停止正在运行的记事本进程，可以执行以下操作：

```
start-process -FilePath notepad.exe
$process = get-process notepad*
stop-process -ID $process.id
```

本输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_89_1.png)

本脚本演示如何启动记事本进程，通过通配符搜索记事本进程，然后使用该信息停止记事本进程。你首先使用 start-process cmdlet 并使用引用 notepad.exe 进程的 -FilePath 参数开始。执行后，它将启动 notepad.exe。然后你使用 notepad* 通配符使用 get-process cmdlet。系统将 PowerShell 进程返回到 $process 变量中。然后你调用 stop-process cmdlet 并利用指向 $process 变量的 -id 参数，引用 .id 的点表示法。执行此脚本后，你将看到记事本打开并关闭。

## 识别已登录用户

可能存在需要识别已登录到系统的用户的情况。当用户登录到你的计算机时，他们会与系统创建交互式会话。这会在该用户名下生成进程作为进程所有者。此外，当你创建服务帐户在系统上启动服务时，底层进程将作为该服务帐户运行。鉴于这两种方法都会调用进程，确定当前已登录用户的最佳方法是评估正在运行的进程。

除了识别已登录用户外，你还需要过滤掉内置 Windows 帐户。为此，你可以创建 switch 语句以对进程所有者进行多次评估。如果进程所有者用户名是 NETWORK SERVICE、LOCAL SERVICE、$null 或 SYSTEM，你可以跳过报告用户名。如果它与这些值中的任何一个都不匹配，它将使用默认 switch 并将用户报告到列表中。由于多个进程在同一用户下运行，你可以将用户结果通过管道传递给 Get-Unique cmdlet，它只会选择数组中的唯一值。

> 根据系统上进程的数量，以下示例最多需要 30 秒才能执行。

要评估正在运行的进程以获取非内置已登录用户列表，可以执行以下操作：

```
$users = @()
$processes = Get-WmiObject win32_process
foreach ($process in $processes) {
    $procuser = $process.GetOwner().User
    switch ($process.GetOwner().User) {
        "NETWORK SERVICE" { $continue = "Skip User" }
        "LOCAL SERVICE" { $continue = "Skip User" }
        "SYSTEM" { $continue = "Skip User"}
        "$null" { $continue = "Skip User" }
        default { $continue = "Report User" }
    }
    if ($continue -eq "Report User") {
        $users += $procuser
    }
}
$users | Get-Unique
```
