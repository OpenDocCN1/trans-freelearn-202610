

为了便于后续引用，我建议你选择一个工作目录，例如 `~/zabbix`（~ 为你的主目录）。将压缩包下载到此目录中。

## 编译

下载完成后，打开一个终端并解压它：

```
$ cd ~/zabbix; tar -zxvf zabbix-3.0.0.tar.gz
```

我建议你立即安装先决条件并编译 Zabbix 以支持外部功能，这样在后续过程中就无需重新编译了。

为本书的目的，我们将编译 Zabbix 以支持 server、agent、MySQL、curl、SNMP、SSH、ODBC、XML（VMware）和 IPMI。

继续操作，在终端中输入以下内容：

```
$ cd zabbix-3.0.0
$ ./configure --enable-server --with-mysql --with-net-snmp --with-libcurl --with-openipmi --enable-agent --with-libxml2 --with-unixodbc --with-ssh2 --with-openssl
```

最后，将打印出已编译组件的摘要。验证以下功能是否已启用：

```
Enable server: yes
Server details:
With database: MySQL
WEB Monitoring: cURL
SNMP: yes
IPMI: yes
SSH: yes
TLS: OpenSSL
ODBC: yes
Enable agent: yes
```

如果配置成功完成，一切正常。如果失败，请检查控制台中打印的错误信息并验证所有先决条件是否已安装。名为 config.log 的文件可能提供有关错误的更多详细信息。如果无法找出问题所在，请查看附录 A，故障排除，其中列出了一些常见的编译问题。

## 初始配置

编译后，我们需要为 server 和 agent 配置一些基本参数。Zabbix 提供了默认配置文件。这些文件的位置取决于你选择的安装方法：

-   源代码安装：`/usr/local/etc`
-   RHEL/CentOS/OpenSUSE 包安装：`/etc`

在其他发行版中，文件可能位于不同的目录。本书中，我们将使用相对名称引用二进制文件和配置文件，除非绝对路径被推荐或要求。

要配置 Zabbix agent，我们无需做任何操作。现在默认配置就足够了。这很容易，对吧？

对于 server，我们需要进行一些更改。用你喜欢的编辑器打开 `zabbix_server.conf` 文件（你需要以 root 用户身份编辑它），并在文件中找到以下条目：

-   DBName
-   DBUser
-   DBPassword

`DBName` 默认应为 `zabbix`；我们可以保持不变。`DBUser` 设置为 `root`，我们不喜欢这样，所以将其更改为 `zabbix`。对于 `DBPassword`，选择任意密码。你无需记住它，所以发挥创意。

> 在类 UNIX 解决方案中，行首的井号 `#` 通常表示该行被注释掉了。请确保不要以井号开头你希望生效的行。

## 创建并填充数据库

为了让 Zabbix server 存储数据，我们必须创建一个数据库。启动一个 MySQL 客户端：

```
$ mysql -u root -p
```

输入 MySQL root 用户的密码（你在安装 MySQL 时已设置此密码，或该密码可能是发行版的默认值）。如果你不知道密码，可以尝试省略 `-p`。此开关将指示客户端尝试在无密码（或空密码）情况下连接。

![](img/f5313a804532fabacc8d07cc8607eeba_42_0.png)

如果你使用的是来自软件包的 MySQL Community Edition 且版本为 5.7.6 或更高，它会生成一个随机密码并存储在日志文件中。有关更多详细信息，请查阅 MySQL 文档 http://dev.mysql.com/doc/refman/5.7/en/linux-installation-rpm.html。

现在，让我们创建数据库。添加 Zabbix 将以此身份连接到数据库的用户，并授予该用户必要的权限：

```text
mysql> create database zabbix character set utf8 collate utf8_bin;
Query OK, 1 row affected (0.01 sec)
mysql> grant all privileges on zabbix.* to 'zabbix'@'localhost'
identified by 'mycreativenpassword';
Query OK, 0 rows affected (0.12 sec)
```

使用你在 `zabbix_server.conf` 文件中设置的密码，而不是 `mycreativenpassword`。

通过输入以下命令退出 MySQL 客户端：

```text
mysql> quit
```

让我们用 Zabbix 架构和初始数据填充新创建的数据库。以下命令指的是 Zabbix 源代码中文件的位置。安装自软件包时，这些文件可能位于 `/usr/share/doc/zabbix-server-mysql-3.0.0/create/` 或 `/usr/share/zabbix-server-mysql` 等目录中：

```text
$ mysql -u zabbix -p zabbix < database/mysql/schema.sql
$ mysql -u zabbix -p zabbix < database/mysql/images.sql
$ mysql -u zabbix -p zabbix < database/mysql/data.sql
```

三个导入过程都应在没有任何消息的情况下完成。如果出现任何错误，请查看消息，修复问题并重试失败的操作。如果导入在过程中途被中断，你可能需要清除数据库——最简单的方法是删除数据库，输入以下命令：

```text
mysql> drop database zabbix;
Query OK, 0 rows affected (0.00 sec)
```

![](img/f5313a804532fabacc8d07cc8607eeba_42_1.png)

> 小心不要删除包含重要信息的数据！删除 Zabbix 数据库后，按我们之前的方式重新创建它。

到目前为止，我们应该已经安装了 Zabbix server 和 agent 并准备好启动。

## 启动

你绝不应以 root 身份启动 Zabbix server 或 agent，这对大多数守护进程来说是常识。如果你从发行版软件包安装 Zabbix，系统用户应该已经创建——如果没有，让我们创建一个新用户来运行这些进程。你可以使用发行版提供的工具或使用最广泛的命令——`useradd`，我们需要以 root 身份执行：

```
# useradd -m -s /bin/bash zabbix
```

![](img/f5313a804532fabacc8d07cc8607eeba_43_0.png)

> 对于生产系统，请考虑为 Zabbix server 和 agent 使用不同的用户账户。否则，具有配置权限的用户将能够通过指示 agent 读取 server 配置文件来发现 Zabbix 数据库凭据。一些发行版软件包，例如 EPEL 和 OpenSUSE，已经默认使用名为 `zabbixsrv` 或 `zabbix` 的单独用户账户。

这将创建一个名为 `zabbix` 的用户，其主目录位于默认位置，通常为 `/home/zabbix`，shell 为 `/bin/bash`。

![](img/f5313a804532fabacc8d07cc8607eeba_43_1.png)

> 虽然在测试系统上使用 bash 会使调试问题更容易，但在生产系统上请考虑使用 `/bin/nologin` 或 `/bin/false`。

如果你是从源代码安装，让我们尝试直接的方法——运行二进制文件。二进制文件的位置取决于所选择的安装方法。不带任何额外参数从源代码安装会将 agent 和 server 二进制文件放置在 `/usr/local/sbin`；发行版软件包很可能将它们放置在 `/usr/sbin`。假设它们在你的路径中，你可以通过运行以下命令确定二进制文件的位置：

```
# which zabbix_server
```

![](img/f5313a804532fabacc8d07cc8607eeba_43_2.png)

> 请注意可能使用连字符或减号代替下划线。

这将显示类似以下内容：

```
/usr/sbin/zabbix_server
```

或者，`whereis` 命令也可以列出配置文件和其他相关文件：

```
# whereis zabbix_server
```

这很可能会列出二进制文件、配置文件和 man 手册页：

```
zabbix_server: /usr/sbin/zabbix_server /usr/local/etc/zabbix_server.conf /usr/share/man/man3/zabbix_server
```

一旦你知道二进制文件的确切位置，以 root 用户身份执行以下操作：

```
# <path>/zabbix_agentd
```

![](img/f5313a804532fabacc8d07cc8607eeba_44_0.png)

> 我们使用 zabbix agentd，它作为守护进程运行。旧版本还具有 `zabbix_agent` 可执行文件，它提供了在互联网服务守护进程 inetd 中运行的选项；它不支持主动项，并且在大多数情况下性能不如 agent 守护进程。

这将启动 Zabbix agent 守护进程，它应静默启动并守护化。如果命令产生错误，请在继续之前解决它们。如果成功，请继续启动 Zabbix server：

```
# <path>/zabbix_server
```

![](img/f5313a804532fabacc8d07cc8607eeba_44_1.png)

> 检查 Zabbix server 日志文件，可在 `zabbix_server.conf` 中配置。如果有数据库相关错误，请修复它们并重新启动 Zabbix server。

如果你是从软件包安装，执行以下操作：

```
# service zabbix-agentd start
Starting zabbix agent daemon
done
# service zabbix-server start
Starting zabbix server daemon
done
```

这样应该可以启动 agent 和 server。在 OpenSUSE 上，你也可以使用不同的、更短的语法：

```
# rczabbix-agentd start
# rczabbix-server start
```

