
## 进一步阅读

无论本章多么详尽，它只是触及了 Kubernetes 能力的表面。以下是未涵盖的一些功能列表：

- **Ingress 资源**：Ingress 资源是一种与服务协同工作的新型负载均衡器抽象。它为 Web 应用程序提供了更精细的路由，包括基于主机和基于路径的路由。参阅 http://kubernetes.io/docs/user-guide/ingress/。

- **Kubernetes 控制面板**：Kubernetes 随附 kube-ui 插件，该插件提供了一个用于查看集群和快速创建新 Pod 及其他资源的控制面板。参阅 http://kubernetes.io/docs/user-guide/ui/。

- **Pod 资源限制**：可以在命名空间级别设置每个 Pod 和每个容器的 CPU 和内存限制。参阅 http://kubernetes.io/docs/admin/limitrange/。

- **配额**：配额可以在项目级别应用，以确保 Pod 的总数或资源用量不超过限制。参阅 http://kubernetes.io/docs/admin/resourcequota/。

- **批处理作业**：Job 是一种预期会运行并终止的资源类型。多个作业可以并行运行。参阅 http://kubernetes.io/docs/user-guide/jobs/。

- **健康检查**：Kubernetes 有能力针对容器运行健康检查，以验证其是否正常运行。参阅 http://kubernetes.io/docs/user-guide/liveness/。

## 摘要

在本章中，你学习了如何安装 Kubernetes 和创建资源。你学习了如何创建 Pod 以及如何将其与 Deployment 配合使用以实现复制和更新。讨论了 DNS-SD 和服务等网络功能，并展示了如何创建负载均衡器以使服务可通过 Internet 访问。最后，讨论了卷，你学习了如何使用 PersistentVolume 使数据跨 Pod 可用。下一章将探讨如何使用 Apache Mesosphere 作为 Docker 编排平台。

## 使用 Mesosphere

Apache Mesos 是一个分布式内核。它允许你在一组服务器上工作，就像它们是一个单一资源一样。Mesosphere 和 DC/OS 在此基础上构建，为运行任何工作负载（包括 Docker 容器）提供了一个强大的系统。对 Cassandra 和 Jenkins 等分布式工作负载以及 Docker 的原生支持，构成了一个强大的组合。

Microsoft 的 Azure 容器服务（ACS）基于 DC/OS。2016 年 3 月，Mesosphere 将 DC/OS 开源，使入门比以往任何时候都更容易。

本章将探讨使用 Mesosphere 来管理和部署基于 Docker 的应用程序。它将涵盖部署、缩放和升级新应用程序。它还将展示如何使用带有 Marathon 负载均衡功能的内置覆盖网络。最后，本章将介绍使用卷来持久化数据以及如何使用私有镜像仓库。

本章将涵盖以下主题：

- 安装 Mesos 和 DC/OS
- 管理容器和应用程序
- 缩放应用程序
- 使用健康检查
- 缩放应用程序
- 使用零停机升级更新应用程序
- 连接到 Mesos 覆盖网络
- 使用虚拟 IP 和 Marathon 负载均衡器
- 使用本地卷和外部卷
- 使用私有镜像仓库

### 开始使用 DC/OS

安装 DC/OS 是一个在此处不予详述的复杂过程，且说明因环境而异。在开始此过程之前，请务必阅读文档。有些功能（例如主节点的数量或外部持久存储）必须在安装时启用，以后若不重新安装便无法更改。安装说明可在 https://dcos.io/docs/1.8/administration/installing/ 查阅。

让 Mesosphere 运行起来最简单的方法是在 AWS 上使用堆栈安装，或使用运行 Mesosphere 的 ACS。

如果你是手动安装或在本地硬件上安装，需要注意以下几点：

- 节点必须至少拥有两个内核或虚拟处理器。
- DC/OS 节点必须运行 CoreOS 7、RHEL 7 或 CoreOS。
- 集群中的所有节点必须启用 NTP。NTP 服务器在 /etc/systemd/timesyncd.conf 中设置。设置 NTP 服务器后，运行 systemctl enable systemd-timedated.service 和 systemctl start systemd-timedated.service 以启用并启动 systemd 内部 NTP 客户端。以下是用于 GCE 的 timesyncd.conf 示例：

```
[Time]
NTP=time1.google.com time2.google.com
time3.google.com time4.google.com
FallbackNTP=0.pool.ntp.org 1.pool.ntp.org
0.fr.pool.ntp.org
```

### 登录 Web 界面

安装 DC/OS 后，即可使用 Web 界面登录。Mesosphere 使用 OAuth 进行 Google、GitHub 或 Microsoft 账户身份验证。登录后，你将被引导至 Mesosphere **控制面板**。在那里，你可以开始管理应用程序或安装命令行客户端 `dcos`：

![](img/02bead9c8ff4515f78dac0eb33163961_148_0.png)

### 安装 DC/OS 客户端

安装 DC/OS 后，登录网站。在左下角，弹出菜单中有一个标记为 **安装 CLI** 的链接。点击此链接后打开的窗口中有适用于 Windows、OS X 和 Linux 的客户端安装说明：

![](img/02bead9c8ff4515f78dac0eb33163961_149_0.png)

客户端也可以手动安装。在 Linux 上，安装命令如下：

```
$ wget -O dcos
https://downloads.dcos.io/binaries/cli/linux/x86-64/dcos-1.8/dcos
$ chmod +x dcos
$ sudo cp dcos /usr/local/bin/
$ dcos config set core.dcos_url https://m1.dcos
```

将 URL 中的版本号更改为与主服务器版本匹配的版本。在本示例中，它是 1.8。最后一条命令配置 DC/OS 主服务器的位置。将传递给 `dcos config` 的 URL 更改为适合你环境的适当地址。如果你没有有效的 SSL 证书，`dcos` 可以配置为忽略验证：

```
$ dcos config set core.ssl_verify false
```

安装 dcos 后，即可登录：

```
$ dcos auth login
```

现在在浏览器中访问以下链接：

```
https://m1.dcos/login?redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

输入身份验证令牌：

转到指示的 URL。DC/OS 将尝试通过 https://auth0.com/ 使用 OAuth 2 对你进行身份验证。复制令牌并将其粘贴到 shell 中。你现在应该已登录并准备好使用命令行。

### 管理应用程序

Mesos 集群使用 dcos 工具和 Web 界面进行管理。Mesos 中的应用程序由 Marathon 调度器处理，并以 JSON 格式定义。

### 运行简单应用程序

让我们从运行 Nginx 的简单容器开始。创建一个名为 nginx.json 的文件，包含以下定义：

```
{
    "id" : "simple-nginx",
    "instances" : 1,
    "cpus" : 0.25,
    "mem" : 64.0,
    "container" : {
        "type" : "DOCKER",
        "docker" : {
            "image" : "nginx:1.11",
            "network" : "HOST"
        }
    }
}
```

id 标签是服务的名称。它在服务列表中显示。instances 标签告诉 Marathon 只需要一个实例。以后可以根据需要增加或减少。

`cpus` 和 `mem` 标签是 Marathon 所需 CPU 百分比和 RAM 用量的提示。它们不会在 Docker 中实际设置资源限制。但是，Marathon 可能会终止使用超过分配资源的任务。在此示例中，应用程序请求 25% 的 CPU 和 64 MB 的 RAM。在应用程序创建后，若不重新部署应用程序，则无法更改这些设置。

`container` 标签是定义 Docker 容器的位置。因为这是 Docker 容器，`type` 标签设置为 `DOCKER`。镜像在 `image` 标签中设置。这与将传递给 `docker run` 的镜像名称相同。

最后，`network` 标签设置为 `HOST`，这告诉 Docker 引擎使用主机网络。本章后面将使用其他网络类型，但目前这足够了。

使用 `dcos marathon app add nginx.json` 启动服务。如果一切顺利，该命令不会产生任何输出。否则，将显示错误。使用 `dcos marathon app list` 验证应用程序是否正在运行：

```
$ dcos marathon app list
ID                MEM   CPUS   TASKS   HEALTH  DEPLOYMENT  CONTAINER  CMD
/simple-nginx     64    0.25   1/1     ---     ---         DOCKER     None
```

Nginx 正在运行，但你如何访问它呢？由于使用了主机网络，你可以将 Web 浏览器指向容器运行所在节点的外部 IP 地址。主机地址可在 Web 界面的服务详细信息中查看，或通过运行 `dcos marathon app show simple-nginx` 获取。

### 停止应用程序

可以使用 `dcos marathon app stop` 命令停止服务而不将其从 Marathon 中移除。Marathon 将创建一个部署，将服务缩放为零个任务。Web 界面将此称为**挂起**应用程序：

```
$ dcos marathon app stop simple-nginx
Created deployment e5be6896-f593-4a32-852f-30bd98551a0e
$ dcos marathon app list
ID                MEM   CPUS   TASKS   HEALTH  DEPLOYMENT  CONTAINER  CMD
/simple-nginx     64    0.25   0/0     ---     ---         DOCKER     None
```
