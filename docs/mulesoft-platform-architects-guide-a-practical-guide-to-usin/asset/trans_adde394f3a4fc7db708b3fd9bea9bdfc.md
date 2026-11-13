

## Pod 自动扩缩容（HPA 和 VPA）

*   然后，我们有 `minReplicas`，它指定将始终运行的最小副本数，以及 `maxReplicas`，它指定当 CPU 或内存利用率超过定义的利用率阈值时，Kubernetes 可以扩展的最大副本数。
*   `metrics` 指定 `HPA` 将用来决定何时扩缩容的属性。在本例中，我们使用了 `CPU utilization` 作为 `HPA` 的属性。

下图显示了当平均 CPU 利用率达到 80% 或更高时，`Pod` 如何水平扩缩容：

![Figure 16.3: Kubernetes’ HPA](image/B19922_16_3.jpg)

图 16.3：Kubernetes 的 HPA

`VPA` 会自动调整 `Pod` 运行应用程序所需的 CPU 和内存，使其达到合适的大小。这将确保集群中资源的有效利用。`VPA` 需要两种类型的配置：

*   `Requests`
*   `Limits`

`Requests` 是分配给 `Pod` 的最小资源量，这保证了 `Pod` 将获得的资源。`Limits` 是 `Pod` 在流量增加时可以消耗的最大资源量。然而，这不被保证，只有在有额外资源可用以容纳更多流量时才能分配。

下图显示了当需要更多资源时，`Pod` 如何垂直扩缩容：

![Figure 16.4: Kubernetes’ VPA](image/B19922_16_4.jpg)

图 16.4：Kubernetes 的 VPA

下表显示了如果需要更多资源可以采取的措施：

| 所需容量调整 | HPA | VPA |
| --- | --- | --- |
| 更多资源 | 添加更多 Pod | 增加现有 Pod 的内存或 CPU |
| 更少资源 | 移除 Pod | 减少分配给现有 Pod 的内存或 CPU |

表 16.1：HPA 和 VPA 资源分配

## Runtime Fabric 的可扩展性（自管理 Kubernetes）

自管理 Kubernetes 是云提供商提供的一种托管服务（AWS 的 `Elastic Kubernetes Service` (`EKS`)、Azure 的 `Azure Kubernetes Service` (`AKS`)、Google 的 `Google Kubernetes Engine` (`GKE`)）。

用户可以专注于他们的应用程序和工作负载，因为云提供商使用自管理 Kubernetes 来管理 `API server` 和 Kubernetes 控制平面，其中包括主节点、存储和其他组件。

这意味着云提供商维护并确保基本组件具有可扩展性和高可用性。

自管理 Kubernetes 可以轻松扩展以满足应用程序的需求。

它自动促进工作节点的供应和管理，这使得在应用程序需求发生变化时更容易扩展集群。

`etcd` 持久化层和 Kubernetes `API servers` 的可扩展性和可用性由云提供商自动管理。

应用程序可以通过添加更多应用程序副本进行水平扩展，并通过增加副本大小（预留 CPU、CPU 限制或内存）进行垂直扩展，例如裸金属服务器/VM 的 Runtime Fabric 组件。

## 灾难恢复 (DR)

DR 是任何组织为确保业务连续性并在自然灾害、区域故障和网络攻击等意外中断后尽快恢复关键业务功能而必须考虑的关键因素。

你需要定义 DR 策略以确保业务连续性；否则，你可能会遭受收入损失或声誉受损，甚至导致业务倒闭。

### CloudHub 1.0 的 DR（多区域）

CloudHub 1.0 在同一区域内跨可用区提供自动 DR。

我们在 [*第 9 章*](#B19922_09.xhtml#_idTextAnchor179) 中讨论了此选项。

可以设置跨多区域的 DR，以便如果整个区域宕机，我们有其他区域来确保业务连续性。

以下是关于 CloudHub 1.0 DR 的一些重要要点：

*   可以通过在两个不同区域创建两个 `Anypoint VPC` 来设置多区域 DR。其中一个区域是主区域，另一个是 DR 区域。
*   可以为每个区域设置两个单独的 Anypoint 专用负载均衡器。每个专用负载均衡器都可以映射到 `DNS` 服务器上的 `CNAME` 记录。
*   全局负载均衡器是客户的责任，MuleSoft 不提供。它将在主区域和 DR 区域之间路由流量。客户可以使用 `Route 53` 或任何 `DNS` 路由器在主区域和 DR 区域之间路由流量。
*   应用程序名称、`Anypoint VPC` 名称和 Anypoint 专用负载均衡器名称必须在各区域中唯一。
*   `object store` 是一项区域性服务，无法跨区域访问。我们需要定义一种策略，以便跨区域使用 `object store` 或任何持久化存储。
*   必须在主区域和 DR 区域之间建立单独的 `VPN` 隧道、`VPC peering` 或 `AWS Transit Gateway` 连接。
*   在主动/主动 DR 机制中，从主区域切换到 DR 区域将比主动/被动 DR 更快，因为负载均衡器会识别主区域不健康并自动将所有流量路由到 DR 区域。
*   在主动/被动 DR 机制中，从主区域切换到 DR 区域将是手动的。你可能需要将应用程序部署到 DR 区域，并且激活它需要额外的时间。这可能导致较高的 `recovery time objective` (`RTO`)。

`RTO` 是企业在区域故障期间可接受的停机时间量。例如，如果企业可接受 1 小时的停机时间，则 `RTO` 为 1 小时。

`recovery point objective` (`RPO`) 是系统恢复后企业可接受的数据丢失量。例如，如果系统在下午 2 点故障，最多可接受 2 小时的数据丢失，则 `RPO` 将为下午 4 点：

![Figure 16.5: CloudHub 1.0 DR](image/B19922_16_5.jpg)

图 16.5：CloudHub 1.0 DR

图 16.5 展示了如何在 CloudHub 1.0 中设置跨多区域的 DR，确保即使一个区域完全宕机，应用程序仍然可用。

我们可以在两个不同的区域设置两个 `Anypoint VPC`，每个区域将有一个专用负载均衡器，将流量路由到部署在 `Anypoint VPC` 内的应用程序。

全局负载均衡器负责在两个区域之间负载均衡流量。

如果其中一个区域宕机，全局负载均衡器将检测到这一点，并将所有流量路由到另一个区域。

### CloudHub 2.0 的 DR（多区域）

CloudHub 2.0 在同一区域内跨可用区提供自动 DR。

我们在 [*第 10 章*](#B19922_10.xhtml#_idTextAnchor228) 中讨论了此选项。

可以设置跨多区域的 DR，以便如果整个区域宕机，我们有其他区域来确保业务连续性。

以下是关于 CloudHub 2.0 DR 的一些重要要点：

*   可以通过在两个不同区域创建两个 `private spaces` 来设置多区域 DR。其中一个区域是主区域，另一个是 DR 区域。
*   将为不同区域中的 `private space` 创建单独的私有和公共端点。
*   全局负载均衡器是客户的责任，MuleSoft 不提供。它将在主区域和 DR 区域之间路由流量。客户可以使用 `Route 53` 或任何 `DNS` 路由器在主区域和 DR 区域之间路由流量。
*   `object store` 是区域性的，无法跨区域访问。我们需要定义一种策略，以便跨区域使用 `object store` 或任何持久化存储。
*   必须在主区域和 DR 区域之间建立单独的 `VPN` 隧道或 `AWS Transit Gateway` 连接。CloudHub 2.0 中的 `VPN` 默认在所有区域中都具有高可用性。

