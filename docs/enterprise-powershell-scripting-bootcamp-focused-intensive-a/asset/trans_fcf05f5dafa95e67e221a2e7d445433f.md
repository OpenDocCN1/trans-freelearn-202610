
## 9 文件扫描

当你在评估环境中的系统时，可能需要扫描文件以查找特定内容。Windows 服务器扫描脚本使你能够搜索目录中包含特定字符串的文件。常见字符串可能包括错误消息、明文密码，甚至是主机名和 IP 地址等特定服务器信息。扫描系统以查找特定内容的能力是一项极其强大的工具，可以揭示有关系统的诸多信息。

扫描系统时，出现大量并非所需的结果是很常见的，这也被称为误报。本章将探讨如何通过将搜索范围缩小到特定文件类型来减少误报数量。它还将解释如何从搜索中排除文件，以帮助你获取所需的数据。

在本章中，你将：

-   了解如何获取子目录及其内容
-   将文件发现过程限制为特定文件类型
-   高效地在文件中搜索字符串并确定其位置
-   学习如何处理超过 248 个字符的长路径和权限问题导致的错误
-   排除搜索中的误报文件

### 文件扫描

文件扫描是企业环境中的一项重要活动。无论是为了识别凭据的使用位置、敏感信息的使用，还是服务器配置信息，你都需要一种快速高效地扫描文件数据的技术。PowerShell 原生具有从明文文件收集数据并解析信息的能力。最常见的明文文件包括但不限于文本文件、日志文件、XML 文件、配置文件和脚本文件。

在开发扫描函数时，你需要扫描目录中的特定文件类型以及文件本身的内容。要扫描特定文件类型，可以使用文件夹路径作为参数利用 Get-ChildItem cmdlet。你还使用 -Include 参数并指定要搜索的通配符文件扩展名。对于日志和文本文件，通配符文件扩展名分别看起来像 *.log 和 *.txt。如果要扫描多个扩展名，可以在每个要搜索的文件扩展名之间放置逗号。如果要扫描子目录中的文件类型，可以包含 -Recurse 参数以递归遍历子目录。

要扫描 Program Files 目录及其子目录中的 *.log 和 *.txt 文件，可以执行以下操作：

```
Get-ChildItem "c:\Program Files\" -Include *.log,*.txt -recurse
```

以下屏幕截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_137_0.png)

此示例演示如何扫描 Program Files 目录中的 .log 和 .txt 文件类型。你首先使用 C:\Program Files\ 参数调用 Get-ChildItem cmdlet。你还使用 *.log,*.txt 参数利用 -Include 参数来扫描 .log 和 .txt 文件。最后，你利用 -recurse 参数来扫描 C:\Program Files\ 的子目录。执行后，PowerShell 脚本将显示 C:\Program Files\ 目录中的所有 .log 文件和 .txt 文件。

要扫描文件内部的特定内容，需要获取文件内容并将其放入内存。为此，可以使用要扫描的文件路径作为参数利用 get-content cmdlet。然后将结果放入变量中进行评估。要扫描文件内容，可以利用 select-string cmdlet，它允许将内容字符串与正则表达式匹配。你首先定义 select-string cmdlet，然后跟上带有文件参数的 -path 参数。然后使用正则表达式作为参数调用 -pattern 参数。要搜索字面字符串，可以使用字符串作为参数的 -pattern 参数，并利用 -SimpleMatch 参数来匹配特定字符串。

你也可以选择在管道后利用 select-string cmdlet。要扫描文件内部的特定内容，可以将 Get-ChildItem cmdlet 的文件名结果通过管道传递给 select-string cmdlet。然后 select-string cmdlet 将对各个文件进行字符串模式评估。这避免了使用 foreach 循环查询各个文件的需要，并且每个文件在由 Get-ChildItem cmdlet 返回时都会被评估。

select-string cmdlet 的另一个独特功能是它还会返回文件中匹配的行号，以及找到各个结果时匹配内容的整行。你可以调用 Line 属性返回整行，并调用 LineNumber 属性返回行号。你还可以调用 FileName 属性返回找到条件的特定文件。

要搜索 Program Files 中的 .log 和 .txt 文件并扫描其中的字符串，可以执行以下操作：

```
$matches = Get-ChildItem "c:\Program Files\" -Include *.log,*.txt -recurse
| select-string -pattern "Complete" -SimpleMatch
foreach ($match in $matches) {
    Write-Host "Filename: " $match.FileName
    Write-Host "Line Number: " $match.LineNumber
    Write-Host "Line Contents: " $match.Line
}
```

以下屏幕截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_139_0.png)

此示例演示如何扫描 Program Files 目录中的 .log 和 .txt 文件，并搜索文件中的字符串 "Complete"。你首先使用 c:\Program Files\ 参数调用 Get-ChildItem cmdlet。你还将 -Include 参数设置为 *.log,*.txt 参数，并使用 -recurse 参数搜索子目录。然后将结果通过管道传递给 select-string cmdlet，并将 -pattern 参数设置为 "Complete" 字符串作为参数。然后发出 -SimpleMatch 参数以对该字符串执行简单搜索。这些命令的输出随后存储在 $matches 变量中。

然后继续利用 foreach 循环评估 $matches 中的每个 $match。然后利用 Write-Host cmdlet 显示 "FileName:" 以及 $match 变量的 .FileName 属性。你还使用 Write-Host cmdlet 显示 "Line Number:" 以及 $match 变量的 .LineNumber 属性。最后，你使用 Write-Host cmdlet 显示 "Line Contents:" 以及 $match 变量的 .Line 属性。

执行后，脚本将已扫描 c:\Program Files\ 目录中的 .log 和 .txt 文件。当识别出正确文件类型的文件时，脚本将评估该文件是否包含 "Complete" 字符串。如果找到该字符串，脚本将继续提供各个文件中的文件名、行号和该行的内容。

### 排除长路径

在企业文件服务器中，你可能会遇到文件夹结构和文件字符数超过 248 个字符的情况。Windows 默认将长文件路径的使用限制为 248 个字符长度。拥有基于 Linux 的文件服务器或映射网络驱动器，其文件路径超过 248 个字符限制的情况并不少见。

PowerShell 与 Windows 操作系统具有相同的路径限制。所有 PowerShell cmdlet 在查询文件夹路径长于 248 个字符的文件系统时都会出现问题。有一些程序例如 robocopy.exe 可以用来显示长于 248 个字符的文件和文件夹的完整路径。虽然你有返回长路径的能力，但仍无法使用 PowerShell cmdlet 与该目录中的任何文件进行交互。此外，与 Get-ChildItem cmdlet 相比，robocopy.exe 速度非常慢，并且需要在其上运行的系统上具有管理员备份权限。

这使你与长文件和文件夹路径交互的选项变得有限。因此，建议你在脚本中处理文件路径并访问错误消息。使用 Get-ChildItem cmdlet 处理错误是通过使用 -ErrorAction 和 -ErrorVariable 参数完成的。然而，与其为错误创建单个变量，你需要将错误放在数组中。这是因为 Get-ChildItem cmdlet 会获取指定路径中的所有文件和目录，即使它存在访问问题或由于路径长度无法访问该路径。如果你只使用一个变量，错误变量中唯一的错误将是最后发生的那个。相反，为了规避这个问题，你可以利用 $errors = @() 定义一个数组，并将 $errors 用作 -ErrorVariable 的参数。这告诉 PowerShell 在发生错误时向 $errors 数组添加一个额外项。

> 要执行以下示例，你需要在系统上创建一个名为 "Brenton" 的本地用户。或者，你可以将反映 "$env:computername\Brenton" 的行更新为你想要的任何用户名。

要创建超过 248 个字符的共享目录结构，收集文件和目录信息并捕获错误，可以执行以下操作：

```
# Create Folder Structure to Scan
$userdrive = "c:\Temp\POSHScript\Chapter9Examples\CompanyXYZ\Milwaukee\InformationTechnologyDepartment\UserHomeDrives\UserLoginID\"
new-item $userdrive -ItemType Directory -Force
$domainuser = "$env:computername\Brenton"
New-SmbShare -name "UserData" -Path $userdrive -FullAccess $domainuser
New-PSDrive -Name H -root "\\$env:computername\Userdata" -Persist -PSProvider FileSystem
cd H:
$userFolder = "Information Technology Department\All Company Software\ISO\Microsoft\Microsoft SQL Server 2012 R2\SQL Server Update Patches\Service Pack 3\Cumulative Update 7\AutomatedWindowsInstaller\"
```
