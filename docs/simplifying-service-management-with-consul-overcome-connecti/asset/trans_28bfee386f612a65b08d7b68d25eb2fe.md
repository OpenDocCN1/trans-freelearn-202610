

如果我们想要将服务注册到特定命名空间（不是默认命名空间），只需在服务自身的配置中指定所需的命名空间即可。在之前的章节中，我们观察到了一个由 Consul 管理的 httpd 服务。我稍微修改了那个 JSON 文件，只是为了表明该服务将在 gus 命名空间中运行。

## httpd.json 服务定义

```
{
    "service": {
        "name": "httpd",
        "namespace": "gus",
        "tags": [
            "python-server"
        ],
        "port": 8080,
        "check": {
            "args": [
                "curl",
                "localhost:8080"
            ],
            "interval": "10s"
        }
    }
}
```

当我们像配置 httpd 服务一样，将服务配置并指定到特定命名空间后，只有该命名空间的成员才能与该服务交互并获取与其关联的信息。接着上面的例子，如果我查询与我的 Consul 系统关联的服务，最终只会得到一个服务：

```
$ consul catalog services
consul
```

哦，不！我的 `httpd` 服务难道没有在运行吗？我可以向你保证，该服务正在运行，只不过它位于一个独立且不同的命名空间中：

```bash
$ consul catalog services -namespace gus
httpd
```

现在，能够将这些服务分开会大有帮助；然而，如果我们没有以独特的方式保护这些命名空间，那么我们就没有真正从这些细分中获益。如果想到 ACL 工作时的经历让你神经紧张，那么你需要相信你的直觉。当我们开始在 Consul 系统中创建命名空间时，这有点像在构建一个管理层次结构。

如果你还记得，在用 ACL 引导系统时，我们需要创建一个引导 ACL 令牌，它赋予我们对整个 Consul 系统的完全管理权限。用一个极其简化的类比，我们可以把这个管理员或管理组看作是 Consul 系统的州长。我们称他为**勒佩托曼州长**。凭借以唯一令牌为代表的凭证，他能够为他的辖区制定规则，辖区内所有城镇和社区都必须遵守这些规则。不过，每个城镇仍然保持一定程度的自治。**约翰逊市长**拥有自己的凭证（令牌），能够为 Rock Ridge 镇制定自己的规章制度，只要这些规则符合勒佩托曼州长制定的规则。Consul 命名空间结构的管理方式几乎完全相同。

当我们在 Consul 中引导 ACL 功能时，我们会创建一个遵循默认全局管理策略的令牌。利用这个令牌，我们能够创建新的策略和相应的令牌，从而对 Consul 能做的所有事情进行非常细粒度的管理。当我们创建命名空间时，需要为这些操作员提供他们自己的管理策略，但不能让他们对相邻的命名空间行使权力。因此，每个命名空间都有自己的*操作员*级令牌，这些命名空间的管理员可以使用该令牌创建他们自己的策略、规则、服务等等。那么这两个默认策略有何不同？

> **重要说明**
>
> 命名空间允许我们对服务和策略进行细分；但是，Consul 集群节点对所有命名空间都是可见的。

让我们先从为 Shawn 的命名空间创建一个*命名空间管理级*策略开始：

```
consul acl token create -namespace shawn -description "Shawn's Management Token" -policy-name "namespace-management"
```

现在，我们有了一个分配给 shawn 命名空间的默认操作级令牌。利用 Consul CLI，我们可以观察到这两个策略的存在。请注意，为了看到*命名空间管理*策略，我需要包含相应命名空间的标志：

```
$ consul acl policy list
global-management:
  ID:             00000000-0000-0000-0000-000000000001
  Namespace:      default
  Description:    Builtin Policy that grants unlimited access
  Datacenters:

$ consul acl policy list -namespace=shawn
namespace-management:
  ID:             2226f072-68ae-9082-6d8d-7c9bdb1a987c
  Namespace:      shawn
  Description:    Default policy that grants all available privileges within a single namespace
  Datacenters:
```

利用 consul acl policy read 命令，我可以查看这两个策略的详细信息。同样，对于 shawn 命名空间，我需要在命令中包含命名空间标志。图 4.12 提供了策略差异的并排视图：

| global-management | namespace-management |
|---|---|
| <pre>ID: 00000000-0000-0000-0000-000000000001&#10;Name: global-management&#10;Namespace: default&#10;Description: Builtin Policy that grants unlimited access&#10;Datacenters:&#10;Rules:</pre> | <pre>ID: 2226f072-68ae-9082-6d8d-7c9bdb1a987c&#10;Name: namespace-management&#10;Namespace: shawn&#10;Description: Default policy that grants all available privileges within a single namespace&#10;Datacenters:</pre> |
| acl = "write" | acl = "write" |
| <pre>agent_prefix "" {&#10;  policy = "write"&#10;}&#10;event_prefix "" {&#10;  policy = "write"&#10;}</pre> | |
| <pre>key_prefix "" {&#10;  policy = "write"&#10;}</pre> | <pre>key_prefix "" {&#10;  policy = "write"&#10;}</pre> |
| keyring = "write" | |
| <pre>node_prefix "" {&#10;  policy = "write"&#10;}</pre> | <pre>node_prefix "" {&#10;  # node policy is restricted to read within a namespace&#10;  policy = "read"&#10;}</pre> |
| <pre>operator = "write"&#10;query_prefix "" {&#10;  policy = "write"&#10;}</pre> | |
| <pre>service_prefix "" {&#10;  policy = "write"&#10;  intentions = "write"&#10;}&#10;session_prefix "" {&#10;  policy = "write"&#10;}</pre> | <pre>service_prefix "" {&#10;  policy = "write"&#10;  intentions = "write"&#10;}&#10;session_prefix "" {&#10;  policy = "write"&#10;}</pre> |
| <pre>namespace_prefix "" {&#10;  acl = "write"&#10;  key_prefix "" {&#10;    policy = "write"&#10;  }&#10;  node_prefix "" {&#10;    policy = "read"&#10;  }&#10;  session_prefix "" {&#10;    policy = "write"&#10;  }&#10;  service_prefix "" {&#10;    policy = "write"&#10;    intentions = "write"&#10;  }&#10;}</pre> | |

图 4.12 – 引导策略和命名空间策略

请注意，引导策略中有整整一节与命名空间管理直接相关。因此，拥有这种特权策略的人能够管理每个命名空间中的 ACL、键、节点、会话和服务。换句话说，我们的州长有能力在城镇层面管理和控制事务，但城镇中的市长对其他城镇没有管辖权。此外，虽然在引导级别我们能够对集群中的节点进行写入，但在命名空间级别，由于命名空间可以访问所有节点，它只能读取。

正如我们所看到的，命名空间能够将整个 Consul 系统分割成独立的迷你系统，为这些系统的操作员提供自主权。然而，在一些高度安全的环境中，可能需要在网络层真正划分 Consul，而不仅仅是在应用层。

我们整个 Consul 集群，也就是每个命名空间中的每个节点，都通过很久以前讨论过的 gossip 协议进行通信。我们不打算深入探讨这个主题，因为它相当高级。不过，考虑一下这种情况：我们可能有数千个 Consul 节点分布在整个数据中心网络中，这种情况在生产环境中并不少见。如果你还记得，当我们开始用树示例（图 4.10）讨论命名空间时，分离发生在 Consul 节点之上的一层。当我们对 Consul 网络进行分段时，实际上是在把这些 gossip 池划分为更小的池，每个池都在自己的网络端口上进行通信。

![](img/52479dafaefba00517c48a137e037b7e_138_0.png)

我试图在图 4.13 中用不同类型的池来展示这种分段。在此示例中，所有网段（或池）都能够与核心集中式服务器集群通信。然而，由于防火墙或其他基于网络的划分，各池之间无法相互通信。通过利用 Consul 网络分段，我们仍然可以在被分割的网络中管理整个 Consul 集群、服务和命名空间。

现在，我们已经成功用 Consul 服务征服了整个数据中心，是时候继续实施 Brain 统治世界的目标了。所以，跟我来吧，Pinky，让我们开始讨论如何连接我们的 Consul 集群。

## 连接集群

认为我们所有的服务都会驻留在单个数据中心内，这种想法在 20 年前我刚开始写这本书时或许还行得通（似乎如此）。世界已经真正演变成一个持续连接的移动环境，尽管有些人可能认为这实际上是一种倒退。无论如何，在这个世界上真正维护我们服务之间通信的唯一方法，就是确保可以从多个地理区域发现它们的可用性和位置。无论我是在波士顿、柏林还是墨尔本，我所依赖的应用程序和服务都必须可用！我们在本章所做的这一切，都是为了走到这一步！

在整个书中，我们已经讨论了很多关于 gossip 和池的内容，我们将继续这一讨论。然而，你可以想象，在单个 Consul 集群内我们通信的数据量可能相当庞大，尤其是当我们接入所有服务时。如果我们愿意，我们可以简单地将 Consul 节点部署到不同的数据中心和区域，然后就完事了。不幸的是，这会导致大量流量在整个网络上广播。此外，将 Consul 服务器节点分散到多个数据中心可能会影响服务器集群的可用性，即使有出色的 Autopilot 也是如此。

当你在广域网（WAN）上加入 Consul 集群时，集群之间不会共享目录信息。每个服务器集群只能感知到其他集群的存在，因此当某个服务请求发出时，该请求可以被传输到另一个数据中心以检查服务的可用性。

