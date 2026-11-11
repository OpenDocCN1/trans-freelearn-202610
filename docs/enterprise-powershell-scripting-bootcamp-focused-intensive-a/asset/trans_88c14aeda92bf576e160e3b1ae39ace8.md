
此输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_162_0.png)

在此示例中，您为第 2 部分创建了一个要排序的数据集：

- 1. 您首先通过将变量 $dlls、$exes 和 $xmls 设置为零值开始。
- 2. 您通过将其设置为等于 @() 来声明 $extensions 数组。
- 3. 接下来，您使用 Get-ChildItem cmdlet，参数为 "c:\Windows\System32"，-include 参数为 "*.xml"、"*.dll"、"*.exe"，以及 -Recurse 参数，并将 -ErrorAction 参数设置为 SilentlyContinue 参数。
- 4. 然后您将该值通过管道传递给 Select Name，以选择每个文件的 name 属性。
- 5. 然后您使用 += 赋值运算符将每个值添加到 $contents 变量中。 然后您继续获取每个文件的扩展名。
- 6. 您首先使用 foreach 循环通过 Name 属性评估 $contents 中的每个 $item。
- 7. 由于文件名可能包含多个句点，您必须通过文件名中的最后一个句点来确定扩展名。您继续按句点字符拆分当前的 $item 并计算文件名中句点的数量。
- 8. 您将计算出的句点数量设置为 $sline 变量。
- 9. 为了选择文件名中的最后一个值，您设置 $item 并使用 .split(".") 方法。
- 10. 然后您通过 $sline 数字减一来选择最后一个拆分值，并且数组是从零开始的。
- 11. 您向 $extensions 数组添加一个 "," 加上文件的最终扩展名。

第 1 部分的最后一部分是将找到的扩展名按 13 的倍数复合。这将模拟大量信息通过第 2 部分的脚本传递。您随后利用 write-host cmdlet 显示 "Total number of extensions: " 并使用 $extensions.count 显示扩展名数量。此示例返回 42,713,088 个待排序的扩展名。

## 第 2 部分 - 要测量使用 if 语句和 switch 语句对数据进行排序之间的持续时间差异，您可以执行以下操作：

```
$time1 = measure-command {
foreach ($extension in $extensions) {
    # Determine the file types
    if ($extension -like "*.xml") { $xmls += 1 }
    if ($extension -like "*.exe") { $exes += 1 }
    if ($extension -like "*.dll") { $dlls += 1 }
}
}
# Reset Count
$dlls = 0
$exes = 0
$xmls = 0
$time2 = measure-command {
foreach ($extension in $extensions) {
    # Determine the file types
    switch($extension) {
".dll" { $dlls += 1 }
".exe" { $exes += 1 }
".xml" { $xmls += 1 }
    default { } #do nothing
    }
}
}

$time1
$time2
$timediff = ($time1 - $time2).TotalSeconds
Write-Host "Total Number of Counted DLL files $dlls"
Write-Host "Total Number of Counted EXE files $exes"
Write-Host "Total Number of Counted XML files $xmls"
Write-Host "Total Difference in Speed: $timediff Total Seconds"
```

此输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_164_0.png)

在此示例中，您测量使用 if 语句和 switch 语句对数据进行排序之间的差异：

- 1. 您首先利用 measure-command cmdlet 来测量使用 if 语句排序的持续时间。
- 2. 您继续使用 foreach 循环来评估 $extensions 数组中的每个 $extension。
- 3. 您声明如果 $extension -like "*.xml" 的语句。如果此条件评估为 true，则它将利用 += 赋值运算符向 $xmls 变量添加一个值。
- 4. 然后您评估如果 $extension -like "*.exe" 的语句。如果此条件评估为 true，则它将利用 += 赋值运算符向 $exes 变量添加一个值。
- 5. 最后，您评估如果 $extension -like "*.dll" 的语句。如果此条件评估为 true，则它将利用 += 赋值运算符向 $dlls 变量添加一个值。数据排序完成后，脚本将测量的时间添加到 $time1 变量中。

为第二个数据排序准备脚本时，您通过将它们设置为 0 来重置 $dlls、$exes 和 $xmls 变量。

- 6. 您继续利用 measure-command cmdlet 来测量使用 switch 语句排序的持续时间。
- 7. 您调用 foreach 循环来评估 $extension 数组中的每个 $extension。
- 8. 您使用 $extension 作为参数调用 switch 语句。
- 9. 您声明 ".dll"；如果匹配，脚本将向 $dlls 变量添加一个值。
- 10. 然后您声明 ".exe"；如果匹配，脚本将向 $exes 变量添加一个值。
- 11. 您还声明 ".xml"；如果匹配，脚本将向 $xmls 变量添加一个值。
- 12. 最后，您使用 default 语句并将其设置为 { }，不执行任何操作。
- 13. 数据排序完成后，脚本将测量的时间添加到 $time2 变量中。

在进行时间测量后，您通过调用变量来显示 $time1 和 $time2 中的持续时间。

- 14. 您调用 write-host cmdlet 并显示 "Total Number of Counted DLL files $dlls"。
- 15. 您再次调用 write-host cmdlet，并显示 "Total Number of Counted EXE files $exes"。
- 16. 您最后一次调用 write-host cmdlet，并显示 "Total Number of Counted XML files $xmls"。
- 17. 您通过调用 $time1 - $time2 并引用 TotalSeconds 属性来计算执行时间差异。您将此值设置为 $timediff 变量。
- 18. 最后，您使用 write-host cmdlet 并使用 "Total Difference in Speed: $timediff Total Seconds" 将时间值打印到显示器上。

您会看到，在此示例中，用于数据排序的 switch 比使用 `if` 语句快约 128 倍。

您会注意到，`switch` 语句中的评估顺序设置为先 `.dll`，其次 `.exe`，最后 `.xml`。这些是基于系统上的常见性进行评估的。由于 switch 在找到匹配项后会停止评估，因此脚本编写为先评估最受欢迎的项目，从而优化 switch 的功能。

## Summary

在本章中，您学习了可以优化执行速度的不同脚本技术。您首先学习了如何通过利用 `measure-command` cmdlet 来测量脚本执行持续时间。然后您比较了将数据写入屏幕和将操作保留在内存中之间的时间差异。然后您学习了一种在运行时优化脚本执行的技术，即打开和关闭屏幕数据写入。您继续评估 `write-progress` cmdlet 以及它对性能的影响。通过评估 `if` 语句和 `switch` 语句之间的性能差异，您完成了本章。

在下一章中，您将学习如何利用正则表达式进行字符串匹配，以显著提高脚本性能。

## 11

## 提高性能以使用正则表达式

当您使用 Windows Server 扫描脚本评估系统时，可能需要找到与特定值不匹配的敏感数据。信用卡号、社会安全号码，甚至税务识别号码都是您需要识别但仅使用字符串搜索就不可能搜索的数据的绝佳示例。

正则表达式提供了模式匹配，可以识别您通常无法通过字符串搜索找到的数据。正则表达式非常动态，可以用于检测脚本中所需的任何模式。事实上，正则表达式可以如此精细，以至于可以搜索单词的一部分。这意味着您可以在 `MyPasswordIsString` 字符串中找到 `Password` 单词。

正则表达式也非常快。与使用多个数组和 `if` 语句相比，正则表达式通常比其他技术快超过 50%！这就是开发人员选择利用正则表达式来优化其代码并提供强大的搜索平台的原因。

在本章中，您将：

- 学习组织中常用的正则表达式
- 利用比较运算符使用正则表达式
- 动态构建脚本中使用的正则表达式
- 评估使用正则表达式与多个 `foreach` 循环的性能增益

