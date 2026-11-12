

## 设置正向亲和性

有两个选项控制容器之间的亲和性。第一个，`MachineOf`，是 Fleet 中的单元文件名称。Fleet 会尝试将带有 `MachineOf` 选项的单元放置在运行名为该服务的同一节点上。

例如，假设上一节中描述的 MySQL 服务正在 Fleet 集群的某个位置运行。出于性能原因，理想的做法是在同一节点上运行使用该数据库的某个 Web 应用程序。在本示例中，`nginx` 将是 Web 应用程序：

```
[Unit]
Description=Nginx Affinity
After=docker.service
Requires=docker.service
Requires=mysql.service

[Service]
TimeoutStartSec=infinity
ExecStartPre=-/usr/bin/docker kill nginx
ExecStartPre=-/usr/bin/docker rm nginx
ExecStartPre=-/usr/bin/docker pull nginx
ExecStart=/usr/bin/docker run --name nginx -p 80:80 nginx
ExecStop=/usr/bin/docker stop nginx

[X-Fleet]
MachineOf=mysql.service
```

这是本章前面定义的同一基本 `nginx` 服务。这里有两点需要注意。首先，添加了一个 `Requires` 选项以确保 `mysql.service` 正在运行。如果它未运行且已定义，Fleet 将启动它。其次是 `[X-Fleet]` 节和 `MachineOf` 选项，它确保 Fleet 会尝试在与 `mysql.service` 相同的主机上运行 `nginx.service`。

需要牢记的是，亲和性定义应始终是单向的。如果不是，就会出现竞态条件。在此示例中，如果 `mysql.service` 也具有 `MachineOf=nginx.service`，Fleet 将无法启动任何一项服务。无法确定在何处启动第一项服务，因为另一项服务尚未启动。

## 使用反向亲和性

有时确保两个服务永远不会在同一节点上运行非常重要。通常，这是出于冗余和高可用性的原因。Fleet 提供了 `Conflicts` 选项以用于此类情况。`Conflicts` 选项可以精确匹配单元名称，也可以使用 glob 匹配匹配一组服务。

使用 glob 匹配时，星号 (*) 用于匹配零个或多个字符。例如，要匹配任何以 `nginx` 开头的服务，模式将是 `nginx*`。以下代码片段显示了一个配置为不会在运行以 `nginx` 开头的服务的任何节点上启动的单元：

```
[Unit]
Description=Nginx Affinity
After=docker.service
Requires=docker.service
...
[X-Fleet]
Conflicts=nginx*
```

## 高可用性

如前所述，反向亲和性通常用于高可用性。Fleet 使用 systemd 模板单元，这使得启动多个相同的服务变得容易。要开始，创建名为 `nginxha@.service` 的文件：

```
[Unit]
Description=Nginx HA
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=infinity
ExecStartPre=-/usr/bin/docker kill nginx
ExecStartPre=-/usr/bin/docker rm nginx
ExecStartPre=-/usr/bin/docker pull nginx
ExecStart=/usr/bin/docker run --name nginx -p 80:80 nginx
ExecStop=/usr/bin/docker stop nginx

[X-Fleet]
Conflicts=nginxha@*.service
```

大部分情况下，这与之前使用的 `nginx.service` 单元相同。`Conflicts` 选项已设置，以确保使用此模板的任何单元。

模板的每个实例都命名为 nginxha@id.service，其中 id 是对服务有意义的任何内容。通常，id 是一个数字，但不必是：

```
$ fleetctl start nginxha@1.service
Unit nginxha@1.service inactive
Unit nginxha@1.service launched on 86bceaea.../172.17.8.103
$ fleetctl start nginxha@2.service
Unit nginxha@2.service inactive
Unit nginxha@2.service launched on 9c07271b.../172.17.8.102
$ fleetctl start nginxha@3.service
Unit nginxha@3.service inactive
Unit nginxha@3.service launched on acbccfa4.../172.17.8.101
```

```
$ fleetctl list-units
UNIT                      MACHINE                         ACTIVE      SUB
nginxha@1.service         86bceaea.../172.17.8.103        active      running
nginxha@2.service         9c07271b.../172.17.8.102        active      running
nginxha@3.service         acbccfa4.../172.17.8.101        active      running
```

在此示例中，启动了 nginxha@.service 模板的三个实例。它们分别命名为 nginxha@1、nginxha@2 和 nginxha@3。请注意，它们都在不同的机器上启动。随后可以使用负载均衡器将负载分布到所有实例上。如果任何单元失败，整体服务将保持运行。

## 辅助服务

大多数服务之所以不知道集群中其他正在发生的事情，是出于设计考虑。服务如何让其他事物知道它已启动并准备好响应请求？为此，会启动一个名为 sidekick 的单独服务。sidekick 将通知其他服务容器已启动或停止。具体内容会因环境而异。例如，它可能会向外部负载均衡器发出 API 调用。

Fleet 没有原生方式来通知其他服务已有新单元启动。它拥有的是 etcd。etcd 键值存储正是为此类消息传递而设计的。以下示例是一个简单的 sidekick，在 nginx.service 启动时更新 etcd：

```
[Unit]
Description=Nginx Sidekick
BindsTo=nginx.service
After=nginx.service

[Service]
ExecStart=/bin/sh -c "while [ true ]; do etcdctl set
/services/websites/nginx '{ "host": "%H", "port": 80 }' --ttl 60; sleep 50;
done"
ExecStop=/usr/bin/etcdctl rm /services/websites/nginx

[X-Fleet]
MachineOf=nginx.service
```

`BindsTo` 选项的工作方式类似于 `Requires`，但它告诉 Fleet，如果 nginx.service 因任何原因停止，则停止 sidekick。`ExecStart` 选项运行一个无限循环，用 JSON 片段更新 etcd 中的 /services/websites/nginx 键。使用 --ttl 标志为键添加了 TTL，以确保 sidekick 失败时该键会消失。`sleep` 命令在再次更新键前等待 50 秒。时间可以根据环境需求进行调整，但 `sleep` 应始终小于 TTL 减去更新 etcd 所需的时间。

JSON 中的 %H 变量将被替换为运行 nginx-sidekick.service 的节点的主机名。如果 sidekick 与 nginx.service 启动在不同主机上，这可能会有问题。但是，Fleet 亲和性选项 MachineOf 确保 Fleet 始终将 sidekick 调度到与 nginx.service 相同的主机上。

另一项服务可以监控 etcd 树中的特定文件或目录，并根据树的更新采取行动。Confd 工具正是如此，它可以根据树的内容更新配置模板并重启服务。Confd 的实现留给读者作为练习。Confd 可在 https://github.com/kelseyhightower/confd 上获得。

## 启动全局单元

通常希望在集群中的每个节点上运行日志或监控容器。Fleet 通过 [X-Fleet] 中的 Global 选项实现此功能：

```
[Unit]
Description=Monitor
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=infinity
ExecStartPre=-/usr/bin/docker kill monitor
ExecStartPre=-/usr/bin/docker rm monitor
ExecStartPre=/usr/bin/docker pull monitor
ExecStartPre=mkdir -p /var/lib/monitor
ExecStart=/usr/bin/docker run -v /var/lib/monitor:/var/www/rrd --net=host --name=monitor hyper/host-monitor
ExecStop=/usr/bin/docker stop monitor

[X-Fleet]
Global=true
```

此示例启动一个简单的容器，用于创建绘制 CPU、内存和网络数据的 RRD 文件和图像。Global 选项告诉 Fleet 在集群中的每个节点上启动该服务的副本：

```
$ fleetctl start monitor.service
Unit monitor.service Triggered global unit monitor.service start
```

```
$ fleetctl list-units
UNIT MACHINE ACTIVE SUB
monitor.service 86bcea....../172.17.8.103 active running
monitor.service 9c07271b....../172.17.8.102 active running
monitor.service acbccfa4....../172.17.8.101 active running
```

就是这样。集群中的每个节点都有一个 monitor.service。可以根据需要为所需数量的全局服务执行此操作。

## 使用 Rancher Cattle

标题中的“Simpler”一词用于 Rancher Cattle 时有点名不副实。Cattle 是 Rancher 提供的功能齐全的编排系统。它比 Kubernetes 或 Mesosphere 更容易设置。它也更易于使用，因为它拥有强大的 Web 界面和基于 Docker Compose 的命令行工具。如果您已经使用过 Docker Compose，那么使用 Cattle 可能会感觉非常自如。

## 安装 Rancher

Rancher 及其组件在 Docker 容器中运行。这使得几乎在任何地方安装都非常容易。

