
负载均衡服务也可能位于它们自己的节点上。将可扩展服务映射到物理（或虚拟）主机时，主要考虑以下因素：

-   该服务是水平扩展还是垂直扩展？
-   对该服务进行垂直扩展是否会妨碍其他共置服务的性能？
-   该服务是否有其他服务不具备的特殊硬件或网络要求？

例如，一些使用 HAProxy 负载均衡服务的 OpenStack 部署选择将负载均衡节点分离出来，放在单独的硬件上。负载均衡节点托管的 VIP 必须处于公开的可路由网络上，而它们所路由到的服务的内部 IP 则没有此要求。将 HAProxy 服务放在独立主机上，可以让控制平面的其余部分仅使用私有地址。

将所有 API 服务集中在专用主机上可能有助于水平扩展。这些服务不需要由集群资源管理器管理，可以通过向负载均衡器添加额外节点来进行扩展，而无需更新集群定义。数据库服务具有高 I/O 需求。将这些服务隔离到可以访问高性能光纤通道的机器上可能是合理的。

最后，你应该考虑是否虚拟化控制平面。如果控制平面将进行虚拟化，创建额外的主机组来托管专用服务就会变得非常有吸引力。将八九台虚拟机专用于控制平面与将八九台物理机专用于控制平面是完全不同的情况。

大多数高可用控制平面至少需要三个节点，以确保集群资源管理器能够轻松确定法定人数。虽然为一百节点的 OpenStack 部署专门使用三个物理节点来承担控制功能非常合理，但专门使用九个物理节点可能就不一定了。我们合作过的许多组织已经拥有基于 VMware 的集群可用于托管管理设备，控制平面可以部署在该现有环境内。部署纯 KVM 云的组织可能不想承担在 OpenStack 之外管理额外虚拟机所带来的额外运维复杂性。

## 更新物理架构设计

一旦确定服务到物理（或虚拟）主机的映射，就应该更新设计文档，加入主机组定义及其相关功能。下面给出一个简单示例：

-   **负载均衡器：**这些系统以主动/被动配置提供负载均衡服务
-   **云控制器：**这些系统以主动/主动配置提供 API 服务、调度服务和 Horizon 仪表板服务
-   **数据库节点：**这些系统以主动/被动配置提供 MySQL 数据库服务
-   **消息节点：**这些系统以主动/主动配置提供 RabbitMQ 消息服务
-   **计算节点：**这些系统充当 KVM 虚拟化主机，并运行 `nova-compute` 和 `openvswitch-agent` 服务

仅使用云控制器主机组的部署可以使用以下定义：

-   **云控制器：**这些系统以主动/被动配置提供负载均衡服务，并以主动/主动配置提供 API 服务、MySQL 数据库服务和 RabbitMQ 消息服务
-   **计算节点：**这些系统充当 KVM 虚拟化主机，并运行 nova-compute 和 openvswitch-agent 服务

定义主机组后，应更新物理架构图，以反映主机组到部署中物理主机的映射。这还应包括网络连接方面的考虑。以下是可包含在设计文档中的架构图示例：

![](img/2f752555f69992669defa8b25e6dcee9_72_0.png)

## 在实验室部署中实现高可用性（H/A）

在最后这一节中，我们将通过添加第二个控制器节点并配置集群软件，使我们在第 2 章《构建云架构》中部署的当前实验室部署实现高可用。此示例应足够简单，以便在实验室中轻松实施，同时仍允许你评估相关技术。

### 配置第二台控制器

在后续章节中，我们将介绍更灵活的部署方法，这些方法允许细粒度的服务放置和自动化集群配置。在本章中，我们只扩展第 2 章《构建云架构》中的 Packstack 部署。Packstack 并不用于部署多个控制器，因此需要手动配置一些服务。

要配置第二台控制器，请以配置第一台控制器相同的方式，在新机器上安装操作系统，然后从第一台控制器复制 Packstack 应答文件。编辑应答文件，将 CONFIG_CONTROLLER_HOST、CONFIG_NETWORK_HOSTS 和 CONFIG_STORAGE_HOST 替换为新控制器的 IP 地址。

我们不会重新配置计算节点或现有控制器，因此请将你在第 2 章《构建云架构》中配置的计算节点和控制器添加到应答文件的 EXCLUDE_SERVERS 参数中。我们会将 MySQL、Redis、MongoDB 和 RabbitMQ 服务保留在第一台控制器上，因此不应修改这些值。

使用我们在第 2 章《构建云架构》中使用的相同命令，在新控制器上运行 Packstack：

```
packstack --answer-file <answer-file>
```

Packstack 完成后，你应该可以通过访问第二台主机的 IP 地址，并使用第 2 章《构建云架构》中使用的相同凭据，登录仪表板。

### 安装 Pacemaker 资源管理器

接下来，我们将安装 Pacemaker 来管理之后要与 HAProxy 配合使用的 VIP，以使 Web 服务高可用。我们假设控制器节点已经可以通过 yum 获取集群软件。

1.  首先，在两台节点上使用以下命令安装 Pacemaker：

    ```
    # yum install -y pcs fence-agents-all
    ```

2.  运行以下命令验证软件是否安装正确：

    ```
    # rpm -q pcs
    pcs-0.9.137-13.el7_1.3.x86_64
    ```

3.  接下来，向防火墙添加规则以允许集群流量：

    ```
    # firewall-cmd --permanent --add-service=high-availability
    ```

4.  使用以下命令在每个节点上设置 Pacemaker 集群的密码：

    ```
    # passwd hacluster
    ```

5.  在每个节点上启动 Pacemaker 集群管理器：

    ```
    # systemctl start pcsd.service
    # systemctl enable pcsd.service
    ```

6.  然后，在第一台节点上使用以下命令对节点进行身份验证：

    ```
    # pcs cluster auth controller1 controller2
    Username: hacluster
    Password:
    controller1: Authorized
    controller2: Authorized
    ```

7.  最后，在第一台节点上运行以下命令以创建集群并启动它：

    ```
    # pcs cluster setup --start --name openstack \
    > controller1 controller2
    Shutting down pacemaker/corosync services...
    Redirecting to /bin/systemctl stop pacemaker.service
    Redirecting to /bin/systemctl stop corosync.service
    Killing any remaining services...
    Removing all cluster configuration files...
    controller1: Succeeded
    controller2: Succeeded
    Starting cluster on nodes: controller1, controller2...
    controller1: Starting Cluster...
    controller2: Starting Cluster...
    ```

8.  对于我们的示例集群，我们将使用以下命令禁用 fencing：

    ```
    # pcs property set stonith-enabled=false
    ```

9.  使用以下命令验证集群是否成功启动：

    ```
    # pcs status
    Cluster name: openstack
    Last updated: Mon Aug 31 01:51:47 2015
    Last change: Mon Aug 31 01:51:14 2015
    Stack: corosync
    Current DC: controller1 (1) - partition with quorum
    Version: 1.1.12-a14efad
    2 Nodes configured
    0 Resources configured
    Online: [ controller1 controller2 ]
    ```

有关设置 Pacemaker 的更多信息，请参阅 http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Clusters_from_Scratch/index.html 上出色的 Clusters from Scratch 文档。

### 安装并配置 HAProxy

在本实验室部署中，我们将使用 HAProxy 对控制平面服务进行负载均衡。一些部署可能还会使用 Keepalived，并以主动/主动配置运行 HAProxy。对于此部署，我们将以主动/被动配置运行 HAProxy，并将其与我们的 VIP 一起作为 Pacemaker 中的资源进行管理。

首先，在两台节点上使用以下命令安装 HAProxy：

```
# yum -y install haproxy
```

使用以下命令验证安装：

```
# rpm -q haproxy
haproxy-1.5.4-4.el7_1.x86_64
```

接下来，我们将为 HAProxy 创建一个配置文件，用于对两个控制器上安装的 API 服务进行负载均衡。使用以下示例作为模板，将示例中的 IP 地址替换为两个控制器的 IP 地址，以及你用于对 API 服务进行负载均衡的 VIP 地址。
