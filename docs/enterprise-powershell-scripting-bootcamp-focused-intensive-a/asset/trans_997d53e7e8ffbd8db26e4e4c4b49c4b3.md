
```
new-item $userFolder -ItemType Directory -Force
```

## 扫描文件夹结构
$directory = "c:\Temp\POSHScript\Chapter9Examples\CompanyXYZ\"
$errors = @()
get-childitem $directory -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors
if ($errors) {
foreach ($err in $errors) {
if ($err.Exception -like "*Could not find a part of the path*") {
    $filepath = ($err.Exception).ToString().split("'")[1]
    Write-Host "Error Accessing Path: "$filepath" may be over 248 Characters."
}
if ($err.Exception -like "*is denied.*") {
    $filepath = ($err.Exception).ToString().split("'")[1]
    Write-Host "Error Accessing Path: "$filepath" Access Is Denied."
}
}
}

## 移除映射驱动器和共享
cd c:
Remove-PSDrive -Name H -Force
Remove-SmbShare -name "UserData" -Force

此输出显示在以下屏幕截图中：

![](img/a27a138d6586c10902d8e86ab3384834_141_0.png)

此示例展示了如何在捕获访问被拒绝和长文件路径错误的同时，正确地查询系统以获取文件和文件夹信息：

- 1. 你首先通过创建所需的文件夹结构开始。由于无法创建超过 248 个字符长的文件夹结构，你创建一组较小的目录，共享这些目录，将驱动器映射到该目录路径，并使用映射的驱动器位置创建额外的文件夹。其输出是一个超过 248 个字符的目录结构。
- 2. 要创建第一个目录结构，你定义字符串 "C:\Temp\POSHScript\Chapter9Examples\CompanyXYZ\Milwaukee\InformationTechnologyDepartment\UserHomeDrives\UserLoginID\" 并将其设置为 $userdrive 变量。
- 3. 然后你利用 new-item cmdlet 以 $userdrive 变量作为参数，-ItemType 参数值为 Directory，以及 -Force 参数。这会创建第一个目录结构。
- 4. 要创建目录共享，你必须指定共享将与之共享的凭据。你指定 $env:computername 来表示计算机名称，后跟反斜杠、Brenton 用户，并将值放在 $domainuser 变量中。
- 5. 你通过利用 New-SmbShare cmdlet 将此目录结构共享，其 -name 参数设置为 "UserData"。你还包含 -Path 参数并指定 $userdrive 参数，以及 -FullAccess 参数并使用 $domainuser 参数。

执行后，"c:\Temp\POSHScript\Chapter9Examples\CompanyXYZ\Milwaukee\InformationTechnologyDepartment\UserHomeDrives\UserLoginID\" 在系统上共享，可以通过 UNC 路径 \\computername\Userdata 访问。

- 6. 然后你继续通过利用 New-PSDrive cmdlet 映射 H 驱动器，-Name 参数为 H 作为参数。然后你指定 -root 参数并指定 \\\$env:computername\Userdata 作为参数。你还包含 -Persist 参数以使映射持久化，以及 -PSProvider 参数设置为 FileSystem。执行后，你将 H:\ 驱动器映射到 \\computername\Userdata。
- 7. 然后你使用 cd H: 发出对 H: 驱动器的目录更改并继续构建目录结构。
- 8. 你构建字符串 "Information Technology Department\All Company Software\ISO\Microsoft\Microsoft SQL Server 2012 R2\SQL Server Update Patches\Service Pack 3\Cumulative Update 7\AutomatedWindowsInstaller\" 并将其放在 $userFolder 变量中。
- 9. 然后你利用 new-item cmdlet 以 $userFolder 作为参数，-ItemType 参数值为 Directory，以及 -Force 参数。

这会在 H: 驱动器中创建第二组目录。然后你继续扫描文件夹结构。

- 10. 你首先通过定义要在 C:\Temp\POSHScript\Chapter9Examples\CompanyXYZ\ 中扫描的目录并将其设置为 $directory 变量开始。
- 11. 然后你通过将其设置为 @() 的值来定义 $errors 数组。
- 12. 你利用 Get-ChildItem cmdlet 使用 $directory 参数和 -recurse 参数扫描 C:\Temp\POSHScript\Chapter9Examples\CompanyXYZ\ 目录及其子目录。你利用 -ErrorAction 参数值为 SilentlyContinue 以及 -ErrorVariable 参数值为 +errors。这为一系列错误消息启用了累加操作，因为它们会发生到 $errors 数组中。
- 13. Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 14. 由于你创建了一个超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句评估为 $true。
- 15. 然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 16. 然后脚本执行 if 语句以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。如果 Get-ChildItem 知道路径存在但无法进一步遍历目录，则会指定此特定错误消息。这表明文件路径长于 248 个字符。
- 17. 如果评估为 true，脚本将获取 $err.Exception 属性并将异常转换为字符串。
- 18. 你评估字符串以按撇号字符将其拆分。
- 19. 你指定 [1] 以选择第二个值，即文件名，并将其设置为 $filepath 变量。
- 20. 你利用 Write-Host cmdlet 使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。
- 21. 如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的 message。你继续进行对 $errors 变量的第二次评估。
- 22. 在此评估期间，你确定 $err.Exception 属性是否类似于 "*is denied.*"。这会捕获系统上可能发生的访问被拒绝错误消息。
- 23. 如果评估为 true，脚本将获取 $err.Exception 属性并将异常转换为字符串。
- 24. 你评估字符串以按撇号字符将其拆分。
- 25. 你指定 [1] 以选择第二个值，即文件名，并将其设置为 $filepath 变量。
- 26. 你利用 Write-Host cmdlet 使用字符串 "Error Accessing Path: "$filepath" Access Is Denied."。
- 27. 如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的 message。
最终步骤是脚本的清理部分。
- 28. 为了清理脚本，你执行 cd c: 将目录更改回 c: 驱动器。
- 29. 你通过利用 Remove-PSDrive cmdlet 将 H 驱动器断开连接，-name 参数设置为 H 且 -Force 参数。
- 30. 你关闭 SMB 共享，利用 Remove-SmbShare cmdlet 将 -name 参数设置为 "UserData" 且 -Force 参数。

如果此脚本在服务器文件共享上执行，Get-ChildItem cmdlet 将报告 $directory 变量中的所有文件和目录。它还将打印到屏幕上，如果有任何目录长于 248 个字符，以及文件夹结构上是否生成任何访问被拒绝错误消息。

## 排除文件

当你在环境中执行扫描时，你最终希望过滤掉一些误报。这在你利用模板构建系统时尤其正确，因为可能会检测到大量共同点。你可能还希望过滤掉已知安全的非常大的文件以优化扫描持续时间。

Get-ChildItem cmdlet 包含 -exclude 参数，它使你能够指定要排除的对象字符串。-exclude 参数还支持用逗号分隔的多个值以及使用星号字符的通配符。首先，你指定 Get-ChildItem cmdlet 并以要扫描的目录作为参数。然后你提供 -exclude 参数并以文件作为其参数。当 Get-ChildItem cmdlet 执行时，它将过滤掉指定为 -exclude 参数参数的那些项。

