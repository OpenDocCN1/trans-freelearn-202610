## 集群事件的过滤

在上一章中，我们看到了如何使用生产者、消费者和处理器创建和传输集群事件。我们看到每个资源（bundle、feature 和 configuration）以及每个集群组都管理着一个内部队列。这使我们能够对节点进行分组，并将资源的供应定位到特定的组上。所以，这是节点级别的第一种过滤。

在本章中，我们将介绍以下主题：

- 如何在给定的集群组中过滤集群事件
- 如何定义要过滤的资源（bundles、features、configurations 和 OBRs）
- 如何定义过滤器的方向（从节点进入或离开）

### 过滤器的配置

集群事件的过滤在 `etc/org.apache.karaf.cellar.groups.cfg` 配置文件中定义。在此配置文件中，你可以找到具有以下格式的过滤器：

```
cluster_group.[bundle|config|features].[whitelist|blacklist].[inbound|outbound]=regex
```

例如，以下配置（blacklist）阻止了默认集群组中包含名为 my-feature 或 other-* 的 features 的传入集群事件（inbound）：

```
default.features.blacklist.inbound=my-feature,other-*
```

Cellar 为默认集群组提供了一个默认的过滤配置。当你创建一个新的集群组时，此配置用作模板。你可以通过直接在节点上修改配置或使用 config:* 和 cluster:config-* 命令实时更新过滤器配置。

例如，要向集群组添加新的过滤器，可以运行以下命令：

```
karaf@root> config:edit org.apache.karaf.cellar.groups
karaf@root> config:propset mygroup.bundle.blacklist.outbound none
karaf@root> config:update
```

你也可以使用 ConfigMBean 远程更新配置（例如使用 JConsole）。

集群组的名称是 cluster_group，即你在 cluster:group-create 命令中提供的名称。

### 资源

在上一章中，我们看到每个资源和每个集群组都有一个内部队列。

资源是集群事件的类型。Cellar 定义了以下集群事件：

- bundle：这是一个对应于 bundle 操作（install、uninstall、start、stop 等）的集群事件
- config：这是一个对应于配置操作（create、delete、propset、propappend、propdel 等）的集群事件
- features：这是一个对应于 feature 操作（addurl、delurl、install、uninstall 等）的集群事件

请记住，每种类型的集群事件由特定的处理器管理。

在过滤器定义中，在 `cluster_group` 之后指定资源。这将允许你过滤特定类型（`bundle`、`config` 和 `features`）的事件。

### 黑名单和白名单

在过滤器定义中，你可以在资源之后指定 `blacklist` 或 `whitelist` 关键字。被列入黑名单的过滤器将阻止相应的集群事件。这意味着集群事件不会在集群中传递。另一方面，被列入白名单的过滤器将允许相应的集群事件。过滤器是通过先评估白名单然后再覆盖黑名单来构建的。通过组合黑名单和白名单过滤器，你可以定义细粒度的过滤。

### 入站和出站

用于配置过滤器的另一个属性是集群事件的方向。
两种方向可以区分如下：

- `inbound`：`inbound` 集群事件是从另一个节点进入的事件，并进入其他节点。
- `outbound`：`outbound` 集群事件是从节点发出的事件。这些集群事件由节点产生并发送到其他节点。

得益于入站和出站过滤器，你可以根据其方向：传入或传出，来过滤相同的集群事件。

### 正则表达式和事件标识

最后，我们必须指定集群事件。其目的是允许（`whitelist`）或阻止（`blacklist`）特定的集群事件。例如，我们想要阻止以下情况：

- 包含名为 `my-feature` 的 feature 更改的集群事件
- 包含名为 `my-bundle` 的 bundle 更改的集群事件

因此，我们必须标识并声明集群事件。要标识集群事件，我们可以使用以下方法：

- 可以使用事件标识符；例如，`org.apache.karaf.cellar.groups` 标识配置的集群事件（PID）。
- 可以使用基于事件标识符的 **正则表达式（regex）** 或 glob。声明事件使用完全限定名称可能很繁琐。使用 regex 速度要快得多。例如，`org.apache.karaf.cellar.*` 标识包含 PIDs 的所有配置集群事件。
- `*` 选择所有集群事件。
- `none` 关键字是保留关键字，表示不选择任何集群事件。

标识取决于集群事件过滤器的资源。

#### Bundle

对于 bundle 集群事件，标识符是 bundle 的位置。你可以使用 `osgi:list -l` 命令获取 bundle 的位置，它将给出以下输出：

```
...

[ 61] [Active      ] [Created  ] [  40]
mvn:org.apache.karaf.cellar/org.apache.karaf.cellar.features/2.3.2

[ 62] [Active      ] [Created  ] [  40]
mvn:org.apache.karaf.cellar/org.apache.karaf.cellar.management/2.3.2

[ 70] [Active      ] [Created  ] [  30]
mvn:org.apache.felix/org.apache.felix.eventadmin/1.3.2
```

例如，如果你想阻止（过滤）`my` 集群组中 eventadmin bundle 的所有 bundle 集群事件，入站和出站，你可以在 `etc/org.apache.karaf.cellar.groups.cfg` 配置文件中指定以下过滤器：

```
my.bundle.blacklist.inbound=mvn:org.apache.felix/org.apache.felix.eventadmin/1.3.2

my.bundle.blacklist.outbound=mvn:org.apache.felix/org.apache.felix.eventadmin/1.3.2
```

这就是 regex 非常有用的地方。我们可以简化过滤器如下：

```
my.bundle.blacklist.inbound=*org.apache.felix.eventadmin*

my.bundle.blacklist.outbound=*org.apache.felix.eventadmin*
```

#### Configuration

对于配置集群事件，标识符是配置 PID。你可以使用 config:list 命令获取配置 PID。

默认情况下，Cellar 会将自己的配置列入黑名单，并避免同步仅对节点本地有效的配置。以下是在 `etc/org.apache.karaf.cellar.groups.cfg` 配置文件中此配置的外观：

```
default.config.whitelist.inbound = *
default.config.whitelist.outbound = *
default.config.blacklist.inbound = org.apache.felix.fileinstall*,\
                            org.apache.karaf.cellar*,\
                            org.apache.karaf.management,\
                            org.apache.karaf.shell,\
                            org.ops4j.pax.logging,\
                            org.ops4j.pax.web
default.config.blacklist.outbound = org.apache.felix.fileinstall*,\
                            org.apache.karaf.cellar*,\
                            org.apache.karaf.management,\
                            org.apache.karaf.shell,\
                            org.ops4j.pax.logging,\
                            org.ops4j.pax.web
```

以下是前面代码片段中配置的解释：

- org.apache.felix.fileinstall* 配置被排除（入站和出站），因为轮询目录是节点本地的（因此将此配置传播到默认集群组是没有意义的）
- org.apache.karaf.cellar* 配置被排除，因为它们由 Cellar 本身提供
- org.apache.karaf.management 配置被排除，因为它包含 Karaf JMX MBean 服务器的端口号（因此是节点本地的）
- org.apache.karaf.shell 配置被排除，因为它包含 Karaf SSHd 服务器的端口号（因此是节点本地的）
- org.ops4j.pax.logging 配置被排除，以便能够更改节点的日志级别而不更改其他节点的日志级别
- org.ops4j.pax.web 配置被排除，因为它包含 Karaf HTTP 服务的端口号（因此是节点本地的）





## Features

对于特性集群事件，标识符是特性的名称。您可以使用 `features:list` 命令获取特性名称，该命令会给出以下输出：

```
...
[uninstalled] [2.3.2          ] cellar-webconsole
    karaf-cellar-2.3.2  Cellar plugin for Karaf WebConsole
[uninstalled] [1.0.1          ] transaction
    karaf-enterprise-2.3.1 OSGi Transaction Manager
[uninstalled] [1.0.1          ] jpa
    karaf-enterprise-2.3.1 OSGi Persistence Container
[uninstalled] [1.0.0          ] jndi
    karaf-enterprise-2.3.1 OSGi Service Registry JNDI access
[uninstalled] [1.0.0          ] application-without-isolation
    karaf-enterprise-2.3.1 Provide EBA archive support
```

默认情况下，Cellar 按如下方式黑名单某些特性：

```
default.features.whitelist.inbound = *
default.features.whitelist.outbound = *
default.features.blacklist.inbound = config,management,hazelcast,cellar*
default.features.blacklist.outbound = config,management,hazelcast,cellar*
```

Hazelcast 和 Cellar 特性是由 Cellar 本身安装的；因此，无需处理这些特性的集群事件。这就是默认的过滤配置允许所有集群特性事件，除了 config、management、hazelcast 以及所有 Cellar（cellar*）特性。

另一方面，config 和 management 特性默认由 Karaf 提供。由于这些是非常底层的特性，因此不需要处理这些特性的事件。

## The default filter configuration

Cellar 使用以下默认过滤配置：

```
default.config.whitelist.inbound = *
default.config.whitelist.outbound = *
default.config.blacklist.inbound = org.apache.felix.fileinstall*, \
    org.apache.karaf.cellar*, \
    org.apache.karaf.management, \
    org.apache.karaf.shell, \
    org.ops4j.pax.logging, \
    org.ops4j.pax.web
default.config.blacklist.outbound = org.apache.felix.fileinstall*, \
    org.apache.karaf.cellar*, \
    org.apache.karaf.management, \
    org.apache.karaf.shell, \
    org.ops4j.pax.logging, \
    org.ops4j.pax.web

default.features.whitelist.inbound = *
default.features.whitelist.outbound = *
default.features.blacklist.inbound = config,management,hazelcast,cellar*
default.features.blacklist.outbound = config,management,hazelcast,cellar*

default.bundle.whitelist.inbound = *
default.bundle.whitelist.outbound = *
default.bundle.blacklist.inbound = none
default.bundle.blacklist.outbound = none
```

以下是前述配置的说明：

- 所有集群配置事件在默认集群组中均被允许（白名单），对节点而言为入站和出站，除了（黑名单）具有 org.apache.felix.fileinstall*、org.apache.karaf.cellar*、org.apache.karaf.management、org.apache.karaf.shell、org.ops4j.pax.logging 和 org.ops4j.pax.web PID 的配置。
- 所有集群特性事件在默认集群组中均被允许（白名单），对节点而言为入站和出站，除了（黑名单）名为 config、management、hazelcast 和 cellar* 的特性。
- 所有集群 bundle 事件在默认集群组中均被允许（白名单），对节点而言为入站和出站。没有 bundle 被阻止（黑名单为 none）。

## Summary

在本章中，我们学习了如何配置集群事件的过滤。它提供了对集群事件的细粒度管理：按集群组、按方向（入站或出站）、按资源（特性、bundle 和 config），支持使用正则表达式来识别集群事件和过滤类型（白名单或黑名单）。

它还允许集群管理员定义哪些资源应在集群上同步。

在下一章中，我们将看到资源同步的另一个应用：使用 **Distributed OSGi (DOSGi)** 的 bundle 之间远程通信。
