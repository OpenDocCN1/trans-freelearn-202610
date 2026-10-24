# 3 Hazelcast

Hazelcast 是 Cellar 的核心，并在核心模块中使用。Cellar 核心模块指定了集群行为，但未指定实现。默认的 Cellar 实现由 Hazelcast 提供支持。Cellar 直接利用 Hazelcast 的功能来提供集群中预期的功能，例如发现集群节点、节点之间的通信以及在集群中分发资源的状态。

在本章中，我们将讨论以下主题：

- 对 Hazelcast 及其功能的介绍
- 理解 Cellar 如何使用 Hazelcast
- 为 Hazelcast 创建自定义扩展

### What is Hazelcast?

Hazelcast 是一个集群和分布式解决方案。在 Cellar 中，每个节点都嵌入了一个 Hazelcast 实例。Hazelcast 实例可以使用不同的机制相互识别。一旦连接，这些 Hazelcast 实例会共享状态并在集群中分发对象，如下所示：

- 分布式集合和映射的形式，允许 Cellar 在集群中存储资源的状态。
- Cellar 用于将集群事件从一个节点传输到其他节点的分布式主题和队列。它类似于我们在 JMS 等消息系统中找到的主题和队列目标——一个节点可以作为队列的生产者/消费者和/或作为主题的发布者/订阅者。
- 多种网络配置和发现机制，允许 Cellar 节点自动发现并看到其他节点。

Cellar 以 OSGi 的方式封装 Hazelcast 实例；这意味着 Hazelcast 实例作为 OSGi 服务公开，可以被任何客户端 bundle 使用。对于节点之间的通信，Cellar 使用 Hazelcast TCP/IP 层与 Java NIO。

Cellar 会自动在 `etc/hazelcast.xml` 中安装一个 Hazelcast 配置文件。Hazelcast 配置和调优存储在此文件中。本章涵盖此配置文件的內容。

### Distributed cluster resource states

Cellar 使用 Hazelcast 来维护和存储集群中不同资源的状态（bundle、features、配置、OBR、eventadmin、DOSGi 等）。为此，Cellar 使用 Hazelcast 的分布式数据结构。借助此功能，集群数据分布在集群中的所有节点上。每个 Cellar 节点存储数据的一部分（以及最终的副本/备份）。

例如，在 Cellar 中，你安装了以下三个功能：

- Feature A
- Feature B
- Feature C

功能的状态存储在一个分布式映射中，看起来类似于以下状态：

- Feature A: Installed
- Feature B: Uninstalled
- Feature C: Installed

功能分布式映射在集群中的分区可能如下所示：

- Feature A 的状态在 node1 上
- Feature B 的状态在 node2 上
- Feature C 的状态在 node1 上

如果某个节点宕机或离开集群，Hazelcast 会在剩余节点上重新分发数据（包括副本）。同样，当新节点加入集群时，Hazelcast 会在不同节点上重新分发数据。

因此，没有集群管理器或单点故障：集群中的每个节点的作用都与下一个节点相同。

### Distributed queues and topics

Cellar 使用分布式队列和主题进行节点间通信。

当某个节点上的资源发生变化时（例如，你在此节点上安装了新功能），Cellar 会创建一个集群事件并将其发送到队列或主题中（取决于集群事件是针对一个节点还是全局）。

另一方面，其他节点可以消费此集群事件并做出相应反应（它们将在本地安装该功能）。

Hazelcast 分布式队列是 `java.util.concurrent.BlockingQueue` 的实现。作为一个常规队列，队列中的元素按 FIFO 模式排序。同样，由于此队列分布在 Cellar 节点上，队列中的元素是实现 `java.io.Serializable` 的对象。默认情况下，队列中元素的数量没有限制，这些元素的生存时间（TTL）也没有限制。现在，如果集群节点之间连接不良（网络问题或不同的 WAN/数据中心），你可能会在队列中拥有非常多的元素（你入队速度快于出队速度）。在这种情况下，一个好的做法是在 `etc/hazelcast.xml` 中为队列定义一个限制。

每个 Cellar 实例的最大容量和队列的 TTL 可以在 `etc/hazelcast.xml` 文件中配置。例如，我们可以使用 `max-size-per-jvm` 元素限制队列的大小，使用 `map-store` 元素定义队列的自定义存储，如以下代码所示：

```
<hazelcast>
    ...
    <queue name="*">
        <!-- Maximum size of the queue. When a JVM's local queue size reaches the maximum, all put/offer operations will get blocked until the queue size of the JVM goes down below the maximum. Any integer between 0 and Integer.MAX_VALUE. 0 means Integer.MAX_VALUE. Default is 0. -->
        <max-size-per-jvm>10000</max-size-per-jvm>
        <!-- Name of the map configuration that will be used for the backing distributed map for this queue. -->
        <backing-map-ref>queue-map</backing-map-ref>
    </queue>
    <map name="queue-map">
        <backup-count>1</backup-count>
        <map-store enabled="true">
            <class-name>org.example.MyStore</class-name>
            <write-delay-seconds>0</write-delay-seconds>
        </map-store>
        ...
    </map>
</hazelcast>
```

默认情况下，Cellar 将资源状态存储在内存中分布式队列中（没有 `map-store` 配置）。你可以提供一个存储实现，将队列持久化到后端，例如数据库。

### The Cellar distributed map

Cellar 使用分布式映射来存储集群中资源的状态。为此，以下是实例：

- 集群中功能的列表和当前状态存储在如下所示的分布式映射中：
    - Feature A: Installed
    - Feature B: Uninstalled
- 集群中 bundle 的列表和当前状态存储在如下所示的分布式映射中：
    - Bundle A: Installed
    - Bundle B: Started
    - Bundle C: Uninstalled
- 集群上的配置和属性列表存储在如下所示的分布式映射中：
    - PID A: Properties
    - PID B: Properties

这些映射条目在集群节点上分区。这意味着每个节点都有映射的一部分。

### Replicas/Backup

默认情况下，分布式映射有一个副本以确保故障转移；当条目添加到映射或在映射中更新时，Hazelcast 会在另一个节点上同步创建副本。

Hazelcast 使用备份操作管理副本；它提供以下两种操作：

- Sync backup operations
- Async backup operations





Cellar 仅使用同步备份操作来确保在继续之前副本是完整的。您可以使用 `etc/hazelcast.xml` 文件中的 `backup-count` 元素来增加副本数量。

默认情况下，仅在节点宕机时才使用副本；Hazelcast 可以使用另一个节点上存在的副本重新加载失败节点上的数据。

然而，在实际运行环境中，节点必须调用存储数据的节点，即使它本地持有该数据的副本。

为了提高读取性能，您可以允许节点读取本地副本，而不是实际调用存储数据的节点。要做到这一点，您只需在 `etc/hazelcast.xml` 文件中启用 `read-backup-data` 即可：

```xml
<hazelcast>
    ...
    <map name="*">
        <!-- Number of sync-backups. If 1 is set as the backup-
             count for example, then all entries of the map will be
             copied to another JVM for fail-safety.
             Valid numbers are 0 (no backup), 1, 2, 3. -->
        <backup-count>1</backup-count>
        <!-- Can we read the local backup entries? Default value
             is false for strong consistency. Being able to read
             backup data will give you greater performance. -->
        <read-backup-data>false</read-backup-data>
        ...
    </map>
</hazelcast>
```

## Persistence

正如我们在上一节中所看到的，Cellar 默认使用内存分布式映射。这没什么大不了的，因为每个节点在启动时都会重建数据。然而，出于性能原因以及数据安全的原因，您可以通过在 `etc/hazelcast.xml` 文件中使用以下代码描述持久化数据存储，为 Cellar 使用的映射实现持久化数据存储：

```xml
<hazelcast>
    ...
    <map name="*">
        ...
        <map-store enabled="true">
            <!-- Name of the class implementing MapLoader and/or
                 MapStore. The class should implement at least one of
                 these interfaces and contain no-argument
                 constructor. Note that the inner classes are not
                 supported. -->
            <class-name>com.hazelcast.examples.DummyStore</class-name>
            <!-- Number of seconds to delay to call the
                 MapStore.store(key, value). If the value is zero
                 then it is write-through so
                 MapStore.store(key, value) will be called as soon as
                 the entry is updated. Otherwise it is write-behind
                 so updates will be stored after write-delay-seconds
                 value by calling MapStore.storeAll(map).
                 Default value is 0. -->
            <write-delay-seconds>0</write-delay-seconds>
        </map-store>
    </map>
</hazelcast>
```

## Networks

在前面的章节中，我们看到了 Cellar 用于格式化和传输集群事件的 Hazelcast 资源。传输层中继到网络（线路）。Hazelcast 提供了我们可以利用的网络配置。在本节中，我们将了解如何配置 Cellar 节点发现，如何保护节点之间的通信，以及如何支持 IPv6。

### Multiple clusters

在同一网络上，您可能希望定义多个 Cellar 集群，尤其是在使用节点发现时。Cellar 集群在 `etc/hazelcast.xml` 配置文件中通过名称和密码标识。您可以使用以下代码以简单的方式分隔集群（例如按环境）：

```xml
<hazelcast>
    <group>
        <name>dev</name>
        <password>dev-pass</password>
    </group>
    ...
</hazelcast>
```

### TCP/IP

如果多播（默认）不是发现节点首选的方式，那么您可以配置完整的 TCP/IP 集群（也称为 Cellar 节点的静态定义）。

在以下 `etc/hazelcast.xml` 文件中，我们将禁用多播并优先使用 `tcp-ip` 发现。

为此，我们需要提供托管不同节点的机器的主机名或 IP 地址列表：

```xml
<hazelcast>
    ...
    <network>
        <port auto-increment="true">5701</port>
        <join>
            <multicast enabled="false">
                <multicast-group>224.2.2.3</multicast-group>
                <multicast-port>54327</multicast-port>
            </multicast>
            <tcp-ip enabled="true">
                <hostname>machine1</hostname>
                <hostname>machine2</hostname>
                <hostname>machine3:5799</hostname>
                <interface>192.168.1.0-7</interface>
                <interface>192.168.1.21</interface>
            </tcp-ip>
        </join>
        ...
    </network>
    ...
</hazelcast>
```

### Interfaces

默认情况下，Cellar 使用机器的任何网络接口（`0.0.0.0`）。然而，在服务器机器上，拥有多个网络接口是很常见的。在这种情况下，您可以指定要用于 Cellar 的接口；这可以通过以下代码完成：

```xml
<hazelcast>
    ...
    <network>
        ...
        <interfaces enabled="true">
            <interface>10.3.16.*</interface>
            <interface>10.3.10.4-18</interface>
            <interface>192.168.1.3</interface>
        </interfaces>
    </network>
    ...
</hazelcast>
```

该配置允许使用 * 和 - 字符。

* 字符表示网掩码上的所有 IP 地址；在我们的示例中，10.3.16.* 表示 10.3.16.1、10.3.16.2、10.3.16.3 等。

- 字符表示 IP 地址范围；在我们的示例中，10.3.10.4-18 表示 10.3.10.4、10.3.10.5、10.3.10.6 等，直到 10.3.10.18。

### SSL

Cellar 允许您在所有 Cellar 成员之间使用 SSL Socket 通信。

使用 SSL，节点之间的通信通道是安全的。

在 `etc/hazelcast.xml` 中，您只需启用 `ssl` 并提供包含 SSL 证书的密钥库，如以下代码所示：

```xml
<hazelcast>
    ...
    <network>
        ...
        <ssl enabled="true">
            <factory-class-name>
                com.hazelcast.nio.ssl.BasicSSLContextFactory
            </factory-class-name>
            <properties>
                <property name="keyStore">keyStore</property>
                <property name="keyStorePassword">keyStorePassword</property>
            </properties>
        </ssl>
    </network>
    ...
</hazelcast>
```

您必须提供一个 SSLFactory 类。Hazelcast 提供了一个您可以直接使用的 BasicSSLContextFactory 类，或者您可以指定自己的 SSLFactory。

### Encryption

除了 SSL（传输层加密）之外，您还可以启用集群事件内容的加密。您可以使用非对称加密（带公钥/私钥对）或对称加密（带单个密钥）。您可以在 `etc/hazelcast.xml` 文件中的网络元素级别配置此项。

以下示例展示了如何使用 Hazelcast 为您生成的密钥进行对称加密：

```xml
<hazelcast>
    ...
    <network>
        ...
        <!-- Make sure to set enabled=true
             Make sure this configuration is exactly the same on
             all members -->
        <symmetric-encryption enabled="true">
            <!--
                encryption algorithm such as
                DES/ECB/PKCS5Padding,
                PBEWithMD5AndDES,
                Blowfish,
                DESede
            -->
            <algorithm>PBEWithMD5AndDES</algorithm>
            <!-- salt value to use when generating the secret key
            -->
            <salt>thesalt</salt>
            <!-- pass phrase to use when generating the secret key
            -->
            <password>thepass</password>
            <!-- iteration count to use when generating the secret
            key -->
            <iteration-count>19</iteration-count>
        </symmetric-encryption>
    </network>
    ...
</hazelcast>
```

对于非对称加密，您需要使用 keytool 或 OpenSSL 创建私钥/公钥对。

密钥必须存储在 `etc/hazelcast.xml` 文件中使用的密钥库中：

```xml
<hazelcast>
    ...
    <network>
        ...
        <!--
            Make sure to set enabled=true
        -->
        <asymmetric-encryption enabled="true">
            <!-- encryption algorithm -->
            <algorithm>RSA/NONE/PKCS1PADDING</algorithm>
            <!-- private key password -->
            <keyPassword>thekeypass</keyPassword>
            <!-- private key alias -->
            <keyAlias>member1</keyAlias>
            <!-- key store type -->
            <storeType>JKS</storeType>
            <!-- key store password -->
            <storePassword>thestorepass</storePassword>
            <!-- path to the key store -->
            <storePath>keystore</storePath>
        </asymmetric-encryption>
    </network>
    ...
</hazelcast>
```

### IPv6 support

您可以在 `etc/hazelcast.xml` 文件中使用 IPv6 地址格式，在使用 IPv4 格式的确切位置直接使用 IPv6 格式。这可以通过以下代码完成：

```xml
<hazelcast>
    ...
    <network>
        <port auto-increment="true">5701</port>
        <join>
            <multicast enabled="false">
                <multicast-group>FF02:0:0:0:0:0:0:1
                </multicast-group>
                <multicast-port>54327</multicast-port>
            </multicast>
            <tcp-ip enabled="true">
```




```xml
<member>[fe80::223:6cff:fe93:7c7e]:5701</member>
<interface>192.168.1.0-7</interface>
<interface>192.168.1.*</interface>
<interface>fe80:0:0:0:45c5:47ee:fe15:493a
</interface>
</tcp-ip>
</join>
<interfaces enabled="true">
    <interface>10.3.16.*</interface>
    <interface>10.3.10.4-18</interface>
    <interface>fe80::0:0:0:45c5:47ee:fe15:*</interface>
    <interface>fe80::223:6cff:fe93:0-5555</interface>
</interfaces>
...
</network>
```

你可以通过 JVM 系统属性强制指定 IP 地址的格式（当你的机器同时支持两者时）：

- `java.net.preferIPv4Stack=<true|false>`
- `java.net.preferIPv6Addresses=<true|false>`

## 限制出站端口

默认情况下，节点之间的通信使用建立连接时创建的随机端口号。根据网络安全策略（防火墙、PAT、NAT 等）的不同，这有时可能会成为一个问题。你可以不采用随机端口，而是定义一个端口范围或端口列表，用于节点之间的通信：

```xml
<hazelcast>
    ...
    <network>
        <port auto-increment="true">5701</port>
        <outbound-ports>
            <ports>33000-35000</ports> <!-- ports between 33000 and 35000 -->
            <ports>37000,37001,37002,37003</ports> <!-- comma separated ports -->
            <ports>38000,38500-38600</ports>
        </outbound-ports>
        ...
    </network>
    ...
</hazelcast>
```

端口可以指定为一个范围（33000–35000 表示该范围内的端口会被使用）、一系列以逗号分隔的端口（例如 38000 等），或者两者的混合形式。

## 总结

在本章中，我们学习了 Cellar 和 Hazelcast 如何协同工作。我们了解了队列/主题用于节点间的通信，以及映射/集合用于在集群上存储资源。我们还了解了如何调整 Cellar 组件以添加持久化、限制元素大小以及执行其他任务。

我们还学习了如何扩展默认的 Cellar Hazelcast 配置，以满足一些额外需求，例如安全性。

下一章将向你介绍集群组，它允许你根据节点名称对节点进行分组。
