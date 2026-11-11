
## 创建 Windows 事件日志源

如果您想要写入 Windows 事件日志，其先决条件之一是将脚本作为事件源注册到 Windows 事件日志系统。这是使用 New-EventLog cmdlet 完成的。您首先指定 New-EventLog cmdlet，然后是 -LogName 参数，后跟事件日志名称。您可以选择自己的事件日志，也可以指定内置事件日志之一，例如 Application 或 System。最后，您使用正在执行的脚本名称指定 -Source 参数。

事件日志源只能注册一次，这可能导致在同一系统上多次执行脚本时出现错误。您可以通过指定带有 -LogName 参数的 get-eventlog cmdlet 并将结果通过管道传递到 Select-object Source -Unique 来检索事件日志源。这并不理想，因为它要求系统查询所有日志事件以确定源是否已注册。由于 Windows 系统具有大量事件日志和日志事件，因此该查询非常占用 CPU 和内存。

避免重复事件日志源错误消息的最快方法是抑制错误消息本身。这是通过使用 New-EventLog cmdlet 将 -ErrorAction 参数设置为 SilentlyContinue 来实现的。指定 SilentlyContinue 的 -ErrorAction 参数将捕获错误并静默继续执行脚本，从而抑制错误消息。

要注册新的事件日志源并抑制错误消息，您可以执行以下操作：

```
New-EventLog -LogName Application -Source "WindowsServerScanningScript" -ErrorAction SilentlyContinue
```

PowerShell 窗口的输出将如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_35_0.png)

前面的脚本演示了如何使用 Windows 事件日志系统注册新的事件日志源。您首先使用 -LogName 参数设置为 Application 的 New-EventLog cmdlet。然后您指定 -Source 参数并将其设置为脚本名称 “WindowsServerScanningScript”。最后，您通过使用 -ErrorAction 参数并将其设置为 SilentlyContinue 来抑制错误消息。执行此命令后，您将能够使用事件源 WindowsServerScanningScript 写入 Application 事件日志。

![](img/a27a138d6586c10902d8e86ab3384834_36_0.png)

> 当您创建事件日志源时，它提供了使用该源记录消息的能力。您需要向该事件日志源写入内容，才能使其出现在 Application 事件日志中。

![](img/a27a138d6586c10902d8e86ab3384834_36_1.png)

> Windows 事件日志 cmdlet 要求 PowerShell 窗口以管理员身份运行。在测试此代码时，您需要右键单击 PowerShell 图标并选择以管理员身份运行。如果您远程执行此代码，可能需要确保您使用的用户凭据对系统具有管理员权限。

## 创建日志函数

在创建日志文件并注册事件日志源之后，您现在可以创建自定义日志函数。日志函数具有灵活性，因为它可以接受三种类型的数据写入的参数。这些包括事件日志、日志文件和数据收集 .csv 文件。

日志函数使用 function log { 声明。然后您需要利用参数块将参数接受到函数中。参数块使用 param() 声明，并包括变量 $string、$scnlg 和 $evntlg。$string 将是信息、警告、错误或数据收集内容的字符串值。$scnlg 变量决定内容是否写入扫描日志文件。$evntlg 变量决定内容是否写入事件日志。

要创建日志函数的开头，请输入以下内容：

```
function log { param($string, $scnlg, $evntlg)
```

使用日志函数的参数变体，您可以将日志信息写入不同的位置。您可以设置以下内容：

- 如果将 $scnlg 设置为 Y 且未为 $evntlg 指定值，脚本将仅写入扫描日志文件。
- 如果将 $scnlg 设置为 N 且将 $evntlg 设置为 Y，脚本将仅写入事件日志。
- 如果将 $scnlg 设置为 Y 且将 $evntlg 设置为 Y，脚本将同时写入扫描日志文件和事件日志。
- 如果除 $string 之外的参数未传递到函数中，它将把内容写入数据收集 CSV 文件。
- 在所有情况下，日志函数都将使用 write-host cmdlet 并使用 $string 变量的内容。这将详细打印所有日志变体。

要创建完整的日志函数，您可以执行以下操作：

```
function log { param($string, $scnlg, $evntlg)
    # If Y is populated in the second position, add to log file.
    if ($scnlg -like "y*") {
        Add-content -path $scanlog -Value $string
    }
    # If Y is populated in the third position, Log Item to Event Log As well
    if ($evntlg -like "y*") {
        write-eventlog -logname Application -source "WindowsServerScanningScript" -eventID 1000 -entrytype Information -message "$string"
    }
    # If there are no parameters specified, write to the data collection file (CSV)
    if (! $scnlg) {
        $content = "$env:COMPUTERNAME,$string"
        Add-Content -path $scanresults -Value $content
    }
    # Verbose Logging
    write-host $string
}
```

```
$date = Get-Date
log "Starting WindowsServerScanningScript at $date ..." "y" "y"
log "Writing a message to the Event Log Only." "N" "y"
log "ScriptStart,$date"
```

此脚本的输出将如下所示的屏幕截图：

![](img/a27a138d6586c10902d8e86ab3384834_38_0.png)

此示例演示如何创建日志函数并将信息记录到日志文件、事件日志和数据收集 CSV 文件。首先，您先声明 function log {。然后您使用 param($string, $scnlg, $evntlg) 代码段将三个参数接受到函数中。然后您创建一个 IF 语句来确定 $scnlg 内容是否 -like “Y”。如果返回 true，则执行 Add-content cmdlet，将 -path 参数设置为 $scanlog，将 -Value 参数设置为 $string。字符串的内容现在将在 $scanlog 文件中。

第二个 IF 语句评估 $evntlg 内容是否 -like “Y”。如果返回 True，则使用 -logname 参数设置为 Application 的 write-eventlog cmdlet。您还包括 -source 参数设置为 “WindowsServerScanningScript”、-eventID 参数设置为事件 ID 号 1000、-entrytype 参数设置为 Information 以及 -message 属性设置为 $string。字符串的内容现在将在 Application 事件日志中，源为 WindowsServerScanningScript，事件 ID 为 1000。

> 某些企业环境具有可以解析事件日志并根据事件 ID 触发操作的监控解决方案。作为传递 “Y” 的替代方案，您可以传递事件 ID，从而自动使用这些监控系统触发警报。您需要将 if 语句更新为 if ($evntlg) {}，如果变量中包含数据则返回 True。然后您可以在脚本中将 -eventID 参数设置为 $evntlg，从而能够根据需要更改事件 ID。

第三个 if 语句确定 $scnlg 变量中是否有任何数据。这意味着默认操作将写入数据收集 CSV 文件。您使用在 $scnlg 变量前加上感叹号的 if 语句。这指定如果不是 True，或者该变量中未传递任何数据，则继续执行下一步。然后您指定环境变量 $env:COMPUTERNAME，后跟逗号，后跟 $string 变量，并将值设置为 $content 变量。然后您通过使用 -path 参数设置为 $scanresults 且 -value 参数设置为 $content 的 Add-content cmdlet 将 $content 变量写入 CSV 文件。

函数的最后一步是通过使用 write-host cmdlet 并将 $string 作为内容来显示内容。

要测试脚本，您然后使用 Get-Date cmdlet 获取日期时间戳并将值设置为 $date 变量。然后您使用第一个参数为 “Starting WindowsServerScanningScript at $date”、第二个参数为 “Y”、第三个参数为 “Y” 调用 log 函数。这会将第一个参数中的内容同时写入日志文件和事件日志。

以下显示了所创建日志文件的内容：

![](img/a27a138d6586c10902d8e86ab3384834_39_0.png)

以下显示了脚本生成的事件日志的内容：

![](img/a27a138d6586c10902d8e86ab3384834_39_1.png)
