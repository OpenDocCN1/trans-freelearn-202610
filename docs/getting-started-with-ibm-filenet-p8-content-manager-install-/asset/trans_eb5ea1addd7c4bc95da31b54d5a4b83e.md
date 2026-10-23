

对于给定的 WAS 配置文件，您也可以通过该配置文件的 WAS 管理控制台找到这些端口号。如果您使用了 WAS 管理控制台的默认端口号，则可以使用 URL http://wjc-rhel.example.net:9060/ibm/console 或 http://wjc-rhel.example.net:9061/ibm/console 分别连接到 AppSrv01 或 AppSrv02。从那里，导航到 Servers | Server Types | WebSphere application servers | server1 | Configuration | Ports。以下屏幕截图适用于 AppSrv02：

![](img/2295191c450a0c86b2d82df6cec04526_68_0.png)

## WAS 进程属性

准备指南中指出了 CE Java 进程的 UMASK 设置。在类 Unix 操作系统中，UMASK 控制由进程创建的文件的默认权限。

> UMASK 有时被认为是一个用户创建的文件的默认权限，但从技术上讲它是一个正在运行进程的设置。它在用户的登录 shell 进程上设置，并被其他进程继承。

## 安装环境组件

- 将该 WAS 配置文件的所有文件更改为我们在表单上设置的用户和/或组
- 启动该 WAS 配置文件（从 root 账户或我们为该配置文件配置的用户账户启动）

在本例中，我们以用户/组 ecmuser/ecmgroup 运行 AE 和 CE 两个配置文件。我们还在 AE 配置文件上将 UMASK 值设置为 007 仅为保持一致。我们可以只将这些字段留空并始终从 ecmuser 账户启动 WAS，但进行这部分配置可以防止我们犯错。我们可以从 root 账户启动 WAS，它仍然会以 ecmuser 运行。

运行以下命令以修复 AE 和 CE WAS 配置文件下文件的拥有者：

```shell
# cd /opt/ibm/WebSphere/AppServer/profiles
# chown -R ecmuser:ecmgroup AppSrv01 AppSrv02
```

## WAS 性能调整

需要对 CE 配置文件的 WAS 管理控制台进行少量性能调优配置。导航到 **Servers | Server Types | WebSphere application servers | server1 | Java and Process Management | Process definition | Java Virtual Machine**。指定至少 512 MB 的初始堆大小和至少 1024 MB 的最大堆大小。根据您可用的 RAM，您可能会发现指定更大的值对性能有益。

规划和准备指南中有一条关于可选增加 WAS 事务超时的说明。只有少数更新操作存在触发事务超时的风险，例如创建 ObjectStore 以及随后向 ObjectStore 安装 AddOns。WAS 默认值为 300 秒，除非您在这些操作中遇到问题，否则可以保留该值。

最后，有一个经常困扰人们的 WAS 配置重要细节。它是少数在测试或演示环境中比生产环境更可能成为问题的事项之一。前面我们展示了 WAS 管理控制台中 bootstrap 地址的屏幕截图。当时我们关注的是端口号，但现在让我们来看地址的主机部分。WAS 安装尽力确定我们主机的名称，但它只能得出 localhost。这对于从同一服务器连接来说完全可以，但要从任何其他机器连接到该 WAS 实例，bootstrap 主机名必须在客户端机器上可通过 DNS 解析。对于我们的系统，我们将至少为 bootstrap 地址输入 wjc-rhel.example.net。这对于 CM 目的通常已足够，但如果您愿意，也可以更改所有其他 localhost 条目。

根据您的 DNS 环境，您可能只需要想象输入服务器名称。域名可能无法在您的 DNS 中解析。在这种情况下，请改用该服务器的 IP 地址。

## WAS 和 DB2

现在是时候将我们的 WAS 和 DB2 组件结合起来了。这包括两项独立的工作。我们必须确保 WAS 使用与 CE 兼容的 JDBC 驱动程序与 DB2 通信，并且我们必须配置 WAS 数据源以供 CE 用于与 GCD 和 ObjectStores 通信。所有 CE 与数据库的通信都由 J2EE 应用服务器中介。

P8 硬件/软件指南给出了每个受支持数据库和平台的可用 JDBC 驱动程序。这通常以最低发布级别的形式说明，但偶尔会列出需要避免的特定驱动程序版本。由于我们在安装 DB2 时确保指定了 Java 支持，JDBC 驱动程序已作为默认实例的一部分安装在默认位置 /home/db2inst1/sqllib/java/。文件 db2jcc.jar 包含我们感兴趣的实际 JDBC 提供程序。要找到该文件中提供程序的确切版本号，我们使用 Java 来查询驱动程序本身。以下命令执行 JAR 文件中的 DB2Jcc 类并向其传递一个参数以请求版本信息。紧随其后的行是典型的输出。

```
$ cd /home/db2inst1/sqllib/java
$ java -cp db2jcc.jar com.ibm.db2.jcc.DB2Jcc -version
IBM DB2 JDBC Universal Driver Architecture 3.53.95
```

截至本文撰写时，P8 硬件/软件指南指定 DB2 9.5 的最低版本为 3.53.95（DB2 9.7 为 3.57.82），因此我们可以直接使用此已安装的驱动程序。如果不是这种情况，驱动程序软件可从 DB2 支持站点下载。这对于所有受支持的数据库都是典型情况。

要使 WAS 意识到此特定 DB2 驱动程序，请登录 CE 配置文件的 WAS 控制台，并导航到 Environment | WebSphere variables。将变量 DB2UNIVERSAL_JDBC_DRIVER_PATH 的值设置为 /home/db2inst1/sqllib/java。务必保存您的更改。

我们可以手动配置用于连接 GCD 和 ObjectStore 数据库所需的 JDBC 数据源，但我们将让 P8 Configuration Manager 应用在第 3 章安装实际 CE 软件时处理这些细节。如果您有兴趣了解如何手动设置数据源，可以在 WAS 文档中找到详细说明。如果您手动操作，请务必查阅 P8 文档以获取性能和功能方面的必要设置。

## 安装 Tivoli Directory Server

TDS 使用数据库（用于存储目录和配置数据）和应用服务器（用于托管其基于 Web 的管理工具）。安装程序将尝试检测已安装且可用的版本。如果未找到任何版本，它将使用安装程序提供的嵌入式 DB2 和 WAS 版本。

到目前为止，故事应该听起来很熟悉——使用“典型”路径安装 TDS，并接受各种事物的默认建议。这几乎就是情况。我们接下来展示的步骤使用 DB2 9.5 和“典型”安装路径。对于我们的安装，TDS 6.2 能够使用 DB2 9.5。对于 TDS 6.2 的某些早期版本，存在一个可能阻止正常启动的错误。我们接下来将向您展示如何规避该错误。但是，使用 DB2 9.7 时，“典型”路径运行顺畅，就像使用 DB2 9.5 和 TDS 6.2 的后续修复包时一样。

TDS 6.2 不会使用 WAS 7.0，因此它会安装自己的 WAS 6.1 嵌入式副本。如果您接受默认值，您将在 /opt/ibm/ldap/appsrv/ 下找到 WAS 的嵌入式副本。

> TDS 6.2 是 CM 4.5.1 支持的最新版本。CM 5.0 最终可能支持 TDS 6.3。TDS 6.3 反过来可以使用 WAS 7.0 作为其嵌入式应用服务器。在这种情况下，WAS 7.0 将被 TDS 安装程序自动检测并使用。

在“典型”安装路径中，TDS 创建一个单独的 DB2 实例 dsrdbm01，以及同名的操作系统用户账户。它还创建了一个目录管理操作系统用户 idsldap。

在启动 TDS 安装程序之前：

- 确保 DB2 管理服务器正在运行。这对于 TDS 创建自己的 DB2 实例和数据库是必需的。
- 设置一个环境变量来告诉 TDS 使用哪个 DB2 实例。我们现在就这样做，即使实例尚不存在，也只是为了让安装顺利进行。

