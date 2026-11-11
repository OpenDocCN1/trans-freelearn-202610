
## 答案文件读取函数

你继续通过定义答案文件读取函数、将 XML 加载到内存中并删除答案文件来完善扫描脚本。你可以选择注释掉 Remove-Item cmdlet 步骤。这将在你的环境中提供测试，而无需在脚本目录中重新创建 XML 文件。

要创建 XML 读取函数、将 XML 加载到内存中并删除 XML 文件，可以执行以下操作：

```
Function read-xmltag { param ($xmlextract)
return $xml.GetElementsByTagName("$xmlextract")
}

$xmlfile ="$PSScriptRoot\Scan_Answers.xml"

$test = test-path $xmlfile

if (!$test) {
    Write-Error "$xmlfile not found on the system. Select any key to exit!"
    PAUSE
    exit
}

[xml] $xml = get-content $xmlfile
Remove-Item $xmlfile -force
```

创建此函数时没有输出可显示。

在此示例中，你定义答案文件读取函数：

- 1. 你首先通过 param ($xmlextract) 的参数块定义 read-xmltag 函数。
- 2. 然后你利用 $xml.GetElementsByTagName() 方法获取 $xmlextract 中指定的 xml 标签。
- 3. 你将结果返回给调用该函数的脚本部分。然后你继续通过调用 $PSScriptRoot 并将其值与 "\Scan_Answers.xml" 组合来获取脚本的当前路径。你将结果存储在 $xmlfile 变量中。
- 4. 你继续通过使用 $xmlfile 参数的 test-path cmdlet 来确定文件是否存在。
- 5. 你在 $test 变量上声明一个如果不存在的语句，如果文件不存在，则使用 write-error cmdlet 显示 "$xmlfile not found on the system. Select any key to exit."。
- 6. 然后你发出 PAUSE 语句以停止脚本，并在键盘按下键后发出 exit 语句以退出脚本。如果答案文件存在，脚本将继续将 XML 文件加载到内存中。
- 7. 你使用 $xmlfile 参数的 get-content cmdlet。
- 8. 然后你将此值放入具有 [xml] 数据类型的 $xml 变量中。
- 9. 在将 XML 文件加载到内存后，你通过使用 $xmlfile 参数和 -Force 参数的 Remove-Item cmdlet 删除 XML 文件。答案文件将被从系统中删除。

## 解密函数

在完成将 XML 文件加载到内存后，下一步是定义解密函数。由于答案文件包含编码和加密的值，你需要此函数将加密值转换为字符串。

要声明解密函数，可以执行以下操作：

```
Add-Type -AssemblyName System.Security
function Decrypt-String { param($Encrypted)
    if($Encrypted -is [string]){
        $Encrypted = [Convert]::FromBase64String($Encrypted)
    }
    $r = new-Object System.Security.Cryptography.RijndaelManaged
    $pass = [System.Text.Encoding]::UTF8.GetBytes($pass)
    $salt = [System.Text.Encoding]::UTF8.GetBytes($salt)
    $init = [System.Text.Encoding]::UTF8.GetBytes($init)
    $r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 50000).GetBytes(32)
    $r.IV = (new-Object Security.Cryptography.SHA1Managed).ComputeHash($init)[0..15]
    $d = $r.CreateDecryptor()
    $ms = new-Object IO.MemoryStream @($Encrypted)
    $cs = new-Object Security.Cryptography.CryptoStream $ms,$d,"Read"
    $sr = new-Object IO.StreamReader $cs
    try {
```
```
$result = $sr.ReadToEnd()
$sr.Close()
$cs.Close()
$ms.Close()
$r.Clear()
Return $result
}
Catch {
    Write-host "Error Occurred Decrypting String: Wrong String Used In Script."
}
}
```

创建此函数时没有输出可显示。

在此示例中，你声明解密函数：

- 1. 你首先通过 Add-Type cmdlet 加载 System.Security 程序集。
- 2. 然后你使用 param($encrypted) 参数块声明 Decrypt-String 函数。
- 3. 你继续检查 $encrypted 值是否为字符串，如果是则将其转换为字节。
- 4. 然后你创建 System.Security.Cryptography.RijndaelManaged 对象并将其放入 $r 变量中。
- 5. 你获取 $pass 的字节并将其放入 $pass 变量中，获取 $salt 的字节并将其放入 $salt 变量中，并获取 $init 的字节并将其放入 $init 变量中。你继续使用 $pass 和 $salt 变量生成密钥并将此值放入 $r.Key 属性中。
- 6. 你使用 $init 生成 IV 并将值放入 $r.IV 属性中。
- 7. 你继续创建解密器对象并将其放入 $d 变量中，使用 $encrypted 值的内存流对象放入 $ms 变量中，使用 $ms 和 $d 变量的 CryptoStream 对象放入 $cs 变量中，并使用 $cs 变量的流读取器对象放入 $sr 变量中。
- 8. 然后你发出 try...catch 命令以捕获解密过程中发生的任何错误。
- 9. 你继续在流读取器上使用 ReadToEnd() 方法并将解密后的文本放入 $result 变量中。
- 10. 你关闭 $sr、$cs 和 $ms 对象。
- 11. 你清除 $r 变量并将 $result 的结果返回给调用该函数的脚本部分。
- 12. 如果解密过程中出现错误，脚本将利用 write-host cmdlet 显示 "Error Occurred Decrypting String: Wrong String Used In Script"。

## 在变量和数组中填充脚本答案

Windows 服务器扫描脚本中的下一步是将答案文件中的数据填充到变量和数组中。当你访问内存中的答案文件数据时，你需要操作数据以供脚本使用。例如，搜索数据将具有需要解密的加密值，忽略列表需要制作为正则表达式，搜索扩展名需要放入数组中。

由于你将为脚本动态创建多个正则表达式，最佳做法是创建一个生成正则表达式的函数。脚本可以在文件搜索数据、文件忽略列表、搜索忽略列表和内置用户列表中没有值的情况下运行。你需要验证传递给函数的列表是否不为空。空白或未填充任何值的正则表达式将匹配所有值为 true。这将导致脚本执行问题，应避免这种情况。

为了解决此问题，如果你检测到列表为空，则将列表值设置为系统中不会匹配的字符串，例如 No_Input_Provided_For_List_Item。当使用该表达式评估正则表达式时，该表达式与条件匹配的可能性极低。

要创建动态创建正则表达式的函数，可以执行以下操作：

```
```powershell
function create-SearchRegEx { param($list)
    if (! $list) { $list = "No_Input_Provided_For_List_Item" }
    $RegEx = '(?i)^.*(' + (($list | % {[regex]::escape($_)}) -join "|") + ').*$'
    return $regex
}
```
```

创建此函数时没有输出可显示。

在此示例中，你创建一个名为 create-SearchRegex 的新函数，参数块为 param($list)：

- 1. 你创建一个 if 语句来确定 $list 变量是否没有值。如果为空，它将把 $list 变量设置为 "No_Input_Provided_For_List_Item"。
- 2. 该函数将继续动态生成不区分大小写的正则表达式，并将匹配 $list 变量中列出的任何值。
- 3. 创建正则表达式后，值存储在 $regex 变量中。
- 4. 脚本将把 $regex 中的值返回给调用该函数的脚本部分。

与正则表达式类似，你将在脚本中多次进行解码，并且需要一个解码函数。要创建用于解码值的函数，可以执行以下操作：

```
function decode-string { param($string)
    $encbytes = [System.Convert]::FromBase64String($string)
    $string = [System.Text.Encoding]::Unicode.GetString($encbytes)
    return $string
}
```

创建此函数时没有输出可显示。

在此示例中，你创建用于解码值的函数：

- 1. 你首先通过参数块设置为 param($string) 来定义 decode-string 函数。
- 2. 然后你将字符串转换为字节并将输出存储在 $encbytes 变量中。
- 3. 然后你通过调用 [System.Text.Encoding]::Unicode.GetString($encbytes) 解码字节并将输出存储在 $string 变量中。
- 4. 脚本将把 $string 中的值返回给调用该函数的脚本部分。
