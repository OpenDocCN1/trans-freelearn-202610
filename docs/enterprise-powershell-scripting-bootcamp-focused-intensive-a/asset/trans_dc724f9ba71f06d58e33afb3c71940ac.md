
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示适当的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：

- 1. 你首先通过创建名为 `scan-directory` 的函数开始。
- 2. 你创建一个参数块，将第一个参数设为 `$directory`。
- 3. 为了启动函数，你通过将 `$errors` 设为 `@()` 来声明 `$errors` 数组。
- 4. 你使用 `$directory` 参数调用 `Get-ChildItem` cmdlet，使用 `$include` 参数的 `-Include` 参数，以及使用 `$exclude` 参数的 `-exclude` 参数。
- 5. 你继续包含用于子目录扫描的 `-recurse` 参数，将 `-ErrorAction` 参数设为 `SilentlyContinue` 参数，并将 `-ErrorVariable` 设为 `+errors` 加法运算符。
- 6. 然后 `Get-ChildItem` cmdlet 语句被管道传输到 `select-string` cmdlet，使用 `$findword` 参数、`-SimpleMatch` 参数以及 `SilentlyContinue` 的 `-ErrorAction`。
- 7. 此部分将查询 $directory 位置及其子目录中 $include 中指定的文件类型，排除 $exclude 中的文件，并选择具有 -Simplematch $findword 的字符串。
- 8. -ErrorAction 应用于 select-string cmdlet，以抑制无法访问的文件和文件夹或长度超过 248 个字符的错误。
- 9. 这些错误已通过 Get-ChildItem cmdlet 添加到 $errors 变量中。
- 10. 此命令的结果随后存储在 $content 变量中。
- 11. 在 Get-ChildItem cmdlet 完成后，你评估 $errors 变量以确定其中是否有任何值。
- 12. 由于你创建了一个长度超过 248 个字符的文件夹结构，$errors 变量中将有数据，导致 if 语句求值为 $true。然后脚本继续执行 foreach 循环以评估 $errors 数组中的每个 $err。
- 13. 然后脚本执行一个 if 语句，以确定 $err.Exception 属性是否类似于 "*Could not find a part of the path*"。
- 14. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 15. 你评估字符串以按撇号字符将其拆分。
- 16. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 17. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path:""$filepath"" may be over 248 Characters"。如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示正确[content]
你继续对 $errors 变量进行第二次评估。
- 18. 在此评估期间，你确定 $err.Exception 属性是否类似于 "*is denied.*" 。这会捕获系统上可能发生的访问被拒绝错误消息。
- 19. 如果此求值为 true，脚本将获取 $err.Exception 属性并将 Exception 转换为字符串。
- 20. 然后你评估字符串以按撇号字符将其拆分。
- 21. 你指定 [1] 来选择第二个值，即文件名，并将其设为 $filepath 变量。
- 22. 你使用 Write-Host cmdlet 并使用字符串 "Error Accessing Path: \"$filepath\" Access Is Denied." 如果 $errors 变量中的此特定 $err 匹配，它将在屏幕上显示正确的信息。
- 23. 在错误处理部分之后，你继续评估 $content 变量中的每个 $match。
- 24. 你使用 Write-Host cmdlet 显示 "Filename: " 和 $match.FileName 属性。
- 25. 你对该结果使用 .Trim() 方法以移除首尾空格。这会显示匹配项所在的文件名。
- 26. 然后你使用 Write-Host cmdlet 显示 "Line Number: " 和 $match.LineNumber 属性。这会显示匹配项所在的[content]
你继续对 $errors 变量进行第二次评估。
- 18. 在此评估中，你判断 $err.Exception 属性是否像 "*is denied.*" ，以捕获可能的访问被拒绝错误。
- 19. 若为 true，脚本取 $err.Exception 属性并转为字符串。
- 20. 然后按撇号字符拆分该字符串。
- 21. 指定 [1] 选择第二个值即文件名，赋给 $filepath。
- 22. 使用 Write-Host 输出 "Error Accessing Path: \"$filepath\" Access Is Denied." 匹配时显示该信息。
- 23. 错误处理后，遍历 $content 中的每个 $match。
- 24. 用 Write-Host 显示 "Filename: " 和 $match.FileName。
- 25. 使用 .Trim() 去除首尾空格，显示匹配文件。
- 26. 用 Write-Host 显示 "Line Number: " 和 $match.LineNumber。
- 27. 最后用 Write-Host 显示 "Line Contents: " 和 $match.line。
- 28. 使用 .Trim() 去除首尾空格，显示匹配行内容。

函数创建完成。随后设置函数调用：
- 1. 搜索 *.xml 和 *.txt，定义 "*.xml","*.txt" 放入 $include。
- 2. $exclude 设为空 ""，不排除任何项。
- 3. 定义搜索词 "Complete" 放入 $findword，最后调用 scan-directory "c:\windows\system32"。
- 4. 调用后 scan-directory 用 Get-ChildItem 判断 $directory 中是否存在 $include 扩展名的文件。
- 5. $exclude 为空不排除任何值，然后通过管道传给 select-string 判断是否 -SimpleMatch 包含 $findword "Complete"。
- 6. 函数报告 "access or paths exceeding 248 character" 消息。
- 7. 函数打印符合条件的[content]
为了创建一个函数来扫描系统中 .xml 和 .txt 文件中是否包含单词 "Complete"，先不使用排除项，再使用排除项，可以执行以下操作：

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

以下截图显示了此操作的输出：

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

此示例展示了如何扫描目录结构中 .xml 和 .txt 文件以查找单词 "Complete"。它还展示了如何成功排除扫描函数返回的结果：
