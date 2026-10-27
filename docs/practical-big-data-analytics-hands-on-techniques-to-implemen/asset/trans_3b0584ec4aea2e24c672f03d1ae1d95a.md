

## 使用 MongoDB 分析诺贝尔奖得主数据

在第一个练习中，我们将使用 MongoDB（领先的面向文档的数据库之一）来分析 1902 年至今的诺贝尔奖得主。MongoDB 提供了一种简单直观的界面来处理 JSON 文件。如前所述，JSON 是一种灵活的格式，允许使用结构化方法来表示数据。

### JSON 格式

考虑以下表格：

| 名字 | 年龄 | 信息 |
|-----------|-----|-------------|
| John      | 15  | 科目：历史，等级 B |
| Jack      | 18  | 科目：物理，等级 A |
| Jill      | 17  | 科目：物理，等级 A+ |

“信息”字段包含一列在“科目”和“等级”下分类的多个值。包含多个数据的此类列也称为嵌套数据列。

可移植性一直是数据从一个系统传输到另一个系统的重要方面。通常，ODBC 连接器用于在数据库系统之间传输数据。另一种常见的格式是 CSV 文件，其中数据以逗号分隔值表示。CSV 文件适用于不包含嵌套值等更复杂数据结构的结构化数据。在这种情况下，JSON 提供了一种使用键值对语法捕获和保存信息的最优且结构化方式。

在 JSON 表示中，该表可以定义如下：

```json
[
  {
    "Firstname":"John",
    "Age":15,
    "Information":{
      "Subject":"History",
      "Grade":"B"
    }
  },
  {
    "Firstname":"Jack",
    "Age":18,
    "Information":{
      "Subject":"Physics",
      "Grade":"A"
    }
  },
  {
    "Firstname":"Jill",
    "Age":17,
    "Information":{
      "Subject":"Physics",
      "Grade":"A+"
    }
  }
]
```

请注意，`Information` 键包含两个键：`Subject` 和 `Grade`，每个键都有对应的值。

如今，大多数产品开发人员和供应商都支持接收 JSON 格式的数据。此外，由于复杂关系可以以文本格式简单表达并进行交换，JSON 在全球开发者社区中变得非常流行。

MongoDB 以 JSON 格式捕获数据。它在内部以 BSON（JSON 数据的优化二进制表示）存储这些数据。

### 安装和使用 MongoDB

MongoDB 支持所有主要平台，如 Windows、Linux 和 OS X 平台。

![](img/8ededb1e0b69e85687f6e488d1044b79_148_0.png)

MongoDB 的安装详细信息可在其官方网站 `https://docs.mongodb.com/manual/installation/` 上找到。请注意，我们将使用 MongoDB Community Edition。

对于我们的练习，我们将复用 Cloudera Hadoop 分发版虚拟机中的 Linux CentOS 环境。但该练习并不依赖于你安装 MongoDB 的平台。安装完成后，你可以在任何其他受支持的平台上执行本章中的命令。如果你可以访问单独的 Linux 机器，你也可以使用它。

我们将了解 MongoDB 的一些常见语义，并下载两个数据集来计算按大洲分组的最高诺贝尔奖数量。诺贝奖得主的完整数据转储可从 nobelprize.org 获取。数据包含获奖者的所有主要属性。我们希望将这些数据与相应国家的人口统计信息相结合，以提取更多有趣的分析信息：

- 1. **下载 MongoDB**：MongoDB 可从 https://www.mongodb.com/download-center#community 下载。

为了确定适用版本，我们检查了 CDH 虚拟机上安装的 Linux 版本：

```text
(cloudera@quickstart ~)$ lsb_release -a
LSB Version:     :base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch
Distributor ID:  CentOS
Description:     CentOS release 6.7 (Final)
Release:  6.7
Codename: Final
```

- 2. 根据该信息，我们必须使用 CentOS 版本的 MongoDB，并按照 https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/ 上的说明安装软件，如下所示：

第一步是添加仓库，如下所示。在命令行中输入 `sudo nano /etc/yum.repos.d/mongodb-org-3.4.repo` 并输入所示的文本。

```text
(root@quickstart cloudera)# sudo nano /etc/yum.repos.d/mongodb-org-3.4.repo
```

输入如下信息并按 CTRL-X。当提示保存缓冲区时，输入 yes。

```text
(mongodb-org-3.4)
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

以下截图显示了文件的内容：

![](img/8ededb1e0b69e85687f6e488d1044b79_150_0.png)

如下截图所示，输入 Y 表示是：

![](img/8ededb1e0b69e85687f6e488d1044b79_150_1.png)

按如下图像所示保存文件。现在这将允许我们安装 MongoDB：

![](img/8ededb1e0b69e85687f6e488d1044b79_150_2.png)

```shell
# 回到终端，输入以下内容

(cloudera@quickstart ~)$ sudo yum install -y mongodb-org
```

```text
(...)

正在安装:
  mongodb-org              x86_64      3.4.6-1.el6
  mongodb-org-3.4          5.8 k
正在为依赖安装:
  mongodb-org-mongos       x86_64      3.4.6-1.el6
  mongodb-org-3.4          12 M
  mongodb-org-server       x86_64      3.4.6-1.el6
  mongodb-org-3.4          20 M
  mongodb-org-shell        x86_64      3.4.6-1.el6
  mongodb-org-3.4          11 M
  mongodb-org-tools        x86_64      3.4.6-1.el6
  mongodb-org-3.4          49 M

事务概要
================================================================================
安装  5 个软件包

总下载大小：91 M
安装后大小：258 M
正在下载软件包：
(1/5): mongodb-org-3.4.6-1.el6.x86_64.rpm
| 5.8 kB    00:00
(...)

已安装:
  mongodb-org.x86_64 0:3.4.6-1.el6

已安装依赖:
  mongodb-org-mongos.x86_64 0:3.4.6-1.el6        mongodb-org-server.x86_64 0:3.4.6-1.el6
  mongodb-org-shell.x86_64 0:3.4.6-1.el6          mongodb-org-tools.x86_64 0:3.4.6-1.el6

完成！
```

在未先启动守护进程的情况下尝试启动 mongo 会产生错误消息。你需要先启动 mongo 守护进程才能使用它。

```text
(cloudera@quickstart ~)$ mongo
MongoDB shell version v3.4.6
connecting to: mongodb://127.0.0.1:27017
2017-07-30T10:50:58.708-0700 W NETWORK  (thread1) Failed to connect to 127.0.0.1:27017, in(checking socket for error after poll),
reason: Connection refused
2017-07-30T10:50:58.708-0700 E QUERY      (thread1) Error: couldn't connect to server 127.0.0.1:27017, connection attempt failed : connect@src/mongo/shell/mongo.js:237:13
@(connect):1:6
exception: connect failed
```

第一步是创建 MongoDB 的 dbpath——这是 MongoDB 将存储所有数据的地方。创建一个名为 mongodata 的文件夹，这将作为 mongo 的 dbpath。

```text
(cloudera@quickstart ~)$ mkdir mongodata
```

启动 mongod。

```text
(cloudera@quickstart ~)$ mongod --dbpath mongodata
2017-07-30T10:52:17.200-0700 I CONTROL   (initandlisten) MongoDB starting : pid=16093 port=27017 dbpath=mongodata 64-bit host=quickstart.cloudera
(...)
2017-07-30T10:52:17.321-0700 I INDEX     (initandlisten) build index done.  scanned 0 total records. 0 secs
2017-07-30T10:52:17.321-0700 I COMMAND   (initandlisten) setting featureCompatibilityVersion to 3.4
2017-07-30T10:52:17.321-0700 I NETWORK   (thread1) waiting for connections on port 27017
```

打开一个新终端并按以下截图所示下载 JSON 数据文件：

![](img/8ededb1e0b69e85687f6e488d1044b79_152_0.png)

在 Mac OS X 的终端应用程序中选择“打开终端”

```text
# 下载文件
# laureates.json 和 country.json

# 更改目录到之前创建的 mongodata 文件夹
(cloudera@quickstart ~)$ cd mongodata

(cloudera@quickstart mongodata)$ curl -o laureates.json "http://api.nobelprize.org/v1/laureate.json"
% Total    % Received % Xferd  Average Speed   Time     Time     Time
Current                                 Dload  Upload   Total   Spent    Left
Speed
100 428k    0 428k    0     0  292k      0 --:--:--  0:00:01 --:--:--
354k
```

清理 laureates.json 文件。删除文件第一行中第一个 `{` 之前的内容。删除文件中的最后一个 `}` 字符。将清理后的数据集存储在一个名为 laureates.json 的文件中。

请注意，该文件需要稍微修改。代码如下图所示：

![](img/8ededb1e0b69e85687f6e488d1044b79_153_0.png)

```text
(cloudera@quickstart mongodata)$ cat laureates.json | sed 's/^{"laureates"://g' | sed 's/}$//g' > mongofile.json
```

将 laureates.json 文件导入 MongoDB。mongoimport 是一个用于将数据导入 MongoDB 的实用程序。下面的命令将把 mongofile.json 文件中的数据导入到名为 nobel 的数据库中的一个名为 laureates 的集合（即表）中。

```text
(cloudera@quickstart mongodata)$ mongoimport --jsonArray --db nobel --collection laureates --file mongofile.json
2017-07-30T11:06:35.228-0700    connected to: localhost
2017-07-30T11:06:35.295-0700    imported 910 documents
```

