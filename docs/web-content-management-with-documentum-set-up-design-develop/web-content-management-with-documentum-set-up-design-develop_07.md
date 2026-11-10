## 7 设置发布

在设置好 Docbases 后，我们现在可以配置 Documentum Site Caching Services（简称 SCS），以便将内容和属性从 Documentum 发布到我们的网站。这需要以两种组件的形式安装 Documentum SCS 软件：

- SCS Source 组件（安装在托管 Docbase(s) 的 Content Server 机器上）
- SCS Target 组件（安装在 Web 服务器上）

您可能希望快速浏览我们在第 4 章中关于通过 SCS 服务发布的讨论，以回忆架构。

假设您了解从 Documentum 到网站发布的基本流程，我们可以继续进行安装。我们将首先在 Content Server 机器上安装 SCS Source 5.2.5，然后使用 SP2 补丁对其进行升级。

一旦 SCS Source 软件正确安装，我们将在 Web 服务器上安装 SCS Target 5.2.5，然后使用 SP2 补丁对其进行升级。

## 7.1 SCS Source 5.2.5

您可以在具有不同操作系统和数据库的机器上运行 SCS Source 和 Target。例如，您可以在安装了 SQL Server 数据库的 Windows 机器上配置 SCS Source，并在安装了 Oracle 数据库的 UNIX 机器上配置 SCS Target。但是，您应确保这是 Documentum 根据发布的发行说明认证的组合。

### 7.1.1 先决条件

图 7.1 列出了在 Content Server 上安装 SCS Source 之前我们必须处理的某些先决条件：

| 实体 | 注释 |
|---|---|
| Content Server | 在安装 SCS Source 之前，Content Server 应正确设置并可用。服务器安装中的 Content Server 和 Docbases 应处于运行状态。 |
| Database | Docbase 使用的数据库实例应可访问 |
| Hardware | 80 MB 硬盘空间、512 MB RAM 和最低 400 MHz CPU。安装后，主机应具有足够的磁盘空间来存储发布的数据（导出数据集） |
| Others | 有关受支持的 Windows OS、RDBMS 版本和 JRE 的组合，请参阅 Documentum 的发行说明。 |

图 7.1：安装 SCS Source 的先决条件

开始时，请从 Documentum 网站下载以下 SCS 软件安装程序：

- `site_Caching_services_5.2.5_windows_source.exe`
- `site_Caching_services_5.2.5_SP2_all.zip`

### 7.1.2 安装 SCS Source 5.2.5

我们将首先在 Content Server 主机上安装 SCS Source 5.2.5，然后使用服务包 2（SP2）补丁对其进行升级。

1. 以 Documentum 安装所有者的身份登录到 Content Server 主机，解压 `site_Caching_services_5.2.5_windows_source.exe`，并运行以下可执行文件：`SCSSourcewinSuiteSetup.exe`。

2. 单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_100_0.png)

图 7.2：SCS Source 安装的初始屏幕

3. 接受许可协议并单击“下一步”按钮。

4. 可选择安装开发者文档并单击“下一步”按钮。

5. 提供 Content Server 安装所有者的密码并单击“下一步”按钮。

6. 提供 SCS Source Tomcat 实例和管理员实例的主要端口号。我们可以指定当前未被任何其他应用程序使用的端口，或使用安装程序提供的默认值。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_101_0.png)

图 7.3：指定 SCS Source 服务的端口

7. 显示要安装的组件。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_101_1.png)

图 7.4：要安装的组件列表

8. SCS Source 安装程序需要配置 Content Server 机器上现有的 Docbases。请注意，我们始终可以跳过此选项，并稍后通过运行 %DM_HOME%\webcache\install 中的可执行文件 config.exe 来配置相关 Docbase。Windows 机器上的 DM_HOME% 通常为：C:\Documentum\product\<product version>

![](img/b18244e6621ecc39cef03ff6c0e55e7c_102_0.png)

图 7.5：运行配置程序

9. 选择需要运行 SCS 配置程序的 Docbase 并单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_102_1.png)

图 7.6：选择用于 SCS Source 配置的 Docbase

10. 配置程序在所选 Docbase 上安装 SCSDocApp，将 Docbase 配置为与 SCS 服务一起使用。以下对象作为 SCSDocApp 的一部分安装：

- `dm_webc_config` 对象类型
- `dm_webc_target` 对象类型
- `webc_lock` 对象类型
- `dm_SCSLogPurgeJob` 作业
- `dm_SCSLogPurge` 方法
- `PostInstall` 过程

DocApp 安装完成后单击“完成”。

11. 重启服务 Documentum SCS_source_<port number>（例如：如果 SCS Source 配置在默认端口 6677 上，则为 Documentum SCS_source_6677）。

这完成了 SCS Source 5.2.5 的安装。我们现在可以安全地在此安装之上安装服务包 2。

> 每次创建新的 Docbase 时，我们需要在 Content Server 主机上从 %DM_HOME%\webcache\install 运行 SCS Source 配置程序（config.exe）。如果不这样做，将导致从相关 Docbase 发布时出现错误。

### 7.1.3 安装 SCS Source 5.2.5 SP2

1. 在 Content Server 主机上解压 Site_caching_services_5.2.5_SP2_all.zip 并将 webcache.jar 文件复制到 Content Server 主机上的以下位置：`%DM_HOME%\webcache\tomcat\webapps\webcache\WEB-INF\lib`

示例：Windows 上的典型位置可以是：
`c:\documentum\product\5.2\webcache\tomcat\webapps\webcache\WEB-INF\lib`

2. 重启 SCS Source 服务。

就这样！我们已完成将 SCS Source 升级到 SP2，现在可以安装 SCS Target 5.2.5，然后使用服务包 2 对其进行升级。

> 您可以通过浏览器查看以下 URL 来监视 SCS Source 的状态，以了解其产品版本以及总正常运行时间、总增量发布等其他信息：http://<Content Server host>:6677/webcache/state

如果 SCS Source 服务未运行，则在尝试从 Docbase 使用 Site Caching Services 发布时会出现以下错误消息：

Error occurred while publishing the Site Publishing Configuration
[DM_METHOD_E_HTTP_COMMUNICATION]error: "Failed to obtain socket for host:localhost, at port:6677"

## 7.2 SCS Target 5.2.5

我们将首先在 Web 服务器机器上安装 SCS Target 软件，然后在其上应用 SP2 包。

### 7.2.1 先决条件

图 7.7 列出了在 Web 服务器主机上安装 SCS Target 的一些先决条件：

| 实体 | 注释 |
|---|---|
| SCS Target installation owner | 在 Windows 机器上，用户应： - 是 Windows 'Administrators' 组的成员 - 具有“作为操作系统的一部分运行”特权（转到 Programs \| Administrative Tools \| Local Security Policy） - 具有访问 SCS 目录的权限 |
| Transfer user | 应知道 SCS Target 机器上有效 OS 用户的用户名和密码 |
| Web Server | 用于应用程序服务器或 Web 应用程序的 Web 服务器应处于运行状态 |
| Database | 如果配置了 SCS Target 软件，数据库实例应可访问 用户名和密码应可用于数据库连接 DB 系统用户名和密码应可用于允许 Site Caching Services 创建数据库/表空间。 |
| Hardware | 250 MB 硬盘空间、128 MB RAM 和 133 MHz CPU 安装后，网站主机应具有足够的磁盘空间来存储发布的数据。 |
| Others | 有关受支持的 Windows OS 和 RDBMS 版本的组合，请参阅 Documentum 的发行说明。 |

图 7.7：安装 SCS Target 的先决条件

开始时，请从 Documentum 网站下载以下 SCS Target 软件安装文件：

- `site_caching_services_5.2.5_windows_target.exe`
- `site_caching_services_5.2.5_SP2_all.zip`

### 7.2.2 安装 SCS Target 5.2.5

我们将首先在 Web 服务器主机上安装 SCS Target 5.2.5，然后使用服务包 2（SP2）对其进行升级。

1. 以 SCS 安装所有者的身份登录到 Web 服务器主机，解压 `site_caching_services_5.2.5_windows_target.exe` 并运行以下可执行文件：`SCSTargetwinSetup.exe`



- 2. 如果需要安装并配置 SCS Target 以使用数据库，则需要在事先设置 DSN，以便能够建立到指定数据库的连接。
- 在 SCS Target 机器上设置 DSN（数据源名称），方式与我们创建第一个 Docbase 时相同。请记住，可以通过“程序 | 管理工具 | 数据源（ODBC）”在 Windows 机器上创建 DSN。
- 3. 运行 SCS Target 安装程序时，会显示以下屏幕。单击“下一步”按钮。
- 4. 接受许可协议并单击“下一步”按钮。
- 5. 选择显示的默认安装目录或提供新路径。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_105_0.png)

图 7.8：安装 SCS Target 的初始屏幕

![](img/b18244e6621ecc39cef03ff6c0e55e7c_106_0.png)

图 7.9：SCS Target 的目标目录

- 6. Documentum SCS 安装目录会显示给用户以供预览。单击“下一步”按钮。
- 7. 安装完成后，运行配置程序以配置 SCS Target 软件。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_106_1.png)

图 7.10：运行 SCS Target 的配置程序

- 8. 显示用于配置 SCS Target 的确认屏幕。单击“下一步”按钮。
- 9. 选择 Content Server 主机机器与目标机器之间是否需要安全（HTTPS）或非安全（HTTP）连接。

请记住，安全（HTTPS）通信比非安全通信慢约 30%。如果您的网络以及 Content Server 主机与目标机器之间的流量是安全的，则可以选择非安全通信。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_107_0.png)

图 7.11：指定连接类型

- 10. 如果选择了非安全通信（HTTP），则可以选择默认端口 2788。如果选择了安全通信（HTTPS），则显示的默认端口为 2787。我们也可以提供其他端口号，前提是该端口未被其他应用程序使用。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_108_0.png)

图 7.12：指定端口号

- 11. 指定目标数据传输目录。我们可以选择显示的默认值或提供其他方便的位置。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_108_1.png)

图 7.13：数据传输目录

- 12. 指定 SCS 软件通过其对传输用户密码进行身份验证的域或服务器。单击“下一步”按钮。
- 13. 我们可以选择在不使用数据库（用于存储对象属性/元数据）的情况下配置 SCS Target 软件，或选择数据库类型并单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_109_0.png)

图 7.14：指定数据库

- 14. 我们可以拥有已存在的数据库和账户，或使用安装向导创建一个新的。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_109_1.png)

图 7.15：设置数据库账户

- 15. 从下拉列表中选择现有的 ODBC 数据源，并提供数据库系统用户名和密码。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_110_0.png)

图 7.16：选择现有数据源

- 16. 要么选择默认的数据库名称和相关信息，要么自行提供数据库参数（数据设备文件路径和日志设备文件路径）。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_110_1.png)

图 7.17：指定数据库信息

- 17. 提供数据库用户名称和密码，然后单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_111_0.png)

图 7.18：指定数据库用户凭据

- 18. JDBC 驱动程序和数据库连接 URL 信息会显示给用户。
我们可以通过选择“自定义”复选框来自定义 JDBC 驱动程序和/或数据库连接 URL。单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_111_1.png)

图 7.19：数据库连接 URL 和 JDBC 驱动程序信息

- 19. 选择启动 SCS Target 服务的选项并单击“下一步”按钮。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_112_0.png)

图 7.20：启动 SCS Target 实例

- 20. 单击“完成”以完成 SCS Target 安装和配置过程。

我们已在 Web 服务器主机上完成了 SCS Target 5.2.5 的安装。下一步任务是为其应用服务包 2。作为 SCS Target 配置的一部分，Documentum 会在 SCS Target 主机的以下位置修改 agent.ini 文件：

```
<SCS Target Installation Directory>\admin\config\<SCS Target port>
```

在 Windows 上，典型位置为： C:\Documentum\SCS\admin\config\2788

以下片段来自 agent.ini 文件，其中存储了我们刚刚安装的 SCS Target 的数据库信息：

```
target_database_connection=jdbc:odbc:SCS_TARGET_PUBLISHING_DSN
database_user=sa
database_user_pass=systempass
JDBC_DRIVER=sun.jdbc.odbc.JdbcOdbcDriver
database_name=WebCache_2788
```

### 7.2.3 安装 SCS Target 5.2.5 SP2

我们现在将在 Web 服务器主机上安装 SCS Target 5.2.5 SP2。

- 1. 停止 SCS Target 服务并在 Web 服务器主机上解压 site_caching_services_5.2.5_SP2_all.zip。此 ZIP 包含 webcache.jar 文件。
- 2. 将解压后的 webcache.jar 文件复制到 Web 服务器主机上的以下位置：
- `<SCS Target Installation Directory>\product\jre\win\lib\ext`
- 示例：Windows 上的典型位置可以是： `c:\Documentum\SCS\product\jre\win\lib\ext`
- 3. 重启 SCS Target 服务。

如果 SP2 升级未正确完成，在发布期间如果 Content Server 主机和 Web 服务器主机（SCS Source 和 Target）上的 webcache.jar 版本不匹配，则会看到以下错误消息：

```
ERROR: Failed to login to the target server. Network error retrieving target result
com.documentum.webcache.utils.d: Local class not compatible: stream
classdescserialVersionUID=1602290069170353386 local class
serialVersionUID=4590497449032455007
```

就这样！我们已完成将 SCS Target 升级到 SP2。现在我们可以从 Documentum 发布内容和元数据了。

## 7.3 摘要

在本章中，我们讨论了设置 Site Caching Services（SCS）组件以发布在我们新的 Docbase 中创建的文档。我们看到 SCS 架构由两个组件组成：

- SCS Source，安装在 Content Server 主机上
- SCS Target 组件，安装在 Web 服务器主机上

我们讨论了安装 SCS Source 和 SCS Target 的一些先决条件，然后详细介绍了它们的安装步骤。
