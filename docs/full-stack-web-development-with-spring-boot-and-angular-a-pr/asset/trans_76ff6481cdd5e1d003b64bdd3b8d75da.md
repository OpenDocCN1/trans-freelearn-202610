

- **支持 SQL 的不同特性**：它提供 SQL 的各种特性，例如以下内容：
    - 多种索引，如 B-tree 和表达式
    - SQL 子查询
    - 复杂的 SQL 查询
    - **多版本并发控制（MVCC）**
    - 表分区
- **数据完整性兼容性**：它还提供数据完整性，包括以下内容：
    - 主键
    - 外键
    - 显式锁
    - 建议锁
    - `UNIQUE`
    - `NOT NULL`
- **安全数据库**：它遵循标准安全协议，包括以下内容：
    - 诸如 **轻量级目录访问协议（LDAP）**、SCRAM-SHA-256 和 **安全支持提供程序接口（SSPI）** 之类的身份验证
    - 支持列级和行级安全
- **高度可扩展**：它提供多种功能，使其可修改，例如以下内容：
    - JSON/SQL 路径表达式
    - 存储过程和函数
    - 与外部数据包装器的兼容性

现在我们已经概述了 PostgreSQL 的特性和用例，让我们继续在终端上安装它。

## 安装 PostgreSQL

我们有两种方法在开发终端上设置 PostgreSQL。这两种方法如下：

- **常规方法**：直接从 PostgreSQL 网站下载安装程序。
- **Docker 容器中的 PostgreSQL**：直接将我们的应用程序连接到容器。

### 常规方法 – 在 Windows、macOS 和 Linux 上安装

PostgreSQL 主要为类 Unix 平台开发。然而，它被创建为可移植的，可以在 Windows 和 macOS 平台上安装。

我们需要采取的第一步是通过此 URL 下载 PostgreSQL 安装程序：https://www.enterprisedb.com/downloads/postgres-postgresql-downloads。

![](img/7c3ddd2e5893c9b5a0220148ed886548_82_0.png)

图 4.1 – PostgreSQL 安装

三个操作系统的步骤相同，我们只需要配置一些设置：

1. 点击最新版本（**14.1**）并根据您的操作系统下载安装程序。

2. 下载成功后，打开安装程序，点击 **Next**，并指定 PostgreSQL 将安装的路径：

    图 4.2 – PostgreSQL 安装程序（指定安装路径）

    在前面的示例中，我们选择了默认安装路径。再次点击 Next，它会询问我们想要安装哪些组件。我们选择的组件如下：

    - **PostgreSQL Server**：安装运行数据库的服务器
    - **pgAdmin 4**：用于与数据库交互的 GUI 管理工具
    - **Stack Builder**：允许我们下载和安装与 PostgreSQL 兼容的驱动程序的 GUI
    - **命令行工具**：使用命令行工具与 PostgreSQL 交互。

    图 4.3 – PostgreSQL 安装程序（选择所需的组件）

3. 再次点击 **Next**，系统会要求您指定存储数据的目录：

    ![](img/7c3ddd2e5893c9b5a0220148ed886548_84_0.png)

    图 4.4 – PostgreSQL 安装程序（选择数据的目录）

    在前面的示例中，我们可以看到默认路径与 PostgreSQL 安装位置相同，并且创建了一个名为 data 的新文件夹。建议您使用默认路径。

4. 点击 **Next**，这将让您为超级用户（`postgres`）配置密码。

5. 点击 **Next**，这将允许您设置 `postgres` 数据库使用的端口。

6. 再次点击 **Next**，系统现在会询问数据库集群应使用什么区域设置：

    ![](img/7c3ddd2e5893c9b5a0220148ed886548_84_1.png)

    图 4.5 – PostgreSQL 安装程序（为数据库集群选择区域设置）

    在前面的示例中，我们已将 [Default locale] 选为数据库的区域设置。

7. 再次点击 **Next**，将显示我们配置的所有设置；在继续之前请确保所有详细信息正确。

8. 查看后，点击 **Next**，现在将在我们的终端中安装 PostgreSQL。

    安装后，我们可以通过检查当前安装的版本来验证 PostgreSQL 是否成功安装。

9. 为此，打开 **SQL Shell (psql)** 并输入我们的数据库信息，例如 Server、Port、Database、Username 和 Password。

10. 由于我们使用了默认设置，我们可以一直按 *Enter* 直到密码确认。成功验证密码后，执行 `select version()` 命令以显示当前安装的 PostgreSQL：

    ```
    Server [localhost]:
    Database [postgres]:
    Port [5432]:
    Username [postgres]:
    Password for user postgres:
    psql (13.4)
    WARNING: Console code page (437) differs from Windows code page (1252)
            8-bit characters might not work correctly. See psql reference
            page "Notes for Windows users" for details.
    Type "help" for help.

    postgres=# select version();
            version
    ------------------------------------------------------------
     PostgreSQL 13.4, compiled by Visual C++ build 1914, 64-bit
    (1 row)

    postgres=#
    ```

    图 4.6 – PostgreSQL 安装程序（显示 PostgreSQL 的版本）

    在前面的示例中，我们可以看到我们已成功在终端上安装了 **PostgreSQL 版本 13.4**。

现在，让我们学习如何使用 Docker 安装和配置 PostgreSQL。

### Docker 容器中的 PostgreSQL

我们已使用常规安装程序在终端上安装了 PostgreSQL；现在，我们将学习如何使用 Docker 配置 PostgreSQL。此方法将帮助我们跳过配置 PostgreSQL 的复杂步骤，以便我们开始开发并提供数据库管理的 GUI：

1. 您需要做的第一步是在终端上安装 Docker。您可以在以下链接安装 Docker：`https://docs.docker.com/get-docker/`。有关 Docker 系统要求和安装步骤的文档，您可以参考此链接：`https://docs.docker.com/desktop/windows/install/`。

2. 成功安装 Docker 后，打开 Docker Desktop 并在终端上启动 Docker。然后，打开命令行并执行以下命令：

    ```bash
    docker run --name postgresql-container -p 5434:5434 -e POSTGRES_PASSWORD=pass -d postgres
    ```

    前面的命令将从 Docker Hub 拉取 PostgreSQL。命令中的 postgresql-container 部分可以替换，因为这是我们可以定义的一个容器名称。POSTGRES_PASSWORD 参数是 postgres 管理员的密码，我们也可以配置它。

3. 执行命令后，我们可以通过执行 `docker ps -a` 命令或查看 Docker Desktop 来验证新创建的容器，以检查正在运行的容器列表：

    ![](img/7c3ddd2e5893c9b5a0220148ed886548_86_0.png)

    图 4.7 – 使用 Docker 的 PostgreSQL 安装

    在前面的示例中，我们已执行 `docker ps -a` 命令，我们可以看到我们的 PostgreSQL 镜像已被拉取：

    ![](img/7c3ddd2e5893c9b5a0220148ed886548_87_0.png)

    图 4.8 – 使用 Docker 的 PostgreSQL 安装（在 Docker Desktop 中查看容器）

4. 我们也可以在 Docker Desktop 中查看已拉取的 postgresql-container 并在终端中验证其状态。

我们已成功使用 Docker 配置了 PostgreSQL。我们可以通过使用终端 IP 地址和端口创建新服务器将其连接到 pgAdmin。

我们已在终端中配置了 PostgreSQL 数据库。现在，我们将学习 Spring Data JPA 及其在开发 Spring 应用程序中的重要性。

## Spring Data JPA

Spring Data JPA（Java Persistence API）是用于在 Java 应用程序中管理关系数据的广泛使用的规范。它有助于开发 Spring，因为它通过不实现读写操作减少了样板代码。它还处理 JDBC 基于的数据库访问和对象关系映射所涉及的复杂过程。

在讨论 Spring Data JPA 之前，让我们讨论其明确的优势以及为什么它在 Spring 开发中常用。

### Spring Data JPA 的优势

以下是 Spring Data JPA 的优势：

- **无代码仓库**：Spring Data JPA 推广 *无代码* 仓库，这意味着我们不必编写仓库模式，这会产生大量重复代码。它提供了一组接口，我们可以使用它来扩展我们的类以应用数据特定的实现。

