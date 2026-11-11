
一个 XML 答案文件的示例如下所示：

```
```xml
<?xml version="1.0"?>
<!-- XML Comment -->
<!-- XML Declaration -->

<!-- XML Parent Tag -->
<ScriptAnswers>

    <!-- XML Child Tag -->
    <ports port="21" name="FTP"></ports>

    <!-- XML Sibling Tags -->
    <ports port="25" name="SMTP"></ports>
    <ports port="53" name="DNS"></ports>
    <ports port="80" name="HTTP"></ports>
    <ports port="443" name="HTTPS"></ports>

<!-- XML Parent Closing Tag -->
</ScriptAnswers>
```
```

前面的图形显示了一个格式正确的 XML 答案文件。文件通过指定 XML 声明来打开。这必须是 XML 文件中的第一行，否则你会收到语法错误。它以 <?xml 开头，并且将属性 version 设置为属性值 1.0。声明随后以 ?> 关闭。脚本随后展示了 XML 注释的使用。注释的语法以 <!-- 开头，后跟注释，并以 --> 关闭。

答案文件继续声明父标签 <ScriptAnswers>。在父标签之后，声明了子标签 <ports。该标签将 port 属性设置为属性值 21，并将 name 属性设置为 FTP。标签通过声明 </ports> 来关闭。脚本继续使用 ports 的同级标签。它显示了 SMTP 的端口 25、DNS 的 53、HTTP 的 80 以及 HTTPS 的 443。最后，XML 文件以父关闭标签 </ScriptAnswers> 关闭。

要完成本章中的示例，你需要在 C:\Temp\POSHScript\ 位置创建一个名为 Answers.xml 的新 XML 文件。利用你选择的文本编辑器，将前面数据插入 Answers.xml 文件中。示例也是相互关联的，因此你希望按顺序执行本章，以确保所有代码示例都能正常工作。

你也可以参考本章的代码文件，以便快速创建前面描述的内容。

## 读取 XML 答案文件

PowerShell 能够原生读取和解析 XML 文件中的数据。这是通过将 XML 文件加载到 XML 文档对象中，使用 get-content cmdlet 来完成的。你首先通过将数据类型设置为 [xml] 来定义一个像 $xmlfile 这样的变量。然后你调用 get-content cmdlet，并将 -path 参数设置为 XML 文件的位置。如果你定义变量时省略了 [xml] 数据类型，它会将文件解释为文本文件。这意味着你将无法利用任何内置的 XML 支持。

加载你之前创建的 XML 答案文件，可以执行以下操作：

```
[xml] $xml = get-content "C:\temp\POSHScript\Answers.xml"
$xml
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_46_0.png)

此示例展示了如何正确获取 XML 文件的内容并将其放入 XML 格式的变量中。命令首先声明 [xml] 数据类型，然后是 $xml 变量。你随后利用 get-content cmdlet 并声明内容路径为 "C:\temp\POSHScript\Answers.xml"，内容存储在 $xml 变量中。最后，你调用 $xml 变量以显示 XML 格式的内容。

在检索到 XML 文件的内容后，你将希望检索单个 XML 标签，以便将配置信息存储在脚本的内存中。.GetElementsByTagName("TagName") 方法使你能够搜索名称为特定值的标签。这将返回 XML 标签的属性和元素。如果存在同名标签，或子标签有同级标签，该方法将返回所有等于方法中指定的标签值的值。

要检索 XML 文件的内容并检索特定的标签信息，可以执行以下操作：

```
[xml] $xml = get-content "C:\temp\POSHScript\Answers.xml"
$ports = $xml.GetElementsByTagName("ports")
$ports | Select Name, Port
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_47_0.png)

此示例展示了如何获取 XML 文件的内容，将其放入 XML 格式的变量中，并通过 XML 标签名称检索数据。命令首先声明 [xml] 数据类型，然后是 $xml 变量。你随后利用 get-content cmdlet 并声明内容路径为 "C:\temp\POSHScript\Answers.xml"，内容存储在 $xml 变量中。

你随后在 $xml 变量上使用 "ports" 参数利用 .GetElementsByTagName() 方法来获取名为 "ports" 的 XML 标签。执行此命令后，你将在 $ports 变量中拥有与标签名称 "ports" 匹配的子级和同级信息。然后你将 $ports 管道传输到 Select Name, Port，这将显示 Name 和 Port 属性的属性值。

> 一旦你调用 get-content cmdlet 并将 XML 数据放入变量中，你就不再处理 XML 文件中包含的数据。你将处理 XML 文件的内存副本。如果你更改 XML 文件中的任何内容，你需要重新运行 get-content cmdlet 以在内存中获得更新的副本。

XML 还支持使用点表示法检索 XML 标签数据。使用点表示法，你只需通过调用标签名称和属性，以句点分隔，即可检索所有父级、子级和同级标签。对于前面的示例，你可以调用 $ports.Name 来返回 FTP、SMTP、DNS、HTTP 和 HTTPS。你也可以直接引用 $xml 内容并指定 $xml.ScriptAnswers.Ports.Name 来返回 FTP、SMTP、DNS、HTTP 和 HTTPS。

使用点表示法查看端口名称，可以输入以下内容：

```
$ports.Name
$xml.ScriptAnswers.Ports.Name
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_48_0.png)

前面的示例展示了如何在 PowerShell 中利用点表示法查看 XML 内容中的不同属性。第一个示例利用 $ports 变量通过点表示法引用 Name 属性。这返回了 FTP、SMTP、DNS、HTTP 和 HTTPS。第二个示例展示了使用顶级 XML 内容来引用子级和同级属性。这是通过在 $xml 内容、父级 ScriptAnswers、子级 Ports 和属性 Name 之间使用点表示法完成的。这返回了 FTP、SMTP、DNS、HTTP 和 HTTPS。

## XML 标签函数

随着你开始构建更复杂的脚本，你可能会在答案文件中添加更多标签。你可以使用标签名称作为参数利用 .GetElementsByTagName() 方法来收集答案文件中的额外数据。这使你能够单独调用每个标签并将内容存储到变量中。

当你利用 .GetElementsByTagName() 方法时，它只读取一次数据并将数据存储在变量中。这意味着如果 XML 文件发生变化，数据不会自动在变量中更新。你需要第二次调用该方法以收集 XML 文件中数据的更新副本。

或者，你也可以选择创建一个函数来读取单个 XML 标签。此函数每次调用时都可以刷新内存中的 XML 内容。这使得函数可以在脚本的多个部分中调用，并且始终具有当前的 XML 数据。

考虑以下用于 XML 标签函数的答案文件（Scan_Answers.xml）：

```
```xml
<?xml version="1.0"?>
<ScriptAnswers>
    <!-- This section enables and disables features in the script -->
    <!-- To Enable Logging For Each Step, Set To True. To Disable Logging, Set to False -->
    <verboselog id="False"></verboselog>
    <!-- Scan Disks -->
    <scndisks id="True"></scndisks>
    <!-- Scan Scheduled Tasks -->
    <scnschtks id="True"></scnschtks>
    <!-- Scan Processes -->
    <scnproc id="True"></scnproc>
    <!-- Scan Services -->
    <scnsvcs id="True"></scnsvcs>
    <!-- Scan Software -->
    <scnsoft id="True"></scnsoft>
    <!-- Scan User Profiles -->
    <scnuprof id="True"></scnuprof>
    <!-- Scan Windows Features -->
    <scnwfeat id="True"></scnwfeat>
    <!-- Scan Files -->
    <scnfls id="True"></scnfls>
    <!-- Scan Windows Updates -->
    <scnwupd id="True"></scnwupd>
</ScriptAnswers>
```
```

前面的 XML 文件和数据将在本章的其余部分中使用。要完成本章中的示例，你需要在 c:\Temp\POSHScript\ 位置创建一个名为 Scan_Answers.xml 的新 XML 文件。利用你选择的文本编辑器，将之前的数据插入 Scan_Answers.xml 文件中。

> 你也可以参考本章的代码文件，以便快速创建前面描述的内容。

要创建 XML 标签读取函数，可以执行以下操作：

```
```powershell
# Answer File Location
$xmlfile = "c:\temp\POSHScript\Scan_Answers.xml"
Function read-xmltag { param($xmlanswers, $xmlextract)
    # Validate that the XML file still exists
    $test = test-path $xmlanswers
    if (!$test) {
        Write-Error "$xmlanswers not found on the system. Select any key to exit!"
        # Stop the Script for reading the Error Message
        PAUSE
```
