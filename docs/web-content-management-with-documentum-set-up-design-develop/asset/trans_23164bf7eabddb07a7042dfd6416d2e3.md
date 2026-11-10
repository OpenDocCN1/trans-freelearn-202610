
## 5 设置 Documentum Suite

够了理论，对吧？让我们现在准备好潜入 Documentum 的海洋吧！

此处的假设是您已理解 WCM 系统的需求，已很好地分析了业务需求，并且渴望从 Documentum WCM 产品套件中获得最大收益。

在我们开始旅程之前，请确保您已从 Documentum 采购了足够的许可证。从 Documentum 的下载站点下载各种 Documentum 产品的正确版本：

```
https://documentum.subscribenet.com
```

请记住，在获得软件安装包访问权限之前，您需要输入有效的登录名和密码。

一旦您从 Documentum 网站下载了可执行文件，您面临的最重要问题是从何开始。其他关键方面包括：正确的安装顺序、任何先决条件、需要注意的事项等等。

坐下来放松一下——为您已做了不少功课。图 5.1 是一个您可以遵循的安装顺序图表。这不应被视为最佳的 Documentum 安装顺序，但在大多数项目中它会运作良好。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_73_0.png)

Figure 5.1: Documentum 安装顺序

### 5.1 安装 Content Server

我们将安装适用于 Windows 的 Content Server 5.2.5 SQL Server（基础安装），然后使用 SP2（服务包 2）补丁进行更新。

强烈建议您阅读相关 Documentum 软件的完整发行说明和安装手册。书中提到的内容只是应遵循的清单以及需要特别注意的内容。

开始之前，请从 Documentum 网站下载以下 Content Server 软件安装程序：

- content_server_5.2.5_windows_sql.exe
- content_server_5.2.5_SP2_windows_sql.exe

请注意，在整本书中，我们将引用在 Windows 环境下使用 SQL Server DB、IIS Web 服务器和 Tomcat 应用服务器的 Documentum 产品版本 5.2.5 SP2。安装步骤和配置仅适用于特定产品版本 5.2.5 SP2。如需升级路径或了解更多关于最新 Documentum 套件版本的信息，请联系您当地的 Documentum 支持。

在 Windows 主机上只能安装一个 Content Server。请勿尝试在同一台 Windows 机器上安装两个内容服务器。然而，在 UNIX 或 Linux 环境中这是可能的。此外，在 UNIX 和 Linux 系统上，可以在单个主机上创建多个安装，包括服务器不同版本的安装。例如，5.2 和 5.3 安装可以同时存在于单个 UNIX 主机上！

安装完 Content Server 后，可以为相关 Content Server 创建多个 Docbase。

### 5.1.2 先决条件

图 5.2 提到了开始安装 Content Server 的一些先决条件：

| 实体 | 注释 |
|---|---|
| Database | 应可在本地或远程使用并正确设置。代码页应根据 Content Server 的 UTF-8 代码页设置。对于 SQL Server，您可以使用任何排序规则（即代码页）。区分大小写的 SQL Server 应安装行级锁定。无论数据库是本地还是远程，SQL Server 客户端都应安装在 Content Server 机器上。Docbase 所有者（即数据库用户）是用于连接底层 RDBMS 的账户。此账户应可用。 |
| SMTP Server | SMTP 服务器应可在网络上使用或存在于 Windows 2000 机器上。 |
| Network | 应提供正确设置的 TCP/IP 网络。 |
| Environment Variables | 将 PATH 系统变量设置为指向 'Java' |
| Installation Owner | 安装 Content Server 软件的用户应具有有效的操作系统账户。安装所有者用户名的限制：由数字、字母、连字符 (-) 和下划线 (_) 组成。第一个字符应为字母。所有字符必须是 ASCII。如果 SQL Server 数据库安装在与 Content Server 不同的域中，安装所有者应为远程域中的有效用户。 |
| Hardware | 700 MB 硬盘空间、512 MB RAM 和最低 400 MHz CPU。 |
| Java Runtime Environment | Sun JRE 1.3.1、1.4.1（以及适用于 AIX 操作系统的 IBM J2RE 1.3.1） |
| Others | 有关受支持的 Windows 操作系统、RDBMS 版本和 JRE 的组合，请参阅 Documentum 的发行说明。 |

Figure 5.2: Content Server 安装先决条件

> 不要在 Documentum 的安装目录名称中使用空格。例如，不要安装在 c:\Documentum Installation\ 下。

不要再拖延了！让我们开始安装 Content Server 5.2.5，然后在其上安装 SP2 补丁。

### 5.2 安装 Content Server 5.2.5

按照以下说明安装 Content Server：

1. 以安装所有者身份登录，在 Content Server 机器上解压 content_server_5.2.5_windows_sql.exe 并运行 ContentServer.exe。
2. 单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_75_0.png)

Figure 5.3: Content Server 安装初始屏幕

3. 选择安装目录或继续使用显示的默认值。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_76_0.png)

Figure 5.4: Content Server 安装目标目录屏幕

4. 选择 DFC 目标目录或继续使用显示的默认值。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_76_1.png)

Figure 5.5: Content Server 安装 DFC 目标目录屏幕

5. 选择用户目录或继续使用显示的默认值。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_77_0.png)

Figure 5.6: Content Server 安装用户目录屏幕

6. 提供 DocBroker 主机名称和端口号或选择默认值。Documentum 默认将当前 Content Server 机器作为 DocBroker 主机。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_77_1.png)

Figure 5.7: DocBroker 主机和端口屏幕

7. 如有需要，可选择安装 DFC 开发人员文档，然后单击“下一步”按钮。
8. 如有需要，可选择启用受信任内容服务（TCS）并提供许可证密钥。然后单击“下一步”按钮。
9. 如有需要，可选择启用 EMC Centera 的内容服务并提供许可证密钥。然后单击“下一步”按钮。
10. 可选择为启动和关闭 Apache Tomcat 服务器（随 Content Server 安装提供）提供替代端口号，或选择默认值。然后单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_78_0.png)

Figure 5.8: Documentum Java 方法服务器屏幕

11. 如果需要继续创建 Docbase，请选择“立即配置服务器”选项。我们将先完成 Content Server 安装，然后升级到 5.2.5 SP2。稍后我们将创建 Docbase。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_79_0.png)

Figure 5.9: 服务器配置屏幕

12. 安装完成后系统需要重新启动。我们将先安装 Content Server 5.2.5 SP2 补丁，然后重启机器。

### 5.3 安装 Content Server 5.2.5 SP2

1. 以安装所有者身份登录，在 Content Server 机器上解压 Content_Server_5.2.5_SP2_windows_sql.exe 并运行 ContentServerSP.exe。

2. 单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_80_0.png)

Figure 5.10: Content Server 5.2.5 SP2 安装初始屏幕

3. 如有需要，可选择安装 DFC 开发人员文档，然后单击“下一步”按钮。

4. 在 Content Server 机器上停止以下服务：
- Any existing Docbases
- Any existing DocBrokers
- Documentum Java Method Server

5. 我们将完成安装，稍后配置服务器（即创建 Docbase）

![](img/b18244e6621ecc39cef03ff6c0e55e7c_81_0.png)

Figure 5.11: 服务器配置屏幕

6. 重启 Content Server 主机机器。

就这样！我们已完成 Content Server 5.2.5 SP2 的安装。在后续章节中，我们将创建和配置一个 Docbase，以便在新创建的 Docbase 中开始开发自定义对象。

### 5.4 摘要

本章首先讨论了涉及以下工具的简单 WCM 系统的产品安装顺序：

- Content Server
- Site Caching Services
- Documentum Application Builder
- Documentum Administrator
- Web Publisher

我们讨论了安装 Documentum Content Server 的先决条件，然后讨论了在 Windows 平台上安装 Content Server 5.2.5 SP2 的步骤。
