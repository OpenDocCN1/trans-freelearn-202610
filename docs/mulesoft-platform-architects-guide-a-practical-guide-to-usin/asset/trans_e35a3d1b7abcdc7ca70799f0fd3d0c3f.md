
在*第 11 章*中，我们讨论了 Quorum 管理以及如何利用它来确保 Runtime Fabric 的高可用性和容错基础设施。我们讨论了生产环境和非生产环境所需的最小主节点和控制节点数量，以确保基础设施的高可用性。

# Runtime Fabric（自管式 Kubernetes）的高可用性与容错性

Runtime Fabric 的应用层和基础设施层均可实现高可用性。设置多个工作节点有助于实现基础设施级别的高可用性。自管式 Kubernetes 提供托管的 Kubernetes 主节点，从而无需管理主节点。AWS 负责监管 EKS 主节点。再次强调，管理主节点的可用性是云提供商（在 EKS 的情况下为 AWS）的责任，而非 MuleSoft 的职责。

要在自管式 Kubernetes 和 VM/裸金属服务器上的 Runtime Fabric 中实现应用级高可用性，可以为应用程序部署多个副本。您可以将多个副本组成集群，以确保数据和信息分布在各个副本中。就像本地集群一样，Runtime Fabric 支持集群功能。

我们在[*第 11 章*](#B19922_11.xhtml#_idTextAnchor263)中详细讨论了 Anypoint 集群。

# 可扩展性

系统适应应用程序和系统处理需求变化而调整其性能和成本的能力称为可扩展性。示例包括操作系统在各种硬件类别上的运行方式、数据库处理不断增加的查询量的方式，或硬件系统处理用户数量增长的方式。

扩展分为两种类型：

*   **水平扩展**：水平扩展意味着向资源池中添加更多机器。水平扩展分为两种类型：
    *   **扩容**：向资源池中添加更多机器
    *   **缩容**：从资源池中移除一台机器
*   **垂直扩展**：垂直扩展意味着向现有服务器添加更多电源或计算资源（如内存和 CPU）：
    *   **向外扩展**：向现有机器添加更多算力
    *   **向内扩展**：减少现有机器的算力

## CloudHub 1.0 的可扩展性

CloudHub 1.0 是一项托管服务，我们无需监管其基础设施，它完全由 MuleSoft 管理。我们只负责选择运行 Mule 应用程序或代理所需的正确工作节点和工作节点规格。

MuleSoft 将根据我们为应用程序选择的工作节点和工作节点规格自动配置基础设施。应用程序可以通过增加或减少工作节点数量进行水平扩展，通过增加或减少工作节点规格进行垂直扩展。

CloudHub 1.0 支持自动扩展功能，正如[*第 9 章*](#B19922_09.xhtml#_idTextAnchor179)所述，它允许应用程序根据流量的增减自动扩展。

## CloudHub 2.0 的可扩展性

与 CloudHub 1.0 类似，CloudHub 2.0 是一项托管服务，无需监管其基础设施，完全由 MuleSoft 管理。我们只负责选择运行 Mule 应用程序或代理所需的正确副本和副本规格。

MuleSoft 将根据我们为应用程序选择的副本和副本规格自动配置基础设施。应用程序可以通过增加或减少副本数量进行水平扩展，通过增加或减少副本规格进行垂直扩展。CloudHub 2.0 不支持自动扩展功能。

我们在[*第 10 章*](#B19922_10.xhtml#_idTextAnchor228)中详细讨论了 CloudHub 2.0 的可扩展性。

## Runtime Fabric（裸金属服务器/VM）的可扩展性

Runtime Fabric 基础设施可通过向集群添加更多工作节点或主节点进行水平扩展，通过增加或减少 CPU 和内存等计算资源进行垂直扩展。实施基础设施级别的可扩展性变更是客户的责任。

应用程序可通过添加更多应用程序副本进行水平扩展，通过增加副本规格（预留 CPU、CPU 限制或内存）进行垂直扩展。

Runtime Fabric 是 MuleSoft 基于 Kubernetes 提供的用于运行 Mule 应用程序的服务。这抽象化了管理和维护 Kubernetes 集群的复杂性，使开发人员能够专注于应用程序开发和部署。

然而，Runtime Fabric 并不直接提供应用程序扩展功能。在这种情况下，Kubernetes 发挥作用，提供强大的扩展能力。

Kubernetes 是一个用于自动化容器化应用程序部署、扩展和管理的开源平台。

Kubernetes 提供了两种不同的应用程序实例自动扩展方式：

*   `Horizontal Pod Autoscaler` (`HPA`)
*   `Vertical Pod Autoscaler` (`VPA`)

当流量增加时，HPA 会添加更多 `Pod`。当负载减少时，`Pod` 的数量将缩减到最小值。

HPA 作为 Kubernetes API 的控制器和资源使用。控制器的行为由资源决定。位于 Kubernetes 控制平面中的水平 Pod 自动扩展控制器，使用观测到的指标（如平均 CPU 使用率、平均内存使用率或您指定的任何其他自定义指标）定期确定其对象（如 `Deployment`）的适当规模。

HPA 基于目标原则工作。它会调整 `Deployment` 或副本集中 `Pod` 的数量，以匹配给定资源（如 CPU 或内存）的预定义目标使用率。

要创建 HPA，需要使用 `kubectl autoscale` 命令：

```
kubectl autoscale deployment mule-test-app --cpu-percent=80 --min=2 --max=6
```

让我们分解一下前面的 `kubectl` 命令：

*   这里，`mule-test-app` 是应用程序的名称。
*   然后是 `--cpu-percent=80`，这意味着当所有 `Pod` 的平均 CPU 利用率超过 80% 时，Kubernetes 将添加更多副本。
*   最后，`--min=2` 是始终运行的最小副本数，`--max=6` 是在 CPU 利用率高时 Kubernetes 可以创建的最大副本数。

`kubectl autoscale` 命令是一个激活 HPA 的简单命令。然而，使用清单文件定义 HPA 更加灵活，因为它允许多个指标来启用扩展。

以下清单文件允许在节点的平均 CPU 利用率达到 80% 或更高时进行水平扩展。创建一个 `mulesoft-test-app-hpa.yaml` 清单文件。然后，执行 `kubectl apply` 命令以启用 HPA：

```
kubectl apply -f mulesoft-test-app-hpa.yaml
```

输出如下：

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: mulesoft-test-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: mule-test-app
  minReplicas: 2
  maxReplicas: 6
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 80
```

让我们仔细看看此 YAML 文件中定义的属性：

*   这里，`scaleTargetRef` 是该 HPA 将管理的 MuleSoft 部署。
