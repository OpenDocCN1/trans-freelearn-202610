
本输出在以下屏幕截图中显示：

![](img/a27a138d6586c10902d8e86ab3384834_198_0.png)

生成的合并 CSV 文件在以下屏幕截图中显示：

![](img/a27a138d6586c10902d8e86ab3384834_198_1.png)

在此示例中，您将所有创建的 CSV 文件合并为单个 CSV 文件：

- 1. 您首先通过声明 "C:\temp\POSHScript\CSVDEMO\" 并将其放入 $logloc 变量中来定义日志位置。
- 2. 然后您继续使用 Get-Date cmdlet 生成日期时间戳，并将 -format 参数设置为 "yyyyMMddhhmmss" 参数。
- 3. 您将此值存储在 $date 变量中。
- 4. 您通过将 "$logloc" + "Merged_$date.csv" 放入 $mergefile 变量来构建合并文件字符串。
- 5. 然后您通过使用 $mergefile 参数利用 New-Item cmdlet 创建合并文件，并将 -ItemType 参数设置为 File。
- 6. 您将这些结果通过管道传递给 Out-Null 以抑制文件创建通知。
- 7. 您继续从 $logloc 目录收集所有文件，以便在管道中使用 Import-Csv 将它们合并。
- 8. 您首先调用 Get-ChildItem cmdlet，将 $logloc 作为参数，并将 -Filter 参数设置为 "*.csv" 参数。
- 9. 您将结果放入括号语句中并调用 .FullName 属性以获取文件的全部完整路径名。
- 10. 您将所有这些结果通过管道传递给 Import-Csv cmdlet，以在内存中合并所有 CSV 文件。
- 11. 然后您将该 cmdlet 的输出通过管道传递给 Export-Csv cmdlet，将 $mergefile 作为参数，并将 -NoTypeInformation 作为参数。

执行此脚本后，您将收集 $logloc 目录中的所有 CSV 文件，合并 CSV 文件，并将合并结果导出到 $mergefile CSV 文件。

## 摘要

本章中，您回顾了 Windows 服务器扫描脚本的概述。您了解到需要在环境中实现脚本时遵循工作流程。您首先开发了一个脚本来生成随机字符串以创建加密密码、盐和初始化向量。然后在同一脚本中对结果进行编码以进一步保护值。创建编码后的密码、盐和初始化向量后，您使用这些值创建了一个加密脚本以开始加密字符串。加密字符串后，您继续了解脚本所需的答案文件。您了解了需要在答案文件中包含哪些项目以确保 Windows 服务器扫描脚本保持灵活。然后您继续探索终止文件以及它们如何用于远程优雅地停止扫描脚本。您创建了一个脚本，用于在本地以及使用 PSSession 创建和删除终端文件。最后，您开发了一个脚本，将 Windows 服务器扫描脚本的 CSV 结果合并到单个文件中以供审阅。

在下一章中，您将学习如何创建 Windows 服务器扫描脚本。您将创建注释块、参数块、答案文件读取函数和解密函数，然后填充答案文件并创建脚本日志和日志函数。您将创建用于扫描磁盘、计划任务、进程、Windows 服务、软件、用户配置文件、Windows 功能和文件目录的函数。您将通过学习执行后清理注意事项来完成本章。

## 13

## 创建 Windows 服务器扫描脚本和执行后清理

当您在大型企业中运行系统时，通常需要识别环境中不同服务器的信息。虽然您可以手动评估每台服务器上的项目以确定其角色，但您很可能希望自动化该过程。在阅读本书的过程中，您学习了使用 PowerShell 扫描和识别服务器上非标准配置的多个方面。您还学习了如何优化代码以显著提高脚本的性能。这些示例引导您进入本章，本章将您所学到的所有概念结合成一个 Windows 服务器扫描脚本。

Windows 服务器扫描脚本的开发目的是深入了解服务器在环境中的角色和功能。当您针对环境执行 Windows 服务器扫描脚本时，可以快速查看使服务器独特的非默认配置。当您执行大规模服务器发现以确定环境中服务器之间的共同点时，这也非常强大。建议采用此现有脚本并在此基础上构建，以满足您环境的独特需求。

本章中，您将：

- 了解整体脚本结构的概述
- 实现注释块、参数块、答案文件读取函数和解密函数
- 从答案文件填充变量和数组
- 创建一个 `logging` 函数，用于将日志记录到数据收集文件、`log` 文件和事件日志
- 实现脚本终止函数以优雅地退出脚本
- 集成并调用多个函数以扫描系统的不同区域
- 调用脚本的函数并将 `log` 文件复制到文件共享
- 探索如何从命令行启动脚本
- 了解执行后脚本清理活动

## Windows 服务器扫描脚本创建

当您开始构建自己的 Windows 服务器扫描脚本版本时，您需要遵循脚本结构。当您使用标准化的脚本结构时，您可以将代码组织到不同的部分。这使您可以快速在脚本中找到组件并添加到各个部分。它还为他人快速学习和修改您的脚本提供了便捷的平台。

以下是 Windows 服务器扫描脚本的组成部分：

**注释块**：在此处描述脚本的功能。

**参数块**：在此处您可以将变量和数据传递到脚本中，例如运行时解密器。

**答案文件读取函数**：这使您可以动态读取答案文件中的不同值。

**解密函数**：这使您能够在运行时解密加密字符串。

**将脚本答案填充到变量和数组中**：这将采用答案文件读取函数的输出来填充变量和数组。这还使您能够根据答案文件数据动态构建正则表达式。

**创建日志文件和日志函数**：这将使您能够记录脚本的不同操作，记录脚本找到的数据，将输出写入屏幕，甚至将事件写入事件日志。

**创建终止函数**：终止函数将使您能够多次检查系统上是否放置了终止文件。如果存在，则会终止扫描脚本。

**多个脚本函数**：在此处定义所有用于扫描系统不同组件的函数。

调用函数：当所有其他函数和组件都定义后，您通过调用函数（如果在答案文件中启用）来完成本章。

## 注释块

您的 Windows 服务器扫描脚本的第一行应该是注释块。注释块将描述脚本的功能并提供修订详细信息以及有关所需运行时解密器 (RTD) 的信息。本节应提供有关如何运行脚本的示例，以及有关脚本执行的任何其他说明。

以下屏幕截图显示了一个正确创建的注释块，该注释块与帮助系统集成：

```
<#
.SYNOPSIS
This is a server discovery script which will scan different server components to determine
the current configuration.
.DESCRIPTION
This script will scan processes, Windows services, scheduled tasks, server features, disk information,
registry, and files for pertinent server information.
Author: Brenton J.W. Blawat / Packt Publishing / Author / email@email.com
.PARAMETER RTD
This script requires a run time decryptor as a parameter to the script.
.EXAMPLE
powershellscript.ps1 /RTD "Run Time Decryptor"
.NOTES
You must have administrative rights to the server you are scanning. Certain functions will not work properly
without running the script as system or administrator.#>
```

## 参数块

在脚本的注释块之后，您需要立即声明参数块。参数块对于在脚本中捕获 RTD 是必需的。由于解密过程依赖于 RTD，如果该参数不可用，您将退出脚本。

要创建参数块并检查是否存在值，您可以执行以下操作：

```
param($RTD)
if (!$RTD) { exit }
```

声明参数块时没有输出。在此示例中，您使用 param 命令定义参数块，并使用脚本的输入填充 $RTD 变量。您还创建了一个 if 语句，如果 $RTD 变量为 $null，则会退出脚本。
