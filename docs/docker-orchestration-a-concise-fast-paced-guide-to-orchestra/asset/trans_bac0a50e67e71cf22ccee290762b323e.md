

文件更改后，可以将其传递给 `dcos`：

```
$ dcos marathon app update nginx < nginx-1.11.json
Created deployment 2c28f5db-0fcd-4605-9d5b-b1abd5986579
```

同样，Marathon 将使用更新后的镜像替换旧任务。部署完成后，可以验证应用程序是否正在运行新镜像：

```
$ dcos marathon app show nginx | grep image
"image": "nginx:1.11",
```

## 设置升级策略

应用程序可以定义升级策略以控制如何替换应用程序任务。默认情况下，Marathon 会在停止旧任务之前启动所有新任务。这可能会出于两个原因造成问题：

- 首先，如果应用程序无法处理多个任务向同一卷写入数据，则更新可能会损坏数据
- 其次，您需要有足够的容量来运行所有这些额外任务

为解决此问题，Marathon 允许为应用程序设置 `upgradeStrategy`：

```
"upgradeStrategy": {
"minimumHealthCapacity": 1
}
```

`minimumHealthCapacity` 选项是 0 到 1 之间的值。设置为 1 时，新实例在停止旧实例之前启动。设置为 0 时，所有旧实例在新实例启动之前停止。这解决了第一个问题。从技术上讲，它也解决了第二个问题，但当实例数量过低，无法为尝试访问它的客户端提供服务时，可能会出现明显的停机时间。

为在尽量减少对客户端影响的情况下平滑更新应用程序，`minimumHealthCapacity` 可以设置为 0 到 1 之间的分数。此值表示将应用程序缩减到的运行实例百分比。例如，如果有十个实例且 `minimumHealthCapacity` 设置为 `0.4`（40% 容量），应用程序将缩减到四个实例，并启动六个新实例。超过 `0.5` 的任何值意味着升级需要额外容量。如果新实例没有错误，其余旧实例将关闭，新实例将启动以取代它们。

## 使用蓝绿部署测试更新

内置更新机制非常适合自动部署应用程序的新版本。但是，在某些情况下，可能需要更缓慢地推出更新以验证不会对最终用户产生影响。如果更新带来重大变化，这尤其有用。

蓝绿部署是用新版本（称为绿色）缓慢替换旧应用程序（称为蓝色）。在 Marathon 中，此手动过程是通过创建使用更新镜像的新应用程序来完成的。为新应用程序指定一个标题，使其明显是新应用程序。

绿色应用程序启动少量任务，通常只有一个。一旦健康检查显示一切就绪，绿色应用程序将被添加到服务的负载均衡器。假设有较多数量的旧蓝色任务正在运行，大多数请求将发送到那里。不过，少量请求将发送到绿色应用程序。

一旦请求开始发送到绿色应用程序，就可以监控可能需要回滚的错误或其他问题。随着测试通过，绿色应用程序可以扩展，蓝色应用程序可以缩减。这是一个可以花费所需时间的手动过程。在选择扩展绿色应用程序之前，等待数小时或数天并不罕见。如果出现问题，绿色应用程序可以缩减或完全从负载均衡器中删除。

当您确信绿色应用程序已完全准备好投入生产时，绿色应用程序可以扩展到所需的任务数量，蓝色应用程序可以缩减到零并从负载均衡器中删除。此时，应用程序已完全升级。一旦明确不再需要蓝色应用程序进行回滚，就可以将其从系统中删除。

有关蓝绿部署及其脚本化的更多信息可在此处查阅：https://mesosphere.github.io/marathon/docs/blue-green-deploy.htm 1.

## 介绍 Pod

随着 1.9 版本的发布，DC/OS 引入了 Pod。DC/OS 和 Marathon 中的 Pod 与 Kubernetes 中的 Pod 非常相似。它们允许将多个容器分组到单个对象中。所有容器可以共享卷并通过 localhost 相互通信。

## 使用网络服务和负载均衡

Mesosphere 提供多种网络服务，使编排更加容易。这些服务为运行中的应用程序提供服务发现和负载均衡。

## 使用 DNS 发现服务

创建的每个服务都会在 Mesos DNS 中分配一个主机名。条目为 service-name.marathon.mesos，其中 service-name 是 id 字段中的服务名称。以下片段来自 id 设置为 simple-nginx 的服务。Mesos DNS 将创建名为 simple-nginx.marathon.mesos 的记录：

```
{
  "id" : "simple-nginx",
  ...
}
```

因为服务直接翻译为 DNS 主机名，存在一些限制，如下所示：

- 首先，名称只能包含字母、数字和短划线 (-)。
- 其次，名称必须短于 24 个字符。较长的名称会被缩短。如果两个服务最终具有相同的主机名，这可能会导致问题。一个服务中运行的任务可能会被返回给另一个服务。

还可以使用 REST API 通过 HTTP 查询 DNS 服务。条目以 JSON 格式返回。以下示例获取 simple-nginx 服务的 IP 信息：

```
$ curl http://master.mesos:8123/v1/hosts/simple-nginx.marathon.mesos
[
  {
    "host": "simple-nginx.marathon.mesos.",
    "ip": "10.128.0.4"
  }
]
```

## 连接到覆盖网络

迄今，示例使用了主机网络。DC/OS 在安装时创建 Docker 覆盖网络。将应用程序连接到覆盖网络可启用负载均衡以及跨网络连接其他服务的功能：

```
{
"id" : "nginx-overlay",
"instances" : 1,
"cpus" : 0.25,
"mem" : 64.0,
"container" : {
"type" : "DOCKER",
"docker" : {
"image" : "nginx",
"network" : "USER"
}
},
"ipAddress":{
"networkName":"dcos"
}
}
```

前面的示例创建了一个名为 nginx-overlay 的服务，该服务连接到覆盖网络。network 标签设置为 USER，networkName 标签设置为覆盖网络的名称，在此例中为 dcos。

## 使用虚拟 IP

Marathon 可以将基于名称的虚拟 IP 分配给服务。虚拟 IP 为其附加的服务提供四层负载均衡。集群中的任务随后可以使用虚拟 IP 访问服务。

若要添加虚拟 IP，请在 Web 界面中单击服务，然后单击 **编辑** 按钮。转到 **网络** 部分。选择 **虚拟网络**。这与将服务连接到覆盖网络相同。输入 **容器端口**、**名称** 和 **协议**，然后单击 **负载均衡** 旁边的复选框。如果需要更多端口，请单击 **添加端点** 链接。完成后，单击 **部署更改**：

![](img/02bead9c8ff4515f78dac0eb33163961_162_0.png)

服务将被分配一个如 `service-name.marathon.14lb.thisdcos.discovery` 的名称，其中 `service-name` 是服务的名称。对于名为 `nginx` 的服务，名称是 `nginx.marathon.14lb.thisdcos.discovery`。随后可以在集群中的任何位置使用虚拟 IP 访问该服务：

```
$ curl nginx.marathon.14lb.thisdcos.discovery:80
<!DOCTYPE html>
<html>
...
```

## 使用 Marathon 负载均衡器

Marathon 提供名为 Marathon-LB 的第二个负载均衡器。它是基于 HAProxy 的服务，可以处理多个服务，并可根据 Web 服务的主机名进行代理。Marathon-LB 是通过 DC/OS 包管理器安装的插件：

```
$ dcos package install marathon-lb
We recommend a minimum of 0.5 CPUs and 256 MB of RAM available for the
Marathon-LB DC/OS Service.
Continue installing? [yes/no] yes
Installing Marathon app for package [marathon-lb] version [1.4.1]
Marathon-lb DC/OS Service has been successfully installed!
See https://github.com/mesosphere/marathon-lb for documentation.
```

`dcos` 命令将安装 Marathon-LB。默认情况下，安装配置为提供外部请求。负载均衡器在 DC/OS 中显示为名为 marathon-lb 的服务。

将新服务添加到负载均衡器非常简单。以下是运行 nginx 的服务：

```
{
  "id" : "nginx-mlb",
  "instances" : 1,
  "cpus" : 0.25,
  "mem" : 64.0,
  "labels" : {
    "HAPROXY_GROUP" : "external"
  },
  "container" : {
    "type" : "DOCKER",
    "docker" : {
      "image" : "nginx",
      "network" : "BRIDGE",
      "portMappings" : [
        {
          "hostPort" : 0,
          "containerPort" : 80,
          "servicePort" : 10001,
          "name" : "web"
        }
      ]
    }
  },
  "upgradeStrategy": {
    "minimumHealthCapacity": 1,
```

