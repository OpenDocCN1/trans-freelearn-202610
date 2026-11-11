

```
$contents = Get-ChildItem c:\windows\
}
Write-host "Folder Scanning Operation Complete."
}
```

```
$time1
$time2
$timediff = ($time1 - $time2).TotalSeconds
Write-host "Total Difference in Speed: $timediff Total Seconds"
```

此输出在以下屏幕截图中显示：

![](img/a27a138d6586c10902d8e86ab3384834_156_0.png)

在此示例中，您创建一个可以启用和禁用的 write-host 函数，在操作中途禁用详细日志记录，并计时差异：

- 1. 您首先通过声明 $verbose 变量并将其值设置为 $true 开始。
- 2. 然后您创建 log 函数，该函数包含一个 if 语句来验证 $verbose 是否设置为 $true。如果 $verbose 设置为 $true，write-host cmdlet 会将管道或 $_ 打印到屏幕上。
- 3. 您声明 measure-command cmdlet 来测量时间。
- 4. 您创建一个从 1 到 10000 的范围并将其通过管道传递给 %，它是 foreach 的别名。
- 5. 您使用 "Number $_" 利用 log 函数来显示当前步骤的编号。
- 6. 然后您使用 "c:\Windows\" 参数使用 Get-ChildItem cmdlet 并将这些结果设置为 $contents。
- 此操作将重复 10,000 次。执行持续时间将被 measure-command cmdlet 捕获并存储在 $time1 变量中。
- 7. 对于第二个操作，您调用 measure-command cmdlet。
- 8. 您创建一个从 1 到 5000 的范围并将其通过管道传递给 %。
- 9. 您使用 "Number $_" 作为管道发出 log 函数。
- 10. 然后您使用 "c:\Windows\" 参数利用 Get-ChildItem cmdlet 并将这些结果设置为 $contents。
- 11. 操作重复 5,000 次后，您通过将 $verbose 设置为 $false 来禁用详细日志记录。
- 12. 您还使用 write-host cmdlet 将其设置为 "Running Folder Scanning Operation..." 将高级信息写入屏幕。
- 13. 您继续创建一个从 1 到 5000 的第二个范围，即操作的后半部分。您将范围通过管道传递给 %。
- 14. 您使用 "Number $_" 作为管道发出 log 函数。
- 15. 您使用 "c:\Windows\" 参数利用 Get-ChildItem cmdlet 并将这些结果设置为 $contents。
- 操作再重复 5,000 次后，执行持续时间将被 measure-command cmdlet 捕获并存储在 $time2 变量中。
- 要查看测量的时间，您通过调用变量来显示 $time1 和 $time2 中的持续时间。

- 16. 然后您通过调用 `$time1 - $time2` 并引用 TotalSeconds 属性来计算执行时间的差异。
- 17. 您将此值设置在 `$timediff` 变量中。
- 18. 最后，您使用 Write-host cmdlet 并使用 "Total Difference in Speed: $timediff Total Seconds" 将时间值打印到显示器上。

您将看到，在此示例中，虽然两个操作都调用相同的日志函数，但禁用 `$verbose` 可将性能提高约 10%。

## write-progress cmdlet 的速度考虑

write-progress cmdlet 是在屏幕上显示操作进度的绝佳方式。它不会用信息淹没屏幕，并提供当前进度的清晰度量。然而，write-progress cmdlet 与 write-host cmdlet 有类似的问题。由于您在循环中提供视觉反馈，因此会减慢脚本的整体性能。

要显示使用 write-progress 循环与将操作保留在内存之间的速度差异，您可以执行以下操作：

```
$time1 = measure-command {
    1..10000 | % {
        $attempt = $_
        $perComplete = [math]::truncate(($attempt/10000)*100)
        write-progress -Activity "Looping Retrieval of Contents of c:\Windows" -Status "$attempt of 10000." -PercentComplete $perComplete
        # Get the contents of C:\Windows\
        $contents = Get-ChildItem c:\windows\
    }
}

$time2 = measure-command {
    1..10000 | % {
        # Get the contents of C:\Windows\
        $contents = Get-ChildItem c:\windows\
    }
}

$time1
$time2
$timediff = ($time1 - $time2).TotalSeconds
Write-host "Total Difference in Speed: $timediff Total Seconds"
```

此输出在以下屏幕截图中显示：

![](img/a27a138d6586c10902d8e86ab3384834_159_0.png)

此示例显示了使用 write-progress cmdlet 与将操作保留在内存之间的差异：

- 1. 您首先通过声明 `measure-command` cmdlet 开始。
- 2. 然后您声明从 1 到 10000 的范围并将其通过管道传递给 `%`，它是 `foreach` 的别名。
- 3. 接下来，您创建 `$attempt` 变量并将其设置为管道 `$_`，即脚本的当前迭代。
- 4. 由于 write-progress cmdlet 不会自动舍入完成百分比，您必须利用 [math].Net 程序集将完成百分比截断为整数。
- 5. 要计算完成百分比，您取当前尝试次数除以总迭代次数（10,000），并通过将值乘以 100 获得百分比。此舍入值随后存储在 $perComplete 变量中。
- 6. 您继续使用 write-progress cmdlet，将 -Activity 参数设置为 "Looping Retrieval of Contents of c:\Windows:"，将 -status 参数设置为 "$attempt of 10000."，并将 -PercentComplete 参数设置为 $perComplete。
- 7. 最后，您使用 c:\windows\ 参数调用 Get-ChildItem cmdlet。此操作将重复 10,000 次。
- 执行持续时间将被 measure-command cmdlet 捕获并存储在 $time1 变量中。
- 8. 然后您设置第二个测量命令。
- 9. 您声明 measure-command cmdlet。
- 10. 您声明从 1 到 10000 的范围并将其通过管道传递给 %。
- 11. 您使用 "c:\Windows\" 参数利用 Get-ChildItem cmdlet 并将这些结果设置为 $contents。
- 12. 此操作将重复 10,000 次。执行持续时间将被 measure-command cmdlet 捕获并存储在 $time2 变量中。
- 执行第二个命令后，您通过调用变量来显示 $time1 和 $time2 中的持续时间。
- 13. 然后您通过调用 $time1 - $time2 并引用 TotalSeconds 属性来计算执行时间的差异。
- 14. 您将此值设置在 $timediff 变量中。
- 15. 最后，您使用 write-host cmdlet 并使用 "Total Difference in Speed: $timediff Total Seconds" 将时间值打印到显示器上。

您将看到，在此示例中，将操作保留在内存中比使用 write-progress cmdlet 快约 219%。

## 速度考虑 - switch 与 if 语句

可能有时候您需要对字符串的多个值进行评估。常见的做法是创建多个 if 语句来评估这些字符串。虽然这在小型数据集中可能效果良好，但通过利用 switch 语句可以获得性能提升。这是因为 switch 语句在数据通过时评估数据的方式。switch 语句在找到匹配项后会停止向下评估语句列表。相反，多个 if 语句即使找到匹配项也会每次都进行评估。这也意味着，如果您正在排序大量数据，您希望将最常见的数据放在 switch 语句的顶部，而不太可能的数据放在底部。这将使您能够快速匹配最常见的分母并继续传递到循环中的下一个值。

下一个示例将模拟大型 .exe、.dll 和 .xml 文件夹结构。此示例的第一部分设置用于评估的数据集，而第二部分将文件类型的计数排序到不同的变量中。您将利用 if 语句和 switch 来确定 switch 的效率。

> 第 1 部分和第 2 部分要求 PowerShell 窗口以管理员身份运行。在测试此代码时，您需要右键单击 PowerShell 图标并选择以管理员身份运行。如果您远程执行此代码，您需要确保您使用的用户凭据对系统具有管理权限。

Part 1 – 要创建 .dll、.exe 和 .xml 文件的数据集，您可以执行以下操作：

```powershell
# Gather all the File Extensions c:\windows\System32
$dlls = 0
$exes = 0
$xmls = 0
$extensions = @()
$contents += ((Get-ChildItem "c:\windows\System32\" -include "*.xml","*.dll","*.exe" -Recurse -ErrorActionSilentlyContinue) | Select Name)
foreach ($item in ($contents.Name)) {
    $sline = $item.split(".").length
    $extensions += '.' + $item.split(".")[$sline-1]
}
1..13 | % { $extensions += $extensions }
Write-host "Total number of Extensions: " $extensions.count
```
