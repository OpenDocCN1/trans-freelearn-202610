
## 导入 SQL 文件

任何包含 MySQL 语句的文件都可以通过此机制导入。该对话框可在数据库视图或表视图中通过导入子页面，或在查询窗口中使用。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_168_0.png)

> 当前选择的表（此处为 author）与将要导入的 SQL 文件的实际内容之间没有关系。SQL 文件的所有内容都将被导入，并且是这些内容决定了哪些表或数据库受到影响。但是，如果导入的文件不包含选择数据库的 SQL 语句，则导入文件中的所有语句都将在当前选择的数据库上执行。

让我们尝试一个导入练习。首先，我们确保拥有 book 表的当前 SQL 导出文件（如第 7 章所述）。该导出文件必须包含结构和数据。然后我们删除 book 表——是的，真的！我们也可以简单地重命名它。（有关操作步骤，请参见第 10 章。）

现在是时候将文件导入回来了。我们应该在导入子页面上，在那里可以看到文本文件位置对话框。我们只需点击浏览按钮并选择文件即可。

phpMyAdmin 能够检测文件是否应用了压缩方法（如果有的话）。根据 phpMyAdmin 版本以及 Web 服务器 PHP 组件中可用的扩展，程序能够解压缩的格式会有所不同。

然而，要成功导入，phpMyAdmin 必须获知要导入文件的字符集。默认值是 **utf8**。但是，如果我们知道导入文件是用另一种字符集创建的，则应在此处指定。

导入时提供 **SQL 兼容模式** 选择器。此模式应根据数据先前导出的服务器类型进行调整，以匹配即将导入的实际数据。

要开始导入，我们点击 **Go**。导入过程继续，我们收到消息：**导入已成功完成，已执行 2 条查询**。我们可以浏览新创建的表以确认导入操作成功。

该文件可以导入到不同的数据库中进行测试，甚至导入到 MySQL 服务器中。

## 导入 CSV 文件

在本节中，我们将研究如何导入 CSV 文件。有两种可能的方法——**CSV** 和 **CSV using LOAD DATA**。第一种方法由 phpMyAdmin 内部实现，因其简单性而被推荐。使用第二种方法，phpMyAdmin 接收要加载的文件并将其传递给 MySQL。理论上，此方法应该更快。然而，由于 MySQL 本身的原因，它有更多要求（请参阅 *CSV using LOAD DATA* 部分的 *Requirements* 小节）。

## SQL 与 CSV 格式的差异

这两种格式之间存在一些差异。CSV 文件格式仅包含数据，因此我们必须已经有一个现有的表。该表不需要与原始表（数据来源）具有相同的结构；**列名** 对话框使我们能够选择目标表中受影响的列。

由于表必须在导入前存在，CSV 导入对话框仅可在表视图中的 **导入** 子页面中使用，而不在数据库视图中使用。

## 导出测试文件

在尝试导入之前，让我们从 author 表生成一个 author.csv 导出文件。我们使用 CSV 导出选项中的默认值。然后我们可以清空 author 表——我们应避免删除此表，因为我们仍然需要表结构。

## CSV

从 author 表菜单中，我们选择导入然后选择 CSV。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_170_0.png)

我们可以通过多种方式影响导入行为。默认情况下，导入不会修改现有数据（基于主键或唯一键）。然而，Replace table data with file 选项指示 phpMyAdmin 使用 REPLACE 语句而不是 INSERT 语句，以便用导入的数据替换现有行。

使用 Ignore duplicate rows，会生成 INSERT IGNORE 语句。这些语句会导致 MySQL 在插入期间忽略任何重复键问题。导入文件中的重复键不会替换现有数据，并且处理继续进行到 CSV 数据的下一行。

然后我们可以指定终止每个字段的字符、包围数据的字符以及转义包围字符的字符。通常这是 \。例如，对于双引号包围字符，如果数据字段包含双引号，则必须表示为 "some data \" some other data"。

对于 Lines terminated by，phpMyAdmin 的最新版本提供 auto 选项，应首先尝试，因为它会自动检测行结束字符。我们也可以手动指定终止行的字符。通常的选择是 UNIX 系统的 \n，DOS 或 Windows 系统的 \r\n，以及 Mac 系统的 \r（最高到 Mac OS 9）。如果不确定，我们可以使用客户端计算机上的十六进制文件编辑器（不属于 phpMyAdmin 的一部分）来检查确切的代码。

默认情况下，phpMyAdmin 预期 CSV 文件具有与目标表相同数量的字段和相同的字段顺序。但这可以通过在 Column names 中输入以逗号分隔的列名列表来更改，遵守源文件格式。例如，假设我们的源文件仅包含作者 ID 和作者姓名信息：

```
"1","John Smith"
"2","Maria Sunshine"
```

我们必须在 Column names 中输入 id, name 以匹配源文件。

当我们点击 Go 时，导入被执行并且我们得到确认。如果文件的总大小不大，我们也可能会看到生成的实际 INSERT 查询。

```
Import has been successfully finished, 2 queries executed.
INSERT INTO `author` VALUES ('1', 'John Smith', '+01 445 789-1234')
# 1 row(s) affected.

INSERT INTO `author` VALUES ('2', 'Maria Sunshine', '333-3333')
# 1 row(s) affected.
```

## CSV using LOAD DATA

使用此方法，phpMyAdmin 依赖于服务器的 LOAD DATA INFILE 或 LOAD DATA LOCAL INFILE 机制来执行实际导入，而不是在内部处理数据。这些语句是 MySQL 中导入文本的最快方式。它们会导致 MySQL 从位于 MySQL 服务器上的文件（LOAD DATA INFILE）或从其他位置（LOAD DATA LOCAL INFILE）开始读取操作，在此上下文中，始终是 Web 服务器的文件系统。如果 MySQL 服务器位于与 Web 服务器不同的计算机上，我们将无法使用 LOAD DATA INFILE 机制。

## 要求

依赖 MySQL 服务器会带来一些后果。使用 LOAD DATA INFILE 要求登录用户拥有全局 FILE 权限。此外，文件本身必须可由 MySQL 服务器进程读取。

> 第 18 章解释了 phpMyAdmin 的界面，供系统管理员管理权限。

LOAD DATA LOCAL INFILE 中 LOCAL 修饰符的使用必须由 MySQL 服务器和 PHP 使用的 MySQL 客户端库允许。

两种 LOAD 方法都可从 phpMyAdmin LOAD 接口获得，该接口尝试选择最佳的默认选项。

## 使用 LOAD DATA 接口

我们从 author 表菜单中选择导入。选择 CSV using LOAD DATA 会弹出以下对话框：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_172_0.png)

> 可用选项已在 CSV 部分中介绍过。

在熟悉的文本文件位置问题中，我们选择我们的 author.csv 文件。最后，我们可以通过选择 Use LOCAL keyword 选项来选择 LOAD 方法，如前所述。然后我们点击 Go。

如果一切顺利，我们可以看到如下确认屏幕：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_172_1.png)

此屏幕显示使用的确切 **LOAD DATA LOCAL INFILE** 语句。以下是发生的情况：

- 我们选择了 **author.csv**。
- 该文件的内容通过 HTTP 传输并被 Web 服务器接收。
- Web 服务器内部的 PHP 组件将此文件保存在工作目录中（此处为 **/opt/php-upload-tmp/**）并为其分配临时名称。
- phpMyAdmin 获知此工作文件的位置后，构建了 **LOAD DATA LOCAL INFILE** 命令并将其发送到 MySQL。请注意，只执行了一个查询，该查询加载了多行。
- MySQL 服务器读取并将文件内容加载到我们的目标表中。然后它返回受影响的行数（2），phpMyAdmin 在结果页面中显示了该数字。

## Web 服务器上传目录

为了避免 Web 服务器 PHP 配置完全禁用上传或上传限制过小的情况，phpMyAdmin 可以从 Web 服务器文件系统上的特殊目录读取上传文件。此机制适用于 SQL 和 CSV 导入。

我们首先在 $cfg['UploadDir'] 参数中指定我们选择的目录名，例如 './upload'。我们还可以使用第 7 章所述的 %u 字符串来表示用户名。

现在，让我们返回到导入子页面。我们收到错误消息：

**您设置的上传工作目录无法访问。**

此错误消息是预期的，因为目录不存在。它应该已在当前 phpMyAdmin 安装目录内创建。该消息也可能表示目录存在，但 Web 服务器无法读取它。（在 PHP 安全模式下，目录的所有者和 phpMyAdmin 安装脚本的所有者必须相同。）

使用 SFTP 或 FTP 客户端，我们创建必要的目录，现在可以在那里上传文件（例如 book.sql），从而绕过任何 PHP 超时或上传最大限制。请注意，文件本身必须具有允许 Web 服务器读取它的权限。在大多数情况下，最简单的方法是允许所有人读取文件。
