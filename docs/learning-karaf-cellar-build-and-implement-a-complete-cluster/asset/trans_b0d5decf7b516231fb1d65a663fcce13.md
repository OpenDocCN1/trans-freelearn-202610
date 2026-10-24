

我们的两个节点已同步。commons-lang 捆绑包已安装到两个节点上。

现在，我们将启动 commons-lang 捆绑包。不过这次我们不使用 `cluster:bundle-start` 命令，而是在节点 1 上使用本地的 `osgi:start` 命令。由于我们刚刚安装了这个捆绑包，它尚未启动，因此我们需要现在启动它。执行以下命令：

```
karaf@node1> osgi:start 63
```

现在，运行以下命令，我们可以看到 commons-lang 捆绑包已在节点 1 上启动：

```
karaf@node1> la -l|grep -i commons-lang
[ 63] [Active ] [       ] [  80] mvn:commons-lang/commons-lang/2.6
```

得益于 Cellar 捆绑包监听器，这个本地变更已被 Cellar 捕获并广播到其他节点。因此，这意味着 commons-lang 捆绑包现在也应该在节点 2 上启动了。运行以下命令：

```
karaf@node2> la -l|grep -i commons-lang
[ 63] [Active ] [       ] [  80] mvn:commons-lang/commons-lang/2.6
```

再次回到这里，我们的两个节点仍然保持同步。

总而言之，Apache Karaf Cellar 支持你对捆绑包执行的所有操作（安装、启动、停止、卸载、刷新等）。

默认情况下，得益于 Cellar 捆绑包监听器，所有本地操作都会传播到整个集群。

你也可以使用专用的 `cluster:bundle-*` 命令。

## Apache Karaf Cellar

除了 `cluster:bundle-*` 命令之外，你还可以通过 JMX 使用 Cellar BundleMBean。这个 MBean 允许你使用任意 JMX 客户端（如 JConsole）操作集群上的捆绑包，如以下截图所示：

![](img/86f349f6a758f2c3088dbeb4d35dcaca_42_0.png)

## Karaf 特性

Apache Karaf Cellar 还能够同步 Karaf 特性。

Cellar 特性监听器会将所有本地操作传播到集群上的某个特性，就像捆绑包一样。

你还可以使用 `cluster:feature-*` shell 命令来操作集群上的 Karaf 特性。

例如，可以通过以下命令查看集群上的 Karaf 特性仓库 URL：

```
karaf@node1> cluster:feature-url-list default
mvn:org.apache.karaf.cellar/apache-karaf-cellar/2.3.1/xml/features
mvn:org.jclouds.karaf/jclouds-karaf/1.4.0/xml/features
mvn:org.apache.karaf.assemblies.features/enterprise/2.3.3/xml/features
mvn:org.apache.karaf.assemblies.features/standard/2.3.3/xml/features
```

特性状态可以通过以下命令检查：

```
karaf@node1> cluster:feature-list default|more
Features in cluster group default
Status      Version      Name
[uninstalled] [3.0.7.RELEASE ] spring
[uninstalled] [2.3.1       ] cellar-eventadmin
[uninstalled] [2.3.3       ] scr
[uninstalled] [1.4.0       ] jclouds-cloudfiles-uk
[uninstalled] [1.4.0       ] jclouds-services
[uninstalled] [1.4.0       ] jclouds-aws-s3
[uninstalled] [1.4.0       ] jclouds-cloudserver-us
```

请注意，我们可以通过以下方式查看 eventadmin 特性的本地状态和集群状态：

```
karaf@node1> features:list|grep -i eventadmin
[uninstalled] [2.3.3       ] eventadmin
karaf-2.3.3      OSGi Event Admin service specification for event-based communication
karaf@node1> cluster:feature-list default|grep -i eventadmin
[uninstalled] [2.3.3       ] eventadmin
```

现在我们想使用 `cluster:feature-install` 命令在集群上安装 eventadmin 特性。我们将通过以下命令来执行：

```
karaf@node1> cluster:feature-install default eventadmin
```

我们可以通过以下方式检查集群上 eventadmin 特性的状态：

```
karaf@node1> cluster:feature-list default|grep -i eventadmin
[installed  ] [2.3.3       ] eventadmin
```

现在我们可以看到 eventadmin 特性已安装。让我们用以下命令检查节点 1 和节点 2 上本地是否也是如此：

```
karaf@node1> features:list|grep -i eventadmin
[installed  ] [2.3.3      ] eventadmin                 karaf-2.3.3
OSGi Event Admin service specification for event-based communication
```

```
karaf@node2> features:list|grep -i eventadmin
[installed  ] [2.3.3      ] eventadmin                 karaf-2.3.3
OSGi Event Admin service specification for event-based communication
```

我们的两个节点已同步，eventadmin 特性已安装到两个节点上。

现在我们想卸载 eventadmin 特性，但这次我们不使用专用的 `cluster:feature-uninstall` 命令，而是在节点 1 上使用本地的 `features:uninstall` 命令，如下所示：

```
karaf@node1> features:uninstall eventadmin
```

得益于 Cellar 特性监听器，这个本地变更会传播到整个集群。我们可以通过以下方式检查集群上 eventadmin 特性的状态：

```
karaf@node1> cluster:feature-list default|grep -i eventadmin
[uninstalled] [2.3.3      ] eventadmin
```

并在节点 2 上本地检查如下：

```
karaf@node2> features:list|grep -i eventadmin
[uninstalled] [2.3.3      ] eventadmin
karaf-2.3.3        OSGi Event Admin service specification for event-based communication
```

正如我们所见，节点 1 和节点 2 仍然保持同步。Apache Karaf Cellar 提供了一个 Cellar FeatureMBean，允许你使用任意 JMX 客户端（如 JConsole）执行集群操作，就像对捆绑包的操作一样，如以下截图所示：

![](img/86f349f6a758f2c3088dbeb4d35dcaca_45_0.png)

## 配置

除了捆绑包和特性之外，Apache Karaf Cellar 还支持配置的同步和分发。

你可以使用 `cluster:config-list` 命令查看集群上的配置列表，如下所示：

```
karaf@node1> cluster:config-list default
------------------------------------------------------------
Pid:          org.apache.karaf.features.repos
Properties:
    service.pid = org.apache.karaf.features.repos
    openejb = org.apache.openejb:openejb-feature:xml:features: (0,]
```

```
jclouds = org.jclouds.karaf:jclouds-karaf:xml:features:(0,]
camel = org.apache.camel.karaf:apache-camel:xml:features:(0,]
cxf-dosgi = org.apache.cxf.dosgi:cxf-dosgi:xml:features:(0,]
wicket = org.ops4j.pax.wicket:features:xml:features:(0,]
```

我们通过以下命令行创建包含关键属性的 my 配置：

```
karaf@node1> cluster:config-propset default my key value
```

并使用以下命令查看集群上的配置：

```
karaf@node1> cluster:config-list default
...
Pid:        my
Properties:
    key = value
    service.pid = my
```

我们可以通过以下命令检查 Cellar 是否已在本地为节点 1 和节点 2 创建此配置：

```
karaf@node1> config:list "(service.pid=my)"
------------------------------------------------------------
Pid:        my
BundleLocation: null
Properties:
    service.pid = my
    key = value
```

```
karaf@node2> config:list "(service.pid=my)"
------------------------------------------------------------
Pid:        my
BundleLocation: null
Properties:
    service.pid = my
    key = value
```

再次，两个节点都处于同步状态。my 配置已复制到节点 1 和节点 2。

现在我们想把 key 属性的值改为 other。但这次我们不使用 `cluster:config-propset` 命令，而是直接在节点 1 上使用本地 Karaf 原生的 `config:*` 命令，如下所示：

```
karaf@node1> config:edit my
karaf@node1> config:propset key other
karaf@node1> config:update
```

得益于 Cellar 配置监听器，my 配置的状态已在集群上更新：

```
karaf@node1> cluster:config-list default
...
Pid:          my
Properties:
    key = other
    service.pid = my
```

我们可以检查 “my” 配置是否在节点 2 上也已更新：

```
karaf@node2> config:list "(service.pid=my)"
-------------------------------------------------
Pid:          my
BundleLocation: null
Properties:
    service.pid = my
    key = other
```

再次，两个节点都处于同步状态。

## Apache Karaf Cellar

Apache Karaf Cellar 提供了一个 Cellar ConfigMBean，允许你使用任意 JMX 客户端（如 JConsole）操作集群，就像对捆绑包和特性一样，如以下截图所示：

![](img/86f349f6a758f2c3088dbeb4d35dcaca_48_0.png)

## 可选资源

除了默认的捆绑包、特性和配置资源支持之外，Apache Karaf Cellar 还提供了一些可选资源支持。

要启用对这些资源的支持，你必须安装以下专用的 Cellar 特性：

- `cellar-eventadmin` 特性，为 eventadmin 特性添加集群支持。这意味着某个节点上的所有本地 OSGi 事件都会广播到其他节点。

