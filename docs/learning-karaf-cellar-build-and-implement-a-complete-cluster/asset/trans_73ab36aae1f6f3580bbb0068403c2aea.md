

OSGi 应用的部署称为 provision，因为它包含以下内容：

- 一组 bundle 的安装，包括传递性 bundle
- 这些 bundle 所需的一组配置的安装

## OBR

OSGi Bundle Repository（OBR）可以是解决此问题的首选方案。Apache Karaf 可以连接到 OBR 服务器。OBR 服务器存储所有 bundle 的元数据，其中包括 bundle 提供的 capabilities、packages 和 services，以及 bundle 所需的 requirements、packages 和 services。当你通过 OBR 安装一个 bundle 时，OBR 服务器会检查已安装 bundle 的需求并找到提供与需求匹配的 capabilities 的 bundle。OBR 服务器可以自动安装第一个 bundle 所需的 bundle。

## Apache Karaf Features

作为一个轻量级且独立的 OSGi 容器，Apache Karaf 提出了另一种 provision 应用的方式。Apache Karaf Features 是 Apache Karaf 中的默认 provision 解决方案。

Karaf Features 描述了一个应用，声明以下方面：

- 应用中的 bundle
- 应用所需的其他 Karaf features
- 应用所需的配置

一个 Karaf Feature 仓库是一个简单的 XML 文件，描述了一组 Karaf features。
一个 Karaf Feature 仓库如下所示：

```
<features name="my-features">

    <feature name="core-feature" version="1.0">
        <bundle>mvn:bundleA_groupId/bundleA_artifactId/bundleA_version</bundle>
        <bundle>file://path/to/bundleB</bundle>
        <bundle>http://path/to/bundleC</bundle>
    </feature>

    <feature name="extend-feature" version="1.0">
        <feature version="[1,2)">core-feature</feature>
```

为了启用文件系统锁定系统，你必须按如下方式更新 etc/system.properties 配置文件：

```
karaf.lock=true
karaf.lock.class=org.apache.karaf.main.SimpleFileLock
karaf.lock.dir=/path/to/lockfile
karaf.lock.delay=10
```

当共享文件系统不可用时（例如出于安全或基础设施原因），你可以使用数据库来存储锁。使用数据库锁定，Apache Karaf 会对表进行锁定（默认情况下为 KARAF_LOCK 表）。任何支持 JDBC 的数据库都可以使用。

配置也定义在 etc/system.properties 配置文件中，如下所示：

```
karaf.lock=true
karaf.lock.class=org.apache.karaf.main.DefaultJDBCLock
karaf.lock.level=50
karaf.lock.delay=10
karaf.lock.jdbc.url=jdbc:derby://dbserver:1527/sample
karaf.lock.jdbc.driver=org.apache.derby.jdbc.ClientDriver
karaf.lock.jdbc.user=user
karaf.lock.jdbc.password=password
karaf.lock.jdbc.table=KARAF_LOCK
karaf.lock.jdbc.clustername=karaf
karaf.lock.jdbc.timeout=30
```

你必须将 JDBC 驱动 JAR 文件复制到 lib/ext 文件夹中。Apache Karaf 为某些特定数据库提供了专用的 JDBC 锁实现（DefaultJDBCLock 是通用的，OracleJDBCLock 用于 Oracle 数据库，DerbyJDBCLock 用于 Derby 数据库，MySQLJDBCLock 用于 MySQL 数据库，PostgreSQLJDBCLock 用于 PostgreSQL 数据库，以及 SQLServerJDBCLock 用于 Microsoft SQLServer 数据库）。

Apache Karaf 锁定机制为高可用性提供了一个良好的解决方案。然而，只有 一个 Apache Karaf 实例处于活动状态（主节点）；所有其他实例都处于非活动状态（备用/主节点）。

为了同时提供高可用性和性能可扩展性，拥有多个活动 Apache Karaf 实例是一个很大的优势。

## Provisioning clusters

想象一下，你有一个 Apache Karaf 容器的集群，每个容器都在不同的机器上。如果你想在容器实例上 provision 一个 OSGi 应用，你必须连接到每个容器并安装 features。

这意味着你必须执行以下任务：

- 登录到每个容器以反复执行相同的操作
- 最终，根据每个本地实例调整配置（端口号、文件路径等）
- 添加新实例，这将再次需要相同的操作

基本上，这意味着大量的人工操作，存在出错的风险。这就是 provision 集群发挥作用的地方。

provision 集群的目的是保持多个容器实例同步。对于 Apache Karaf 来说，这意味着资源状态的更改将被广播到同一集群中的所有容器成员。

资源可以是 bundle、feature、配置或节点本地的任何类型资源。这意味着本地操作会发送事件以更新集群中的其他成员。

另一方面，也可以创建一个集群事件发送给所有成员以更新它们。

基本上，这意味着 provision 集群执行以下任务：

- **创建事件**：此事件可以由于本地更改或手动创建
- **广播事件**：此事件发送给集群的成员

如果 provision 是 provision 集群的首要目的，这并不意味着它不能提供在集群拓扑中有用的额外功能。例如，集中式日志、负载均衡器、会话复制等都是可以在 provision 集群之上提供的有趣功能。在下一章中，我们将看到 Karaf Cellar 作为 provision 集群解决方案。

## Summary

在本章中，我们回顾了 OSGi 的目标以及一些核心组件（bundle、manifest 等）。我们还简要介绍了 Apache Karaf OSGi 容器，描述了提供的不同功能。最后，我们讨论了将多个 Apache Karaf 实例一起使用的方法：主动/被动方式（故障转移）或主动/主动方式（provisioning cluster）。

下一章将介绍 Apache Karaf Cellar provisioning cluster。

## 2
### Apache Karaf Cellar

Apache Karaf Cellar 是 Apache Karaf 的 provisioning cluster 解决方案。

它是一个在 Apache 2 许可证下发布的 Apache Karaf 子项目。欲了解更多信息，请访问 http://karaf.apache.org/index/subprojects/cellar.html。

Apache Karaf Cellar 允许你管理分布在不同机器和网络上的 Apache Karaf 实例集群。

作为一个 provisioning cluster 解决方案，Apache Karaf Cellar 同步不同实例的状态。通过同步，我们的意思是节点上的资源操作将导致通知其他节点进行更改。

本章将涵盖以下主题：

- 集群拓扑
- Apache Karaf Cellar 架构
- Apache Karaf Cellar 安装
- 集群资源

## Cluster topologies

当我们谈论 provisioning clusters 时，这意味着我们必须在不同实例之间存储和共享资源的状态。

我们将节点命名为 provisioning cluster 的实例成员。

我们可以看到两种共享资源状态的方法。

第一种方法是使用中央组件来存储状态。该中央组件名为 `cluster manager`。它负责存储节点之间共享的数据以及节点之间的通信。请看下图：

![](img/86f349f6a758f2c3088dbeb4d35dcaca_34_0.png)

所有节点都连接到存储资源状态的中央组件。例如，该组件可以是 Apache ZooKeeper。

这种方法的优点是 provisioning cluster 对节点没有影响；节点只需与中央组件通信。

cluster manager 不仅仅是一个简单的共享存储，因为它必须处理节点之间的通信和信息传输。

这种方法的缺点是 cluster manager 是一个单点故障。

为了保证可靠的集群和高可用性，cluster manager 本身应该是集群化的：我们必须提供并安装一个 cluster manager 集群。

这意味着你有两个层级的集群，如下所示：

- 节点之间的实际集群
- **Cluster Manager** 集群，仅用于保证高可用性

