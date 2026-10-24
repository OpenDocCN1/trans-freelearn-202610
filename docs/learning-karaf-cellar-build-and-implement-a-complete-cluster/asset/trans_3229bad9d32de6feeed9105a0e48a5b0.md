

## Cellar 与 Camel

- vm 端点与 seda 完全相同，但它是 JVM 全局的。这意味着来自不同 CamelContext 的路由可以使用 vm 进行通信。

这些组件便于实现 Camel 路由之间的通信。然而，这种通信仅限于同一 JVM 内部。

当我们部署一个由多个 Karaf 实例组成的集群或农场时，部署能够与其他实例上的路由进行通信的 Camel 路由就变得很有意义。借助这一点，我们可以让某个实例专门化，并将部分路由执行任务委托给其他实例。

此外，由于 Cellar 是一个动态集群（在集群中添加新节点很容易），利用多个节点来执行路由是合理的。这样，我们就可以实现一个动态且可扩展的 Camel 路由集群。

实现此类解决方案的一种方法是使用 **Java 消息服务（JMS）**。借助代理（如 Apache ActiveMQ 或任何 JMS 代理），路由可以在代理的队列中生成消息，而其他路由可以从该队列消费消息。这种方法很有吸引力，因为我们可以利用代理提供的特性（消息持久化、高可用性等），但这需要使用额外的中间件资源，即代理。这意味着我们必须使用 Cellar 来供应集群并维护代理。

得益于 Cellar 和带 SEDA 的 Camel Hazelcast 组件，我们可以在没有 JMS 代理的情况下实现类似的功能。无需添加额外的中间件（JMS 代理），从而省去了安装、管理、维护和调优等工作。

然而，JMS 代理（尤其是 ActiveMQ）提供的某些功能非常有用，例如主/从模式、代理网络和持久化存储。

在下图中，我们可以看到 Camel 路由 A 使用 Cellar 在 **NodeA** 上提供的 Hazelcast 实例（得益于 `camel-hazelcast` 组件）。这个 Camel 路由 A 在 Hazelcast 队列中生成一条消息。另一方面，Camel 路由 B 同样借助 `camel-hazelcast` 组件消费 Hazelcast 队列中的消息：

![](img/86f349f6a758f2c3088dbeb4d35dcaca_100_0.png)

在第一个节点（**NodeA**）上，我们按如下方式安装 Cellar、Camel（`camel-blueprint`）和 `camel-hazelcast` 组件：

```
karaf@nodeA> features:addurl
mvn:org.apache.karaf.cellar/apache-karaf-cellar/2.3.0/xml/features

karaf@nodeA> features:install cellar

karaf@nodeA> features:addurl
mvn:org.apache.camel.karaf/apache-camel/2.12.0/xml/features

karaf@nodeA> features:install camel-blueprint

karaf@nodeA> features:install camel-hazelcast
```

该节点现在已准备好部署 Camel 路由。

我们使用 Camel Blueprint DSL 创建第一条路由。为此，我们创建 `route.xml` 文件，内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="
           http://www.osgi.org/xmlns/blueprint/v1.0.0 http://www.osgi.org/xmlns/blueprint/v1.0.0/blueprint.xsd">

    <camelContext xmlns="http://camel.apache.org/schema/blueprint">

        <route id="routeA">
            <from uri="timer:simple?period=5000"/>
            <setBody>
                <simple>Hello World</simple>
            </setBody>
            <to uri="hazelcast:seda:myqueue"/>
        </route>

    </camelContext>

</blueprint>
```

这条路由非常简单。每 5 秒，它创建一个 exchange，将消息体设置为 `Hello World`，并发送到名为 myqueue 的 Hazelcast SEDA 队列。

消息将存储在 Hazelcast 队列中，等待消费者从该队列获取消息。这看起来很像 JMS，但队列分布在所有 Cellar 节点之间。

要部署此路由，我们只需将 `route.xml` 文件放入 **NodeA** 的 `deploy` 文件夹即可。

因此，现在我们有了一个在 Cellar/Hazelcast 队列中生成消息的节点。

在 **NodeB** 上，我们还将安装 Cellar、Camel（`camel-blueprint`）、`camel-stream` 组件（用于直接在 `System.out` 上显示消息）以及 `camel-hazelcast` 组件，操作如下：

```
karaf@nodeB> features:addurl
mvn:org.apache.karaf.cellar/apache-karaf-cellar/2.3.0/xml/features

karaf@nodeB> features:install cellar

karaf@nodeB> features:addurl
mvn:org.apache.camel.karaf/apache-camel/2.12.0/xml/features

karaf@nodeB> features:install camel-blueprint

karaf@nodeB> features:install camel-hazelcast

karaf@nodeB> features:install camel-stream
```

在 **NodeB** 上，我们创建一条 Camel 路由，用于消费 Cellar/Hazelcast 队列中的消息。

我们使用 Camel Blueprint DSL 在 `route.xml` 文件中定义此路由，并将其放入（**NodeB** 的）`deploy` 文件夹，内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
    http://www.osgi.org/xmlns/blueprint/v1.0.0 http://www.osgi.org/xmlns/blueprint/v1.0.0/blueprint.xsd">

    <camelContext xmlns="http://camel.apache.org/schema/blueprint">

        <route id="routeB">
            <from uri="hazelcast:seda:myqueue"/>
            <to uri="stream:out"/>
        </route>

    </camelContext>

</blueprint>
```

一旦此路由启动，我们就可以在 **NodeB** 的 `System.out` 中看到以下消息（因为我们使用了 `stream:out`，消息会直接显示在 shell 控制台上）：

```
Hello World
Hello World
Hello World
Hello World
```

这意味着部署在 **NodeB** 上的 `routeB` 正在消费来自 `myqueue` 分布式队列的消息。由于该队列是 Cellar/Hazelcast 分布式队列，实际数据并不局限于某个节点，而是分布在集群的所有节点上。

我们也可以将 `routeB` 部署在多个节点上（例如，使用特定的集群组配合 `cluster:feature-install` 命令）。在这种情况下，同一个队列将会有多个消费者，从而提供非常高效的可扩展解决方案。

结合 Cellar 的节点发现机制（尤其是使用组播/单播），我们可以非常轻松地在集群中添加新节点，这些节点会直接成为消费者或生产者（取决于我们部署的路由）。

得益于 `camel-hazelcast` 组件和 Cellar 提供的 Hazelcast SEDA 端点，您可以轻松为 Camel 路由提供远程通信解决方案。

如果我们将 Hazelcast SEDA 与 JMS 端点进行比较，会发现它们非常相似。JMS 可能更健壮，因为它利用了代理提供的所有特性（持久化存储、管道订阅、独占消费者等）。然而，对于简单的远程路由通信，且无需添加 JMS 代理等额外中间件的情况下，Hazelcast SEDA 是一个很好的解决方案。

## 使用分布式映射进行缓存

Cellar 与 `camel-hazelcast` 结合的另一个用例是实现分布式缓存解决方案。

`camel-hazelcast` 组件允许您使用分布式映射，并在此映射上执行各种操作。

由于它是分布式的，这意味着数据分布在不同的节点之间：所有节点都可以从分布式映射中看到相同的数据（条目/值）。这意味着您可以在节点之间共享数据。

## Cellar 与 Camel

在 Camel 路由中，您可以对分布式映射执行以下操作：

- `put`：向映射中添加新数据
- `delete`：从映射中删除数据
- `get`：从映射中检索所有数据
- `query`：从映射中检索特定数据

操作类型在 `CamelHazelcastOperationType` 头部中定义。

例如，我们可以使用以下代码实现一个更新缓存的路由：

```
```xml
<route>
    <from uri="direct-vm:add-in-cache"/>
    <setHeader headerName="CamelHazelcastOperationType">
        <constant>put</constant>
    </setHeader>
    <to uri="hazelcast:map:mycache"/>
</route>
```
```

此路由将在 direct-vm 端点上接收一条消息，该消息包含 `CamelHazelcastObjectId` 头部（值为 ID）以及要存储的数据（作为消息体）。

`hazelcast:map` 端点将使用 ID/消息体作为条目更新 mycache 映射。另一方面，路由可以从缓存中获取数据（如果提供了 ID 则获取单个数据集，如果未提供 ID 则获取所有数据），如下所示：

```
```xml
<route>
    <from uri="direct-vm:retrieve-from-cache"/>
    <setHeader headerName="CamelHazelcastOperationType">
        <constant>get</constant>
    </setHeader>
    <to uri="hazelcast:map:mycache"/>
</route>
```
```

除了在需要时请求缓存（通过 get 或 query 操作），还可以在缓存内容发生变化时立即触发一条路由。这样，我们就可以对数据变化做出反应（这是一种变更数据捕获）。为此，我们可以创建一条监听指定分布式映射的路由，如下所示：

```
```xml
<route>
    <from uri="hazelcast:map:mycache"/>
    ...
</route>
```
```

