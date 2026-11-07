
## 奖项

phpMyAdmin 已获得一些奖项，可在项目主页的“奖项”部分看到。首先，它于 2002 年 12 月被 SourceForge 的管理员授予“每月项目”称号。在我们准备放在 SourceForge POTM 页面上的访谈式文档中，我写道我对我们产品的下载率印象深刻，当时为每分钟三次。（此后，我们在高峰日达到了每分钟十次。）

phpMyAdmin 在 2003 年“最佳 PHP 应用/工具”类别中获得了德国 PHP Magazine 及其国际版读者的 75% 投票。该奖项于 2003 年 11 月在法兰克福举行的国际 PHP 大会上正式颁发给团队的两名成员。德国 PHP Magazine 在 2005 年和 2006 年再次举办读者选择，phpMyAdmin 在这两年的比赛中均获胜。

SourceForge.net 于 2006 年首次举办“社区选择奖”，phpMyAdmin 在两个类别中获胜——数据库和系统管理。我于四月代表团队出席了在波士顿 LinuxWorld 的颁奖典礼。该项目还在第五届年度 OS/2 World Awards 上获得了“年度最佳 PHP 应用”奖。2006 年底，我们的项目在第三届“Trophées du Libre”大赛中获得银奖杯；我前往法国领取奖杯。

2007 年，在 SourceForge.net“社区选择奖”中，phpMyAdmin 被提名为“最佳开发者工具或实用程序”类别，并赢得了“最佳系统管理员工具或实用程序”奖。

2008 年初，MySQL AB 被 Sun Microsystems 以 10 亿美元收购。我认为这给了 SourceForge.net 提出新奖项类别的灵感：“最有可能成为下一个 10 亿美元收购对象”。然而，正如预期的那样，phpMyAdmin 也获得了该奖项，同时还获得了“最佳系统管理员工具或实用程序”奖。phpMyAdmin 也是“最佳开发者工具或实用程序”的决赛入围者。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_325_0.png)

phpMyAdmin 还获得了 InfoWorld 2008 年“最佳开源平台和中间件（MySQL 管理）”奖。

## 摘要

本附录涵盖了 phpMyAdmin 的历史——从 1998 年的起源，到 2001 年的项目重新启动，以及其后的演变。它详细介绍了每个主要版本，解释了 phpMyAdmin 在 GoPHP5 计划中的作用，并列出了该软件产品获得的奖项列表。下一个附录通过检查一些错误信息并提出解决方案，解释了如何排查 phpMyAdmin 的故障。

## B

## 故障排除和支持

本附录提出了解决一些常见问题的指南，并提供了避免这些问题的提示。它还解释了如何与开发团队互动以获得支持、报告错误和贡献。

## 系统要求

Documentation.html 文件（随下载包一起提供）开头的一个部分讨论了我们使用的特定 phpMyAdmin 版本的系统要求。必须满足这些要求，并且环境必须正确配置以避免出现问题。

一些问题，例如 phpMyAdmin 错误，实际上是由服务器环境引起的。有时，Web 服务器未正确配置为解释 .php 文件，或者 Web 服务器内部的 PHP 组件未使用 mysql 或 mysqli 扩展运行。MySQL 账户可能配置不当。这可能发生在家庭服务器以及托管服务器上。

当我们怀疑有问题时，可以尝试一个简单的 PHP 脚本 test.php，其中包含以下内容以检查 PHP 组件是否正确响应。

```
<?php
echo 'hello';
?>
```

我们应该看到 hello 消息。如果这有效，我们可以尝试另一个脚本：

```
<?php
phpinfo();
?>
```

该脚本显示有关 PHP 组件的信息，包括可用的扩展。我们至少应该看到关于 MySQL 的部分（证明 mysql 扩展可用），其中提供有关 MySQL 客户端 API 版本的信息。

我们还可以尝试其他连接到 MySQL 的 PHP 脚本，以查看问题是否比 phpMyAdmin 不能工作更普遍。一般来说，我们应该运行每个组件的最新稳定版本。

## 基础配置

我们应始终仔细检查安装方式，包括正确的权限和所有权。在修改 config.inc.php 时可能会出现拼写错误。

## 解决常见错误

为帮助解决问题，我们应首先确定错误消息的来源。以下是可能生成错误消息的各种组件：

- MySQL 服务器：这些消息由 phpMyAdmin 转发，后者显示 MySQL said 后跟消息
- Web 服务器的 PHP 组件：例如，Parser error
- Web 服务器：错误可以从浏览器中看到，或在 Web 服务器的日志文件中看到。
- Web 浏览器：例如，JavaScript 错误

错误消息和其他问题部分主要基于 phpMyAdmin 帮助论坛上的各种消息以及 Documentation.html 的 FAQ 部分。

## 错误消息

本节指的是 phpMyAdmin 显示的明确错误消息。

## 无法加载 MySQL 扩展

要连接到 MySQL 服务器，PHP 需要 mysql 扩展（对于 MySQL 4.1+ 推荐使用 mysqli 扩展），它是一组 MySQL 函数。该扩展可能作为 PHP 服务器的一部分编译。也可能需要动态加载，在这种情况下，phpMyAdmin 可能尝试加载它但失败。此错误意味着没有其他 PHP 脚本可以连接到 MySQL 服务器。

所需扩展包含在文件中，在 Linux 或 UNIX 上可以命名为 mysql.so 或 mysqli.so，在 Windows 上为 mysql.dll（可能是 mysqli.dll）。如果我们的 PHP 服务器来自软件包，我们可以找到并安装另一个软件包——可能称为 php-mysql 或 php-mysqli。（名称取决于发行版。）否则，我们可以根据 PHP 文档说明自行编译 PHP 服务器并使用适当的扩展。

## #2003 - 无法连接到 MySQL 服务器

此消息表明 MySQL 服务器未运行，或无法从 Web 服务器访问。它也可能是套接字（Linux/UNIX）或命名管道（Windows）配置问题。

## 套接字问题（Linux/UNIX）

php.ini 中配置的套接字（下面给出示例）与正在运行的 MySQL 服务器的套接字不对应。

```
mysql.default_socket = /tmp/mysql.sock
```

因此，PHP 无法访问 MySQL。我们可以将其更改为：

```
mysql.default_socket = /var/lib/mysql/mysql.sock
```

然而，为了确保，我们必须找到该套接字的确切位置。

## 命名管道问题（Windows）

这是与上面指示类似的问题，但发生在 Windows 上。可以通过使用正确的命名管道调整 mysql.default_socket 来解决，该命名管道用于本地连接到 MySQL 服务器。例如：

```
mysql.default_socket = MySQL
```

## MySQL said: Access denied

当我们了解相关的登录参数时，可以解决此错误。

## 使用 http 身份验证时

我们不能同时使用基于 .htaccess 文件的 Web 服务器安全机制和 config.inc.php 中的 http 身份验证。作为变通方法，请改用 cookie 作为身份验证类型而不是 http。

## 使用 http、cookie 或 config 身份验证时

config.inc.php 中的 host 参数必须与用户访问权限中定义的 host 匹配。有时，系统管理员可能会创建一个授权用户 bill 和主机 localhost 的账户。如果我们在 config.inc.php 中尝试使用 127.0.0.1 主机，MySQL 将拒绝它，即使它指向同一台机器。如果我们尝试使用机器的真实名称（mysql.domain.com）而定义为 localhost，也会出现相同问题。

## Access denied ... "using password: NO"

如果消息以 using password: NO 结尾，则意味着我们未传输密码，MySQL 正在拒绝此登录尝试。config.inc.php 中可能未设置密码值。

## Access denied ... "using password: YES"

已传输密码，但主机/用户名/密码组合已被 MySQL 拒绝。

## Warning: cannot add header information

此问题是由 config.inc.php 中存在某些字符（例如空行或空格）引起的——要么在开头的 <?php 标签之前，要么在结尾的 ?> 标签之后。我们应该使用支持 .php 文件的编辑器将其删除（如第 1 章所述）。

## MySQL said: Error 127, Table Must Be Repaired

在左侧面板中，我们单击数据库名称。在右侧面板中，我们选择出现错误的表的名称（使用相关复选框）。然后我们从下方的下拉列表中选择 Repair。更多详细信息请参见第 10 章。

## BLOB 列在键规范中使用但未指定键长度

MySQL 要求对 BLOB 列设置的索引大小受限。创建列时可用的简单索引创建技术不允许指定大小。因此，我们需要不带索引创建列。然后我们返回 Structure 页面，并使用 Create an index 对话框选择 BLOB 列并为索引设置大小。
