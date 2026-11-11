
## 解密字符串

字符串的解密与您在加密字符串时执行的过程非常相似。与将数据写入内存流不同，该函数从内存流中读取数据。此外，与使用 .CreateEncryptor() 方法不同，解密过程利用 .CreateDecryptor() 方法。

要创建使用 RijndaelManaged 加密解密加密字符串的脚本，请执行以下操作：

```powershell
Add-Type -AssemblyName System.Security
function Decrypt-String { param($Encrypted, $pass,
$salt="CreateAUniqueSalt", $init="CreateAUniqueInit")

if($Encrypted -is [string]){
    $Encrypted = [Convert]::FromBase64String($Encrypted)
}

$r = new-Object System.Security.Cryptography.RijndaelManaged
$pass = [System.Text.Encoding]::UTF8.GetBytes($pass)
$salt = [System.Text.Encoding]::UTF8.GetBytes($salt)
$init = [System.Text.Encoding]::UTF8.GetBytes($init)

$r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass,
$salt, "SHA1", 50000).GetBytes(32)
$r.IV = (new-Object Security.Cryptography.SHA1Managed).
ComputeHash($init)[0..15]

$d = $r.CreateDecryptor()
$ms = new-Object IO.MemoryStream @(, $Encrypted)
$cs = new-Object Security.Cryptography.CryptoStream $ms,$d,"Read"
$sr = new-Object IO.StreamReader $cs

try {
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
Decrypt-String "hK7GHaDD1FxknHu03TYAPxbFAAZeJ6KTSH1nSCPpJ7c=" "A_Complex_Password_With_A_Lot_Of_Characters".
```

此脚本的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_67_0.png)

此函数演示了如何利用 RijndaelManaged 加密算法解密字符串。您首先通过利用 Add-Type cmdlet 导入 System.Security 程序集，使用 -AssemblyName 参数并指定 System.Security 参数。然后您声明 Decrypt-String 函数。您包含一个参数块以接受并为函数设置值。第一个值是 $Encrypted，即加密文本。第二个值是 $pass，用于加密密钥。第三个是预定义的 $salt 变量，设置为 "CreateAUniqueSalt"。然后您定义 $init 变量，其设置为 "CreateAUniqueInit"。

在参数块之后，您通过使用 if ($Encrypted -is [string]) { 检查加密值是否格式化为字符串。如果此条件评估为 True，您将使用 [Convert]::FromBase64String($Encrypted) 将字符串转换为字节，并将编码值放入 $Encrypted 变量中。接下来，您使用 new-Object cmdlet 并传入 System.Security.Cryptography.RijndaelManaged 参数来声明解密类。您将此对象放入 $r 变量中。

然后您将 $pass、$salt 和 $init 值转换为 UTF8 字符编码标准，并将字符字节值存储在变量中。这是通过为 $pass 变量指定 [System.Text.Encoding]::UTF8.GetBytes($pass)、为 $salt 变量指定 [System.Text.Encoding]::UTF8.GetBytes($salt)，以及为 $init 变量指定 [System.Text.Encoding]::UTF8.GetBytes($init) 来完成的。

在设置正确的字符编码后，您继续为 RijndaelManaged 加密算法创建加密密钥。这是通过将 RijndaelManaged $r.Key 属性设置为由 (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 50000).GetBytes(32) 创建的对象来完成的。此对象利用 Security.Cryptography.PasswordDeriveBytes 类，并使用 $pass 变量、$salt 变量、"SHA1" 哈希名称以及迭代派生 50000 次来创建密钥。此类的每次迭代都会生成不同的密钥值，使密钥更难以猜测。然后您利用 .get-bytes(32) 方法返回密钥的 32 字节值。

为了为算法创建初始化向量，您将 RijndaelManaged $r.IV 属性设置为由 (new-Object Security.Cryptography.SHA1Managed).ComputeHash($init)[0..15] 创建的对象。代码的这一部分利用 Security.Cryptography.SHA1Managed 类并基于 $init 值计算哈希。当您调用 [0..15] 范围运算符时，哈希的前 16 个字节将被获取并放入 $r.IV 属性中。

在设置所需属性后，您现在准备开始解密数据。您首先利用具有已定义 $r.key 和 $r.IV 属性的 $r RijndaelManaged 对象。您使用 $r.CreateDecryptor() 方法生成解密器。

生成解密器后，您必须创建一个内存流以在内存中进行解密。这是通过声明 new-Object cmdlet 并传入 IO.MemoryStream 类参数来完成的。然后您使用 @(, $Encrypted) 将 $encrypted 值引用到内存流对象中，并将填充的内存流存储在 $ms 变量中。

接下来，您创建 CryptoStream，用于将加密数据转换为解密数据。您首先声明 new-Object cmdlet 并传入 Security.Cryptography.CryptoStream 类参数。您还定义 $ms 的内存流、$d 的解密器以及 "Read" 操作符，以告诉该类从内存中的加密流读取加密数据。

创建 CryptoStream 后，您准备好从 CryptoStream 读取解密数据。这是通过使用 IO.StreamReader 类来完成的。您使用 IO.StreamReader 类参数声明 new-Object，并定义要读取的 $cs CryptoStream。

此时，您使用 try { 来捕获从 StreamReader 读取数据时生成的任何错误消息。您调用 $sr.ReadToEnd()，它调用 StreamReader，读取完整的解密值，并将数据放入 $result 变量中。为了停止向 CryptoStream 和 MemoryStream 读取数据，您使用 $sw.Close() 关闭 StreamWriter，使用 $cs.Close() 关闭 CryptoStream，并使用 $ms.Close() 关闭内存流。出于安全目的，您还通过声明 $r.Clear() 清除解密器数据。如果解密成功，您将 $result 的值返回给脚本。

在定义解密函数后，您调用该函数以供使用。您首先通过调用 Decrypt-String 后跟 "hK7GHaDD1FxknHu03TYAPxbFAAZeJ6KTSH1nSCPpJ7c=" 来开始。您还将第二个参数声明为解密器的密码，即 "A_Complex_Password_With_A_Lot_Of_Characters"。执行后，您将收到由该函数生成的解密值 "Encrypt This String"。

## 保护密码

| 脚本侧解密器 SSD | 答案文件解密器 AFD | 运行时解密器 RTD |
|---|---|---|
| A_Complex_Pass + | word_With_A_Lot + | _Of_Characters |

您加密方法中最重要的部分之一是保护用于加密的密码。一种推荐的保护密码的方法是将密码拆分为三个或更多位置。前面的图表将密码 A_Complex_Password_With_A_Lot_Of_Characters 拆分为三个独立的段。目的是使密码数据部分位于脚本本身中，部分位于答案文件中，其余部分在运行时作为参数馈送到脚本中。如果三个密码段中的任何一个未提供，加密过程将因密码不正确而失败。

这种方法的目的是为密码提供多个数据点。运行时解密器需要用户干预，答案文件解密器需要在执行时存在答案文件，而脚本侧解密器确保密码不能仅从答案文件中确定。

![](img/a27a138d6586c10902d8e86ab3384834_70_0.png)

对此方法的一项额外安全层是利用密码输入的编码。虽然编码可能不适合加密脚本中的实际密码，但它为在多个位置存储密码提供了非常有价值的混淆。当您为密码生成编码值时，它会增加字符数量并极大地阻碍对加密过程中使用的编码和密码的暴力破解。

要编码密码，请执行以下操作：

```
$pass = "A_Complex_Password_With_A_Lot_Of_Characters"
$encodedpass = [System.Text.Encoding]::Unicode.GetBytes($pass)
$encodedvalue = [Convert]::ToBase64String($encodedpass)
$encodedvalue
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_70_1.png)

此示例演示了如何编码拆分为多个数据点的密码。您首先通过将值 "A_Complex_Password_With_A_Lot_Of_Characters" 定义为 $pass 变量来开始。然后您利用 [System.Text.Encoding]::Unicode.GetBytes($pass) 方法对密码进行编码，并将值存储在 $encodedpass 变量中。然后您通过利用 [Convert]::ToBase64String($encodedpass) 将编码值转换为字符串，并将值存储在 $encodedvalue 变量中。在此示例的末尾，您调用 $encodedvalue 变量以显示编码字符串。
