

![](img/2f752555f69992669defa8b25e6dcee9_76_0.png)

某些部署可能会为每个服务创建一个 VIP。这允许多个 HAProxy 节点为不同服务响应 HTTP 会话。在本示例中，我们将仅使用一个单一 VIP，因为 HAProxy 处于主/备配置。

以下示例 `/etc/haproxy/haproxy.cfg` 将在我们的环境中对 Horizon 进行负载均衡：

```
global
    daemon
    group haproxy
    maxconn 40000
    pidfile /var/run/haproxy.pid
    user haproxy

defaults
    log 127.0.0.1 local2 warning
    mode tcp
    option tcplog
    option redispatch
    retries 3
    timeout connect 10s
    timeout client 60s
    timeout server 60s
    timeout check 10s

listen horizon
    bind 192.168.0.30:80
    mode http
    cookie SERVERID insert indirect nocache
    option tcplog
    timeout client 180s
    server controller1 192.168.0.10:80 cookie controller1 check inter 1s
    server controller2 192.168.0.11:80 cookie controller2 check inter 1s
```

在本示例中，`controller1` 的 IP 地址为 `192.168.0.10`，`controller2` 的 IP 地址为 `192.168.0.11`。我们选择的 VIP 为 `192.168.0.30`。复制此文件，将 IP 地址替换为实验室中的地址，然后将其放到每个控制器的 `/etc/haproxy/haproxy.cfg` 中。

为了使 Horizon 能响应 VIP 上的请求，我们需要在 Apache 虚拟主机配置中将 VIP 添加为 ServerAlias。这在我们的实验室安装中位于 `/etc/httpd/conf.d/15-horizon_vhost.conf`。查找以下行：

```
ServerAlias 192.168.0.10
```

在两个控制器上添加一个包含 VIP 的额外 ServerAlias 行：

```
ServerAlias 192.168.0.30
```

您还需要告诉 Apache 不要监听 VIP，以便 HAProxy 可以绑定到该地址。为此，修改 /etc/httpd/conf/ports.conf 并在端口号之外指定控制器的 IP 地址。以下是一个示例：

```
Listen 192.168.0.10:35357
Listen 192.168.0.10:5000
Listen 192.168.0.10:80
```

在本示例中，192.168.0.10 是第一个控制器的地址。为每台机器替换适当的 IP 地址。

重启 Apache 以应用新的别名：

```
# systemctl restart httpd.service
```

接下来，将 VIP 和 HAProxy 服务作为资源添加到 Pacemaker 集群中。这些命令应仅在第一个节点上运行：

```
# pcs resource create VirtualIP IPaddr2 ip=192.168.0.30 cidr_netmask=24
# pcs resource create HAProxy systemd:haproxy
```

将 HAProxy 服务与 VirtualIP 协同放置，以确保两者一起运行：

```
# pcs constraint colocation add VirtualIP with HAProxy score=INFINITY
```

验证资源是否已启动：

```
# pcs status
...
Full list of resources:
VirtualIP(ocf::heartbeat:IPaddr2):Started controller1
HAProxy(systemd:haproxy):Started controller1
...
```

此时，您应该能够使用指定的 VIP 访问 Horizon。流量将从客户端流向 VIP 上的 HAProxy，再流向两个节点之一上的 Apache。

## Additional API service configuration

现在我们已经拥有一个可工作的集群和 HAProxy 配置，最后的配置步骤是将每个 OpenStack API 端点都置于负载均衡器之后。此过程包含三个步骤，分别是：

- 1. 更新 HAProxy 配置以包含该服务。
- 2. 将 Keystone 服务目录中的端点移动到 VIP。
- 3. 重新配置服务，使其指向 VIP 而不是第一个控制器的 IP。

在以下示例中，我们将把 Keystone 服务置于负载均衡器之后。此过程可对每个 API 服务重复。

首先，在 HAProxy 配置文件中为 Keystone 的授权和管理员端点添加一个部分：

```
listen keystone-admin
    bind 192.168.0.30:35357
    mode tcp
    option tcplog
    server controller1 192.168.0.10:35357 check inter 1s
    server controller2 192.168.0.11:35357 check inter 1s

listen keystone-public
    bind 192.168.0.30:5000
    mode tcp
    option tcplog
    server controller1 192.168.0.10:5000 check inter 1s
    server controller2 192.168.0.11:5000 check inter 1s
```

确保在两个控制器上更新配置。在活动节点上重启 haproxy 服务：

```
# systemctl restart haproxy.service
```

您可以使用 pcs status 的输出确定活动节点。使用以下命令检查 HAProxy 是否已在 5000 和 35357 端口上监听：

```
# curl http://192.168.0.30:5000
# curl http://192.168.0.30:35357
```

两者都应输出一些描述 Keystone 服务状态的 JSON。

## 4

## Building the Deployment Pipeline

我们经常告诉我们合作的客户，OpenStack 不是被 **installed**，而是被 **deployed**。虽然词语上的差异可能看起来很细微，但它可能在组织内部带来革命性的变化。大多数企业基础设施团队习惯于在部署新基础设施平台时采用以下流程：

- 1. 安装平台。
- 2. 配置并集成平台。
- 3. 运行平台。
- 4. 升级平台。

安装和配置平台可能需要数月或数年时间，一旦安装完成，平台预计将运行数年。对平台的升级每 3 到 5 年发生一次，是为期 6-12 个月的大型项目。Red Hat 已围绕这些周期构建了企业 Linux 操作系统的发布结构——RHEL 5 和 RHEL 6 之间相隔 3 年，RHEL 6 和 RHEL 7 之间相隔近 4 年。每个版本都支持 10 年，保守的基础设施团队在新版本发布后至少等待一年才会开始为期 6-12 个月的升级项目。

这种对平台部署的保守方法在操作系统、虚拟化平台和应用平台领域工作得非常好。然而，如果您私有云项目的最重要业务驱动因素是提高敏捷性，那么以 12 个月的推出周期和 5 年的生命周期是不太可能实现目标的。

在本章中，我们将研究 OpenStack 通常作为应用软件而非基础设施软件的部署方式。我们将构建一个简单的部署流水线，该流水线使用 Puppet 描述 OpenStack 部署，使用 Jenkins 这款开源持续集成工具来实现该部署，然后运行单元测试以验证部署是否符合我们的规范。在整章中，我们将讨论迭代开发以及它如何适用于 OpenStack 部署。

## Dealing with Infrastructure as a Software

一家大型公司的基础设施副总裁曾经这样告诉我们：“我喜欢硬件。我的硬件几乎从不坏。我部署的软件总是坏。当我的数据中心变成软件时，我怎么才能拥有一个稳定的平台？”。同样的担忧曾经也可能适用于虚拟化，但今天大多数组织对软件假装成硬件的想法都很舒适。软件定义的数据中心则有点更令人畏惧。虽然我们很早就有了软件假装成 CPU，但软件假装成存储阵列相对较晚。而且，当存储阵列宕机时，一切往往都会随之宕机。

然而，更大的问题可能在于软件在部署时不断出错。事实上，我们合作过的最成功的 OpenStack 部署都采用了现代软件开发技术，以确保其软件在部署时不会出错。

## Eating the elephant

我们将应用于部署流程的第一个也是最重要的概念是迭代开发。不是试图在一轮部署中解决所有已识别的需求，我们将其分解为用户故事，分配给为期两到三周的冲刺，然后逐一实现。冲刺是源自 Scrum 流程和框架的一种基本开发度量。冲刺是经过规划且限制在特定持续时间内的限时工作量（两周很常见）。这不仅允许我们限制每次发布的复杂性并专注于彻底测试每个组件，还允许我们在流程中更早地为业务带来价值。仅需要少量功能集的工作负载将能够在无需等待大型发布的情况下加入平台。

