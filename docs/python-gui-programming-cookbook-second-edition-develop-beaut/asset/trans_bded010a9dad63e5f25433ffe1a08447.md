

## 通过 GUI 在我们的 MySQL 数据库中存储数据

![](img/986b0dbf3f539f4f3baa82bd3003f04e_232_0.png)

在安装过程中，您将为 Root 用户选择一个密码，并且还可以添加更多用户。我建议您将自己添加为 **DB Admin** 并同样选择一个密码：

## 账号和角色

## Root 账号密码

输入 root 账号的密码。请务必将此密码保存在安全的地方。

MySQL Root Password:

Repeat Password:

Password Strength: Weak

## MySQL 用户账号

为您的用户和应用程序创建 MySQL 用户账号。为用户分配一个由一组权限组成的角色。

| MySQL Username | Host | User Role |
| Burkhard | % | DB Admin |

Add User

Edit User

Delete

![](img/986b0dbf3f539f4f3baa82bd3003f04e_233_0.png)

在本章中，我们使用的是最新的 MySQL Community Server Release 5.7.17。

验证我们是否安装了正确的驱动程序并且它能让 Python 与 MySQL 通信的一种方法，是查看 Python `site-packages` 目录。如果您的 `site-packages` 目录中出现了新的 `MySQLdb` 文件夹以及一些其他 `_mysql` 模块，则安装成功：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_235_0.png)

## 通过 GUI 在我们的 MySQL 数据库中存储数据

首先，让我们通过使用 *MySQL Command Line Client* 来验证我们的 MySQL 服务器安装是否正常工作。在 `mysql>` 提示符下，输入 `SHOW DATABASES;` 然后按 *Enter*：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_236_0.png)

接下来，我们将验证是否可以使用 Python 3.6 实现相同的结果：

> 将占位符带括号的名称 `<adminUser>` 和 `<adminPwd>` 替换为您在 MySQL 安装中实际使用的凭据。

```
```python
import MySQLdb as mysql
conn = mysql.connect(user=<adminUser>, password=<adminPwd>,
host='127.0.0.1')
print(conn)
conn.close()
```
```

如果运行上述代码导致以下输出打印到控制台，则我们就没问题了：

```
MySQL_connect.py

<terminated> C:\Eclipse_NEON_workspace\2nd Edition Python GUI Programming Cookbook\Ch07_Code\MySQL_connect.py
<_mysql.connection open to '127.0.0.1' at 605769b8>
```

如果您无法通过 *Command Line Client* 或 Python *mysqlclient* 连接到 MySQL 服务器，则安装过程中可能出了问题。如果是这种情况，请尝试卸载、重新启动您的 PC，然后再次运行安装。

## How it works...

为了将我们的 GUI 连接到 MySQL 服务器，如果我们想创建自己的数据库，就需要能够使用管理员权限连接到服务器。如果数据库已存在，则我们只需要连接、插入、更新和删除数据的授权权限即可。我们将在下一个配方中在 MySQL 服务器上创建一个新数据库。

## 配置 MySQL 数据库连接

在之前的配方中，我们通过将用于身份验证的凭据硬编码到 `connection` 方法中使用了连接 MySQL 服务器的最简便方式。虽然这对于早期开发来说是一种快速的方法，但我们绝对不想让任何人看到我们的 MySQL 服务器凭据。相反，我们 *grant* 权限让特定用户访问数据库、表、视图和相关的数据库命令。

一种更安全的方式来让 MySQL 服务器进行身份验证是将凭据存储在配置文件中，这就是我们在本配方中要做的事情。我们将使用配置文件连接到 MySQL 服务器，然后在 MySQL 服务器上创建自己的数据库。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_237_0.png)

我们将在以下所有配方中使用此数据库。

## Getting ready

运行本配方中显示的代码需要访问具有管理员权限的正在运行的 MySQL 服务器。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_238_0.png)

> 上一个配方展示了如何安装免费的 MySQL Server Community Edition。管理员权限将使您能够实现本配方。

## How to do it…

首先，我们在 GUI_MySQL_class.py 代码的同一模块中创建一个字典：

```
```python
# create dictionary to hold connection info
dbConfig = {
    'user': <adminName>,       # use your admin name
    'password': <adminPwd>,    # not the real password
    'host': '127.0.0.1',       # IP address of localhost
}
```
```

接下来，在连接方法中，我们解包字典值。看一下以下代码片段：

```
```python
mysql.connect('user': <adminName>, 'password': <adminPwd>, 'host':
'127.0.0.1')
```
```

与其使用上述片段，我们使用 (**dbConfig)，它实现了与上述相同的功能，但要短得多：

```
```python
import MySQLdb as mysql
# unpack dictionary credentials
conn = mysql.connect(**dbConfig)
print(conn)
```
```

这会产生对 MySQL 服务器的相同成功连接，但区别在于连接方法不再暴露任何关键任务信息：

数据库服务器对您的任务至关重要。一旦您丢失了宝贵的数据...并且找不到任何最近的备份，您就会意识到这一点！

MySQL_connect_with_dict.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_239_0.png)

现在，将相同的用户名、密码、数据库等放入同一 Python 模块中的字典并不能消除任何浏览代码的人看到凭据的风险。

为了提高数据库安全性，我们首先将字典移至其自己的 Python 模块。让我们将新的 Python 模块命名为 GuiDBConfig.py。

然后我们导入此模块并像之前一样解包凭据：

```
import GuiDBConfig as guiConf
# unpack dictionary credentials
conn = mysql.connect(**guiConf.dbConfig)
print(conn)
```

一旦我们将此模块放置在与代码其余部分分离的安全位置，我们就为我们的 MySQL 数据实现了更好的安全级别。

现在我们知道如何连接到 MySQL 并且拥有管理员权限，我们可以通过发出以下命令来创建自己的数据库：

```
GUIDB = 'GuiDB'

# unpack dictionary credentials
conn = mysql.connect(**guiConf.dbConfig)

cursor = conn.cursor()

try:
    cursor.execute("CREATE DATABASE {}
                    DEFAULT CHARACTER SET 'utf8'".format(GUIDB))
```

```
except mysql.Error as err:
    print("Failed to create DB: {}".format(err))

conn.close()
```

为了执行对 MySQL 的命令，我们从连接对象创建一个游标对象。

游标通常是数据库表中特定行的位置，我们可以在表中上下移动它，但在这里，我们用它来创建数据库本身。我们将 Python 代码包装在 try...except 块中，并使用 MySQL 的内置错误代码来告诉我们是否出现任何问题。

我们可以通过执行创建数据库的代码两次来验证此块是否有效。第一次，它将在 MySQL 中创建一个新数据库，第二次，它将打印出一条错误消息，指出该数据库已存在：

MySQL_create_DB.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_240_0.png)

我们可以通过使用完全相同的游标对象语法执行以下 MySQL 命令来验证哪些数据库存在。与发出 CREATE DATABASE 命令不同，我们创建一个游标并使用它执行 SHOW DATABASES 命令，其结果我们获取并打印到控制台输出：

```
import MySQLdb as mysql
import GuiDBConfig as guiConf

# unpack dictionary credentials
conn = mysql.connect(**guiConf.dbConfig)

cursor = conn.cursor()

cursor.execute("SHOW DATABASES")
print(cursor.fetchall())

conn.close()
```

我们通过在游标对象上调用 `fetchall` 方法来检索结果。

运行此代码会显示我们 MySQL 服务器实例中当前存在的数据库。正如输出所示，MySQL 附带了多个内置数据库，例如 `information_schema` 等。我们已成功创建了自己的 `guidb` 数据库，这在输出中显示。所有其他数据库都是随 MySQL 一起提供的：

```
MySQL_show_DBS.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_241_0.png)

请注意，即使我们在创建时以混合大小写字母 GuiDB 指定了数据库，`SHOW DATABASES` 命令也会以小写显示 MySQL 中所有现有数据库，并将我们的数据库显示为 `guidb`。

## How it works…

为了将我们的 Python GUI 连接到 MySQL 数据库，我们首先必须知道如何连接到 MySQL 服务器。这需要建立连接，而此连接只有在我们能够提供所需凭据时才会被 MySQL 接受。

虽然将字符串放入一行 Python 代码很容易，但当我们处理数据库时，我们必须非常谨慎，因为今天的个人沙盒开发环境明天就可能在万维网上变得可访问。

您不想损害数据库安全性，本配方的第一部分展示了更安全的方法，即将 MySQL 服务器的连接凭据放入单独的文件中，并且通过将此文件放置在无法从外部世界访问的位置，我们的数据库系统将变得更安全。

在真实的生产环境中，MySQL 服务器安装、连接凭据和此 `dbConfig` 文件将由 IT 系统管理员处理，他们将使您能够导入 `dbConfig` 文件以连接到 MySQL 服务器，而无需您知道实际凭据是什么。解包 `dbConfig` 不会像我们的代码中那样暴露凭据。

