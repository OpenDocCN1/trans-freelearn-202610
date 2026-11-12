# 使用 Kubernetes 部署和管理服务

Kubernetes 最初由 Google 启动，用于管理容器，并于 2014 年 6 月作为开源软件发布。其他公司也迅速加入。Kubernetes 是 Red Hat OpenShift 3、Deis Workflow 和 fabric8 的平台即服务（PaaS）产品的后端。每天，越来越多的组织正在部署 Kubernetes 来编排其服务。

Kubernetes 是一个巨大的项目；无法在单个章节中涵盖其所有内容。本章将介绍你使用 Kubernetes 开始编排容器所需的一切。完整文档可在 http://kubernetes.io/docs/ 上找到。

本章将涵盖以下主题：

- 安装 Kubernetes
- 运行 Pod
- 管理部署
- 使用滚动更新在零停机时间的情况下升级部署
- 使用服务和负载均衡器将应用程序暴露到 Internet
- 使用持久卷来保留容器数据

## 了解 Kubernetes

Kubernetes 是一个复杂的软件。因此，管理员需要了解许多组件。有些组件每天都会使用，而其他组件则会在后台静默工作，以保持一切正常运行。

## 命令行工具

以下是命令行工具：

- kubeadm：此工具是在 Kubernetes 1.4 中添加的，截至撰写本文时，仍被视为 alpha 工具。它是 Linux 主机上 Kubernetes 的新安装程序。

- kubectl：这是用于与运行中的集群交互的主要工具。它用于添加 Pod 和服务、执行滚动更新等。几乎所有与 Kubernetes 交互的人都会使用 kubectl。

### Master 组件

这些组件运行在 Master 节点上。它们负责集群的所有高级控制。以下是 Master 组件：

- etcd：Kubernetes 在这里保存其集群配置。请确保对此进行备份，否则在完全重建的情况下你将无法恢复集群。

- kube-apiserver：API 服务器是所有与集群交互的内容的主要接入点。kubectl 工具使用它来管理服务，控制器使用它进行配置，等等。kube-apiserver 组件是 Master 上唯一需要能够从集群外部访问的组件。它也是唯一需要连接到 etcd 的组件。

- kube-controller-manager：不出所料，这管理正在运行的控制器。这包括复制控制器、节点控制器、第三方控制器和服务帐户。

- kube-scheduler：这调度 Pod 在集群中运行。每当新 Pod 启动或缩放时，调度器会确定在哪些节点上运行它们。

### Node 组件

Node 组件运行在 Worker 节点上。它们控制正在运行的容器、网络和其他服务。它们如下：

- **kubelet**：这是每个节点上的主处理守护程序。它负责启动容器、挂载卷、下载密钥，并保持节点作为集群的一部分。

- **kube-proxy**：这负责维护网络规则，并将网络请求转发到正确的位置。

## 安装 Kubernetes

Kubernetes 可以安装在大多数云服务上，或者在你自己的硬件或虚拟机上本地安装。不出所料，由于 Kubernetes 是 Google 项目，它在 GCE 上运行效果最好，新功能倾向于首先在那里得到支持。

此外，Google 推出了一项名为 **Google Container Engine (GKE)** 的服务，它是一种预配置的 Kubernetes 服务。Google 维护 Kubernetes Master，并使添加新节点到集群变得容易。你只需要关心容器。这是开始使用 Kubernetes 的最简单方式。

对于本地安装，有几种选择。第一个是随 Kubernetes 1.4 作为 Beta 工具发布的新 `kubeadm` 工具。第二个是 `kube-up.sh` 脚本，它是 Kubernetes 发布包的一部分，可以在各种平台上安装 Kubernetes。

还有其他两个值得考虑的选项。RHEL 有一个可用的 `kubernetes` 软件包，可以使用 `yum` 安装。Rancher 容器管理套件有一个选项可自动部署 Kubernetes 环境。

### 使用 kubeadm 安装

`kubeadm` 工具是在 Kubernetes 1.4 中引入的。目标是提供一种在任何平台上安装 Kubernetes 的简便方法。目前，它仅支持 CentOS 7 和 Ubuntu 16.04。使用 `kubeadm` 进行安装仍然有点混乱，但这是一个令人兴奋的开始。有关更多详细信息，请参阅 http://kubernetes.io/docs/getting-started-guides/kubeadm/。

### 使用 kube-up.sh 安装

GCE 是运行 Kubernetes 最受完整支持的方式。在 GCE 上运行 Kubernetes 与在任何其他地方运行之间唯一的严重区别在于外部服务如何映射到正在运行的 Pod。GCE 还会为你完成大量繁琐的工作，例如管理防火墙设置以及向集群注册新节点：

```
$ export KUBERNETES_PROVIDER=gce; wget -O - https://get.k8s.io | bash
```

如果你不喜欢将下载的脚本直接管道到 Bash 的安全风险，或者你想更改安装选项，可以下载最新版本，解压它，然后手动运行 kube-up.sh：

```
$ cd kubernetes/cluster/
$ export KUBERNETES_PROVIDER=gce; ./kube-up.sh
```

该脚本将创建一个新的 Kubernetes Master 和若干 Worker 节点。完成后，它将在 ~/.kube/config 中创建一个 kubeconfig 文件。然后你将能够使用 kubectl 与集群交互。

kube-up.sh 还支持安装多个其他云提供商，包括 AWS、Azure 和 GKE。它还可以使用 Vagrant 安装测试集群以供本地测试。有关完整列表以及如何为你的环境调整 kube-up.sh 的说明，请参阅 http://kubernetes.io/docs/getting-started-guides/binary_release/。

### 安装附加组件

Kubernetes 支持使用附加组件来扩展集群功能。最常见的是 kube-dns 和 kube-ui 附加组件，它们分别提供 DNS-SD 和集群的图形化视图。附加组件还可以用于安装不同的工具，例如用于覆盖网络的 Weave Net。可用附加组件列表可在以下网址找到：http://kubernetes.io/docs/admin/addons/。

## 管理资源

Kubernetes 中的每个资源都定义在 JSON 或 YAML 格式的文件中。这两种格式完全可以互换。YAML 通常更易于人类阅读。可以将一些资源定义在 YAML 中，其他资源定义在 JSON 中。作为最佳实践，选择一种格式并坚持使用。




### 创建资源

所有资源至少将具有两个字段：

- apiVersion: 这是正在使用的 API 版本。随着新功能的添加，版本可能会更改。
- kind: 这是资源的类型。这会告知 Kubernetes 正在创建什么类型的资源。

每个资源可以定义在单独的文件中。在某些情况下，将资源合并到一个文件中是有价值的。例如，在与其暴露的服务同一文件中定义 ingress 对象是有意义的。在这种情况下，使用单独一行上的三个短线（---）来分隔资源。以下示例演示了如何执行此操作：

```
apiVersion: v1
kind: Service
metadata:
  name: wp-service
  labels:
    app: wordpress
spec:
  ports:
    - port: 443
      name: https
  selector:
    app: wordpress
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: wp-ingress
spec:
  backend:
    serviceName: wp-service
    servicePort: 80
```

定义资源后，通过运行 kubectl create 将其添加到 Kubernetes。-f 标志告诉 kubectl 从给定文件读取资源定义。如果文件名是短线（-），则从 stdin 读取资源：

```
$ kubectl create -f multi-deployment.yml
deployment "multi-deploy" created
```

还可以通过添加 `-R` 标志并将 `-f` 设置为目录名，来指示 `kubectl create` 读取目录中的所有文件。这样可以轻松地将特定应用程序的所有资源放在一个目录中并一次性加载它们。

Kubernetes 项目创建了一个名为 Helm 的工具，可用于打包预配置的资源。有关更多信息，请参阅 https://github.com/kubernetes/helm。

### 删除资源

资源使用 `kubectl delete` 命令删除。最简单情况下，`kubectl delete` 使用要删除的资源的类型和名称来调用：

```
$ kubectl delete service wp-service
service "wp-service" deleted
```

与 `create` 命令一样，`delete` 可以使用 `-f` 标志来传入文件。文件中定义的每个资源都将被删除。`-R` 标志也可以用于删除目录中定义的每个资源。

### 运行 Pod

Kubernetes 将容器组织为 Pod。每个 Pod 是一个或多个容器的集合。Pod 可以独立于集群中运行的其他 Pod 进行复制、缩放和更新。每个 Pod 都分配了一个 IP 地址，用于连接到 Pod 中运行的容器的公开端口。必须注意确保 Pod 中的服务不尝试使用相同的端口，但这比管理 Docker Swarm 中的端口使用要容易协调得多。

### 定义 Pod

**Pod** 定义一个或多个容器以及容器所需的所有卷。容器作为一个单元运行。它们一起启动和停止。它们也可以通过 localhost 相互通信。Pod 使用 YAML 或 JSON 定义并传递给 `kubectl create`。以下是一个简单的 Pod 定义：

```
apiVersion: v1
kind: Pod
metadata:
 name: simple
spec:
containers:
 - name: web
 image: nginx
 ports:
 - containerPort: 80
 - containerPort: 443
```

前面的 YAML 文件定义了一个运行单个 nginx 容器的简单 Pod。name 字段设置 Pod 的名称。在此示例中，Pod 名为 simple。generateName 字段可以代替 name 字段使用，它采用用于自动生成名称的前缀。如果您需要运行多个独立的 Pod 副本，这很有用。

spec 字段是定义 Pod 容器和卷的位置。containers 字段接受一个或多个容器的列表。前面的示例创建了一个名为 web 的单个容器。容器镜像通过 image 字段设置。这是容器仅有的两个必填字段。

ports 字段定义将在 Pod IP 地址上公开的端口。这是容器中的端口，而不是将要公开的端口。设置外部端口将在本章后面的服务资源中介绍。

ports 字段默认使用 TCP。协议可以设置为 UDP 作为端口的附加字段。以下代码片段显示了它的样子：

```
ports:
 - containerPort: 53
 protocol: UDP
```

Pod 还有很多内容无法在一章中全部介绍。有关完整详细信息，请参阅 Pod 规范：http://kubernetes.io/docs/api-reference/v1/definitions/#_v1_podspec。

将 Pod 规范写入文件后，可以使用 kubectl create 将其添加到 Kubernetes：

```
$ kubectl create -f simple-pod.yml
pod "simple" created
```

## 查看 Pod

运行 `kubectl get pods` 可获取运行中的 Pod 列表。添加 `-o wide` 将显示 Pod 的 IP 地址以及 Pod 所在的节点：

```
$ kubectl get pods
NAME        READY     STATUS    RESTARTS   AGE
simple      1/1       Running   0          12s
$ kubectl get pods -o wide
NAME        READY     STATUS    RESTARTS   AGE       IP            NODE
simple      1/1       Running   0          1m        10.246.46.5   kubernetes-node-1
```

Pod 的详细信息可使用 `kubectl describe pod` 命令获取。关于运行中 Pod 的所有所需信息都显示在 `describe` 的输出中。以下命令显示名为 `simple` 的 Pod 的详细信息：

```
$ kubectl describe pod simple
Name:               simple
Namespace:          default
Node:               kubernetes-node-1/10.245.1.3
Start Time:         Wed, 05 Oct 2016 18:55:46 -0600
Labels:             <none>
Status:             Running
IP:                 10.246.46.5
Controllers:        <none>
Containers:
  web:
  Container ID:     docker://6460c3565b2c0d7cb356f0da2ea5e12f58017e881fb91176eaafb65b5a46bc31
  Image:            nginx
  Image ID:         docker://sha256:ba6bed934df2e644ffdd34e9d324c80f3c615544ee9a93e4ce3cfddfcbf84bdbdc2
  Ports:            80/TCP, 443/TCP
  Requests:
    cpu:            100m
  State:            Running
  Started:          Wed, 05 Oct 2016 18:55:48 -0600
  Ready:            True
  Restart Count:    0
  Volume Mounts:    /var/run/secrets/kubernetes.io/serviceaccount from default-token-dlmur (ro)
  Environment Variables: <none>
Conditions:
  Type              Status
  Initialized       True
  Ready             True
PodScheduled  True
Volumes:
 default-token-dlmur:
  Type:  Secret (a volume populated by a Secret)
  SecretName:  default-token-dlmur
QoS Class:  Burstable
Tolerations:  <none>
Events:
  FirstSeen    LastSeen    Count   From                   SubobjectPath  Type      Reason          Message
  ---------    --------    -----   ----                   --------------  --------  ------          -------
  7s           7s          1       {default-scheduler }                      Normal   Scheduled    Successfully assigned simple to kubernetes-node-1
  ...
```

## 环境变量

许多容器使用环境变量来配置其运行方式。`mysql` 镜像使用环境变量来初始化数据库并设置用户密码。`wordpress` 镜像使用环境变量来配置 MySQL 数据库的位置，并在配置中设置相应的凭据。

以下是一个运行 `mysql` 镜像并设置两个环境变量的 Pod 示例。第一个，`MYSQL_ROOT_PASSWORD`，设置 root 用户的密码。第二个，`MYSQL_DATABASE`，配置容器创建名为 `wordpress` 的数据库：

```
apiVersion: v1
kind: Pod
metadata:
  name: mysql
  labels:
    app: mysql
spec:
  containers:
  - name: db
    image: mysql:5.7
    ports:
    - containerPort: 3306
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: password
    - name: MYSQL_DATABASE
      value: wordpress
```

## 多容器 Pod

可以定义运行多个容器的 Pod。在将旧版应用程序迁移到 Docker 和 Kubernetes 时，这尤其有用。

Pod 的一大好处是，Pod 中的每个容器都可以通过 localhost 连接到 Pod 中的任何其他容器。这使得将旧版应用程序迁移到 Kubernetes 变得相当简单。使用独立 Docker 时，每个容器都分配了自己的随机 IP 地址。除非安装了 DNS-SD 工具，否则容器可能很难相互发现。更糟糕的是，由于每个容器都有不同的 IP，必须使用单独的组件（例如 HAProxy）将它们统一在一个 IP 地址下供最终用户使用。

例如，假设你有一个带 Web 界面和 SFTP 服务器的 Samba 文件服务器，用于异地访问。每个服务都通过 localhost 与其他服务通信，并可从相同的 IP 地址和主机名访问。无论用户希望使用哪个服务来访问其文件，都可以指示用户连接到 myfiles.example.com，这非常方便。

通过将这些服务合并到一个 Pod 中，该应用程序几乎可以像在单个主机上一样处理。它们仍然通过 localhost 相互通信，并且仍然可以从单个 IP 地址访问。稍后，在解耦组件并将其变为多个 Pod 的过程中，它们仍然可以统一在一个服务下。该服务被分配了单个 IP 地址，主机名可以映射到该 IP 地址。服务将在本章后面介绍。

让我们来看一个启动带有 MySQL 数据库的 WordPress 的多容器 Pod 示例。此示例创建名为 multi 的 Pod。这一次，containers 字段列出了两个容器。第一个名为 wordpress，运行官方 wordpress 镜像。第二个名为 db，运行 mysql 镜像：

```
apiVersion: v1
kind: Pod
metadata:
  name: multi
  labels:
    app: wordpress
spec:
  containers:
  - name: wordpress
    image: wordpress
    ports:
    - containerPort: 80
    - containerPort: 443
    env:
```



```
- name: WORDPRESS_DB_HOST
  value: 127.0.0.1
- name: WORDPRESS_DB_USER
  value: wordpress
- name: WORDPRESS_DB_PASSWORD
  value: password
- name: db
  image: mysql
  env:
    - name: MYSQL_ROOT_PASSWORD
      value: password
    - name: MYSQL_DATABASE
      value: wordpress
    - name: MYSQL_USER
      value: wordpress
    - name: MYSQL_PASSWORD
      value: password
```

需要注意的是，本示例有几点：

- 首先，`wordpress` 容器配置为在 `127.0.0.1`（即本地主机）上查找 MySQL。
- 其次，`db` 容器未定义任何端口。这是因为数据库不需要在 pod 的 IP 地址上暴露。

唯一能连接到数据库的容器是同一个 pod 中的容器。

## 使用密钥

上一节中的 WordPress 示例非常精彩，但所有注重安全的管理员可能都会因 pod 定义中的明文密码而心脏病发作。好消息是，Kubernetes 使将应保密的内容分离到特殊 `Secret` 类型中成为可能。密钥可用于将密码等内容排除在 pod 定义之外，也可用于在不同命名空间中提供不同的密码、SSL 证书或 SSH 密钥，而无需更改 pod 定义。

## 创建密钥

密钥是经过 base64 编码的字符串。在 Linux 上，可以使用 base64 工具对字符串进行编码。字符串编码后，可以将其添加到定义密钥的 YAML 或 JSON 文件中：

```
$ echo -n "root-password" | base64
cm9vd2dlC1wYXNzd29yZA==
```

以下示例为前面的 WordPress 应用程序的 root 和 wordpress 用户定义了两个密码：

```
apiVersion: v1
kind: Secret
metadata:
  name: mysql-passwords
type: Opaque
data:
  root: cm9vd2dlC1wYXNzd29yZA==
  wordpress: d29yZHByZXNzTmV3LXhBcmN3N3J3J3Jk
```

YAML 定义了一个名为 mysql-passwords 的新密钥。密钥位于 data 部分。root 和 wordpress 键将用于从节点定义中引用密码。

可以使用 `kubectl describe secret` 显示密钥。呃，几乎可以。会显示带有关联标签和密钥列表的密钥对象，但密钥本身被隐藏了：

```
$ kubectl describe secret mysql-passwords
Name:         mysql-passwords
Namespace:    default
Labels:       <none>
Annotations:  <none>
Type:  Opaque
Data
====
wordpress:  18 bytes
root:       13 bytes
```

需要注意的是，未加密的密钥相当容易获取。使用 `kubectl describe secret` 描述密钥时，它们会被隐藏，但拥有适当访问权限的用户可以登录使用密钥的正在运行的容器并将其打印出来。不要依赖密钥来保护密码和其他敏感信息。有关更多详细信息，请参阅 http://kubernetes.io/docs/user-guide/secrets/。

### 在环境变量中使用密钥

将密钥导入容器有两种方法。第一种是通过环境变量。以下是再次给出的 WordPress pod 定义。这次密码是从 mysql-passwords 密钥对象中检索的。`key` 选项指定使用 mysql-passwords 密钥对象中的哪个密钥。相关部分已突出显示。在容器中，`*_PASSWORD` 环境变量将包含解码后的值：

```
apiVersion: v1
kind: Pod
metadata:
  name: multi-secrets
  labels:
    app: wordpress
spec:
  containers:
  - name: wordpress
    image: wordpress
    ports:
    - containerPort: 80
    - containerPort: 443
    env:
    - name: WORDPRESS_DB_HOST
      value: 127.0.0.1
    - name: WORDPRESS_DB_USER
      value: wordpress
    - name: WORDPRESS_DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: mysql-passwords
          key: wordpress
  - name: db
    image: mysql
    env:
    - name: MYSQL_ROOT_PASSWORD
      valueFrom:
        secretKeyRef:
          name: mysql-passwords
          key: root
    - name: MYSQL_DATABASE
      value: wordpress
    - name: MYSQL_USER
      value: wordpress
    - name: MYSQL_PASSWORD
      valueFrom:
        secretKeyRef:
          name: mysql-passwords
          key: wordpress
```

### 在文件中使用密钥

另一种选择是将密钥作为文件在容器中提供。这是通过 **卷** 完成的。将创建一个卷，其中包含以密钥命名的文件中的解码密钥值。这对于将 SSL 证书和 SSH 密钥添加到容器很有用。应用程序可以配置为使用挂载的密钥卷，或者一个小的包装器脚本可以将文件复制到应用程序可以找到的位置。

以下是运行 `nginx` 的简单 pod。它创建了一个挂载到容器中 `/etc/secrets` 的卷。此示例使用与前面示例相同的 `mysql-passwords` 密钥：

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-secrets
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: secrets
      mountPath: "/etc/secrets"
      readOnly: true
  volumes:
  - name: secrets
    secret:
      secretName: mysql-passwords
```

在容器中，解码后的密钥在 `/etc/secrets` 中可用。每个密钥都存储在卷中的文件中：

```
### cd /etc/secrets/
### ls
root wordpress
### cat root
root-password
```

## 登录到容器

登录到容器以排查问题通常是必要的。与 `docker exec` 相同的功能在 Kubernetes 中作为 `kubectl exec` 提供。如预期，`kubectl exec` 略有不同，以考虑 pod 的概念：

```
$ kubectl exec multi-secrets -c wordpress -it bash
root@multi-secrets:/var/www/html#
```

此示例在 `multi-secrets` pod 的 `wordpress` 容器中启动一个 bash shell。`-i` 和 `-t` 标志的工作方式与 `docker exec` 相同，它们指示 `exec` 从 stdin 获取输入并创建一个新的 TTY。

如果要运行的命令带有标志，可以在命令前加上两个破折号 (--) 。以下示例在 `multi-secrets` pod 的 `db` 容器中打开一个 MySQL shell：

```
$ kubectl exec multi-secrets -c db -it -- mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.15 MySQL Community Server (GPL)
Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights
reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input
statement.
mysql>
```

如果 pod 只有一个容器，或者要连接到 pod 中的第一个容器，可以省略 `-c` 标志。即使 pod 只有一个容器，也可以使用 `-c` 以避免混淆。

### 删除 pod

当不再需要 pod 时，可以使用 `kubectl delete pod` 将其删除。以下示例删除名为 `simple` 的 pod：

```
$ kubectl delete pod simple
pod "simple" deleted
```

## 使用部署

**部署**是具有一个或多个副本的 pod 集合。它负责确保请求的 pod 数量正在运行并在必要时重新启动它们。部署使扩展 pod 和稍后更新它们变得容易。部署取代了 `ReplicationController` 资源。

### 创建单容器部署

Docker 的乐趣之一是能够快速轻松地启动容器。Kubernetes 可能很复杂，但启动单个容器仍然很简单：

```
$ kubectl run web --image=nginx:1.11 --replicas=2 --port=80
deployment "web" created
```

前面的命令创建了一个名为 `web` 的新部署，运行两个 `nginx` 镜像。`--replicas` 标志可用于设置所需的副本数量。如果省略，只启动一个 pod。`--port` 标志告诉 Kubernetes 公开端口 80。`--port` 标志可以多次使用以公开其他端口。

`kubectl get deployments` 命令将显示集群中所有当前部署。在此示例中，它显示两个 pod 副本均在运行、最新且可用：

```
$ kubectl get deployments
NAME DESIRED CURRENT UP-TO-DATE AVAILABLE AGE
web 2 2 2 2 12s
```

可以使用 `kubectl get pods` 命令获取正在运行的 pod 列表。`-l` 标志将显示限制为带有给定标签的 pod。使用 `run` 命令时，pod 会自动获得 `run=<name>` 标签，其中 `<name>` 是部署的名称。要将列表限制为仅来自名为 `web` 的 `run` 的 pod，请使用以下命令：

```
$ kubectl get pods -l run=web
NAME READY STATUS RESTARTS AGE
web-4291803130-haisr 1/1 Running 0 13m
web-4291803130-j7y51 1/1 Running 0 13m
```

两个副本均在运行并准备好提供网页服务。





### 创建多容器部署

多容器部署要求部署定义在 YAML 或 JSON 中。该文件指定部署名称、副本数量和 pod 规范。此相同格式也可用于单容器部署：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: multi-deploy
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: web-deploy
    spec:
      containers:
        - name: nginx
          image: nginx:1.10.4
        - name: mysql
          image: mysql:5.5
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: password
```

该示例创建带有两个容器（nginx 和 mysql）的两个 pod。Pod 在部署定义的 template 部分中定义。它的工作方式与本章已介绍的 pod 相同。最大的区别是省略了 pod 名称。Pod 名称将基于部署名称。

请注意 apiVersion。Deployment 是一个新功能，官方仍处于 beta 阶段。规范在后续版本中可能会更改。

## 扩缩部署

Kubernetes 使得使用 scale 命令轻松增加或减少部署中的副本数量。以下命令将名为 web 的部署扩缩至三个副本：

```
$ kubectl scale --replicas=3 deployment/web
deployment "web" scaled
```

部署可以根据需要随意扩缩。调度器将确保在集群中启动所需的 pod。

## 滚动更新

Kubernetes 中的 Deployment 具有一个功能，允许部署中的所有 pod 自动更新。每当部署模板部分的 pod 规范的一部分发生更改时，都会触发更新。

### 更新部署

请考虑以下包含两个容器的部署。第一个名为 web，是 nginx:1.10.1 镜像。第二个名为 db，是 mysql:5.5 镜像：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: update-deploy
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: update-deploy
    spec:
      containers:
        - name: web
          image: nginx:1.10.1
        - name: db
          image: mysql:5.5
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: password
```

两个容器都在运行其镜像的较旧版本。让我们先升级 web 容器：

```
$ kubectl set image deployment/update-deploy web=nginx:1.11.4
deployment "update-deploy" image updated
```

此命令将 update-deploy 部署中 web 容器的 image 属性设置为 nginx:1.11.4。运行该命令后立即，Kubernetes 会启动带有更新镜像的新 pod。新 pod 就绪后，会停止一个现有 pod。此过程重复，直到所有副本都更新。

警告。在更新部署之前，请确保您的容器可以安全地多次运行。默认情况下，Kubernetes 执行更新所使用的机制与扩缩 pod 相同。如果您的应用程序无法运行多个副本，可以在部署定义中将更新策略类型设置为 Recreate。Kubernetes 将在启动新 pod 之前停止现有 pod。以下摘录显示如何设置部署策略：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: update-deploy
spec:
  strategy:
    type: "Recreate"
...
```

### 获取滚动更新历史

Kubernetes 会记住对部署所做的更改。可以通过运行 `kubectl rollout history deploy/<name>` 命令查看修订历史记录，其中 `<name>` 是部署的名称：

```
$ kubectl rollout history deployment/update-deploy
deployments "update-deploy"
REVISION          CHANGE-CAUSE
1                 <none>
2                 <none>
```

向历史命令添加 `--revision=N`（其中 N 是修订号）即可查看部署的完整详细信息。在以下输出中，您可以看到 `web` 容器的镜像已更新为 `nginx:1.11.4`：

```
$ kubectl rollout history deployment/update-deploy --revision=2 deployments
"update-deploy" with revision #2
Labels:         app=update-deploy
                pod-template-hash=1680774371
Containers:
web:
  Image:        nginx:1.11.4
  Port:
  Volume Mounts: <none>
  Environment Variables: <none>
db:
  Image:        mysql:5.5
  Port:
  Volume Mounts: <none>
  Environment Variables:
  MYSQL_ROOT_PASSWORD:  password
No volumes.
```

### 回滚更新

您可能会发现，更新后出了严重问题，需要回滚更新。幸运的是，有一个非常方便的 `undo` 命令：

```
$ kubectl rollout undo deploy/update-deploy
deployment "update-deploy" rolled back
```

不带选项时，该命令会将指定的部署回滚到历史记录中的上一个版本。`--to-revision=N` 标志会将部署回滚到历史记录中的修订版本 `N`。

### 删除部署

请记住，部署的作用是确保运行所需数量的 pod。尝试通过删除 pod 来删除部署，就像西西弗斯试图将石头推上山一样。Kubernetes 会以删除 pod 的同样速度重新创建它们。当不再需要部署时，可以使用 `kubectl delete deployment` 将其删除。请确认您要继续，因为这将删除部署中所有运行的 pod：

```
$ kubectl delete deployment web
deployment "web" deleted
```

可以使用 `--cascade=false` 标志删除部署而保留 pod 运行。剩余的 pod 需要手动删除。

## 使用 DaemonSets 在每个节点上运行 pod

**DaemonSet** 是一种资源类型，用于确保 pod 或一组 pod 在集群中的每个节点上运行。该资源的格式与部署非常相似。`template` 部分是一个 pod 规范，与部署一样：

```
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: daemon
spec:
  template:
    ...
```

DaemonSets 适用于运行日志记录或监控等服务。例如，您可能有类似 collectd 的服务，用于将性能数据发送到监控解决方案。

还可以将 DaemonSet 限制为仅在与给定选择器匹配的一组节点上运行。例如，您的本地 Kubernetes 集群可能混合了 HP 和 Dell 的服务器硬件。您可以创建一个 DaemonSet，在硬件标签设置为 dell 的节点上运行 Dell OpenManage 工具：

```
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: omsa
spec:
  template:
    spec:
      nodeSelector:
        hardware: dell
...
```

## 介绍定时作业

Kubernetes 1.4 引入了定时作业。这些是按计划运行的一组容器。它们类似于 Linux 中的 Cron 作业，甚至在规范中使用 Cron 时间格式。定时作业仍被视为 alpha 版本，除非在 API 服务器启动时添加 `--runtime-config=batch/v2alpha1` 标志，否则不可用。有关详细信息，请参阅 http://kubernetes.io/docs/user-guide/jobs/。

## 使用标签

Kubernetes 中的几乎每个资源都可以分配标签。标签是一种组织资源的方式，但不会改变它们的操作方式。它们完全是任意的，应选择以适应您的环境。例如，您可能希望按应用程序对资源进行分组，标记当前正在测试的 pod，或指示某些 pod 只能在前端节点上运行。

标签在资源的 `metadata` 部分中设置。它们是一系列键/值对。在以下示例中，pod 设置了四个标签。`app` 标签用于表示该 pod 是 `wordpress` 应用程序的一部分。`tier` 标签表示该 pod 应在后端节点上运行。该 pod 仍在测试中，因此 `env` 标签设置得当。最后，此示例中的 `hero` 标签用于展示标签的任意性：

```
apiVersion: v1
kind: Pod
metadata:
  name: mysql
  labels:
    app: wordpress
    tier: backend
    env: testing
    hero: darkwing-duck
...
```

标签本身不执行任何操作。创建资源时，Kubernetes 会记录标签并开始在集群中的某个位置调度资源。

### 使用选择器查找资源

列出资源的所有工具都可以带 `-l` 标志使用选择器，以将列表限制为具有匹配标签的资源。本章前面已使用此方法查看由部署启动的 pod 列表：

```
$ kubectl get pods -l 'app=update-deploy'
NAME READY STATUS RESTARTS AGE
update-deploy-1680774371-9x8qd 2/2 Running 0 23h
update-deploy-1680774371-y0tbf 2/2 Running 0 23h
```

此示例列出所有带有 `app` 键和 `update-deploy` 值的 pod。可以使用几种不同的比较运算符。将选择器用单引号括起来，以确保 shell 正确处理空格和特殊字符：

| 运算符 | 描述 | 示例 |
|---|---|---|
| =, == | 键的值必须匹配。 | `env = production` |
| != | 键的值必须不匹配。 | `env != production` |
| in | 值在一组值中。 | `env in (testing, development)` |





| 操作符 | 描述 | 示例 |
|---|---|---|
| notin | 值不在一组值中。 | env notin (testing, development) |
| label | 资源是否有该标签？值被忽略。 | app |
| !label | 没有给定标签的资源。值被忽略。 | !app |
| ,（逗号） | 连接多个查询。所有查询都必须匹配。 | env in (production, testing), app=website |

Kubernetes 中有一些资源使用标签。例如，服务资源会将与选择器匹配的 Pod 分组，以实现负载均衡和外部访问。每当创建与服务所查找内容匹配的 Pod 时，它会自动被添加到该组中。

> 有关使用标签和选择器的完整详细信息，请参阅 http://kubernetes.io/docs/user-guide/labels/。

## 使用命名空间

Kubernetes 使用 **命名空间** 作为运行中的 Pod 组及其关联卷、服务和控制器之间的逻辑分隔。默认情况下，Kubernetes 有两个命名空间：

- kube-system：用于运行 Kubernetes 服务和插件的容器
- default：其他所有内容都运行在此处

命名空间可用于为开发和生产环境、两个业务单元甚至多个客户提供不同的环境。还可以为每个命名空间定义资源限制。

### 查看命名空间

可以列出当前配置的命名空间。使用 kubectl get namespaces 命令：

```
$ kubectl get namespaces
NAME		STATUS	AGE
default		Active	3h
kube-system	Active	3h
```

可以使用 kubectl describe namespace 显示特定命名空间的详细信息。这包括命名空间的总配额以及各个实体的资源限制：

```
$ kubectl describe namespace default
Name: default
Labels: <none>
Status: Active
No resource quota.
Resource Limits
Type Resource Min Max Default Request Default
Limit Max Limit/Request Ratio
-------------------------
Container cpu - - 100m -
-
```

### 创建命名空间

新命名空间在 YAML 文件中定义。此示例创建名为 production 的命名空间：

```
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

然后将命名空间定义文件传递给 kubectl create：

```
$ kubectl create -f production-ns.yml
namespace "production" created
```

### 切换命名空间

创建命名空间后，可以对该命名空间执行操作。对于一次性更改，可以将 `--namespace` 选项传递给 `kubectl`：

```
$ kubectl --namespace=kube-system get pods
NAME                              READY   STATUS    RESTARTS   AGE
heapster-v1.2.0-3649594813-4zdoy   0/4     Pending   0          4h
kube-dns-v19-zmals                  3/3     Running   0          4h
kube-proxy-kubernetes-node-1        1/1     Running   0          3h
kubernetes-dashboard-v1.4.0-pbhe2   1/1     Running   0          4h
monitoring-influxdb-grafana-v4-zc7x4 2/2   Running   0          4h
```

如果计划在一个命名空间中做大量工作，最好创建一个上下文。此示例创建一个名为 `production` 的上下文，它使用 `production` 命名空间：

```
$ kubectl config set-context production --namespace=production
context "production" set.
```

如果集群和用户名不同，可以使用 `--cluster` 和 `--user` 标志为上下文设置集群和用户名。设置上下文后，需要时可以轻松切换到它。以下示例切换到 `production` 上下文。现在，使用 `kubectl` 运行的每个命令都将在 production 命名空间中：

```
$ kubectl config use-context production
switched to context "production".
```

### 删除命名空间

命名空间可以是临时沙箱，也可以是长期运行的环境。无论你使用命名空间做什么，可能都需要删除一个。以下命令将删除 `production` 命名空间：

```
$ kubectl delete namespace production
namespace "production" deleted
```

在删除命名空间之前，请深呼吸。这样做会销毁该命名空间中的所有内容，包括 Pod、服务和卷。在删除之前，请确保命名空间中没有任何重要内容。

## 网络

Kubernetes 中的网络与直接使用 Docker 不同，IP 地址是按 Pod 而非按容器分配的。这允许 Pod 内的容器通过 localhost 相互连接。它还允许它们作为一个服务运行。这与传统服务器模型更为相似，而大多数 Docker 工具则不然。此外，Kubernetes 为应用程序提供自动负载均衡工具，并可以与大多数云提供商合作，使用其现有的负载均衡器提供外部访问。

Kubernetes 需要使用覆盖网络来允许 Pod 在集群之间通信。它支持 GCE、Weave、Flannel、OpenVSwitch 等，无论是开箱即用还是通过插件。DNS-SD 也可以通过 kube-dns 插件开箱即用，但可以使用其他服务代替。

### 服务

Kubernetes 为每个 Pod 分配一个 IP 地址，但每次重启 Pod 时该地址都会更改。此外，分配给 Pod 的 IP 是私有地址，集群外部无法访问。如果要使应用程序公开，这两者都没有帮助。解决方案是服务。

服务提供负载均衡，并在服务存在期间维护静态 IP 地址。Pod 可以重启、更新或缩放，服务将跟踪它们并适当路由流量。这是通过选择器实现的。

### 创建服务

如前所述，Pod 和 Deployment 可以分配标签。服务使用这些标签来确定将流量发送到哪些 Pod。让我们来看一个示例：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: wp
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress
        ports:
          - containerPort: 80
          - containerPort: 443
...
```

这是用于 WordPress 安装的精简 Deployment。请注意，标签 `app` 已设置为 `wordpress`。现在，让我们看看此应用程序的服务：

```
apiVersion: v1
kind: Service
metadata:
  name: wp-service
  labels:
    app: wordpress
spec:
  type: LoadBalancer
  ports:
    - port: 80
      name: http
    - port: 443
      name: https
  selector:
    app: wordpress
```

这里有几点需要注意。`ports` 部分列出了此服务将暴露的所有端口。如果端口与使用 `containerPort` 设置的端口匹配，则无需其他操作。否则，可以将 `targetPort` 字段添加到 `port` 条目中，指定容器正在监听的端口。以下示例展示如何将服务中的端口 80 发送到 Pod 中的端口 5000：

```
ports:
  - port: 80
    name: http
    targetPort: 5000
```

`selector` 字段是配置服务以了解要为哪些 Pod 提供服务的地方。在此示例中，服务正在查找 `app` 标签设置为 `wordpress` 的 Pod 或 Deployment。命名空间中出现的任何与选择器匹配的新 Pod 都会自动添加到服务中。

> 确保服务的选择器不重叠。重叠将是 *糟糕* 的。如果重叠，将会发生争用，并且你的应用程序可能会出现在多个服务中。例如，你的 WordPress 服务可能会突然显示你的会计应用程序。

Type 字段告诉 Kubernetes 如何暴露此服务：

- **ClusterIP**：这是默认类型。它从预配置池中为服务分配一个 IP。该地址几乎总是私有的，仅可从集群内部访问。如果你的服务只为 Kubernetes 中的其他应用程序提供服务，请使用此类型。
- **NodePort**：这会在集群中的每个节点上分配一个 30,000 到 32,767 之间的随机端口。发送到该节点的任何请求都将转发到服务针对的 Pod。可以在端口条目上使用 `nodePort` 选项设置特定端口。该端口必须仍在配置范围内。如果你想运行自己的负载均衡器，`NodePort` 类型很有用。
- **LoadBalancer**：这将启动云特定的负载均衡器来处理服务的流量。

服务的内容远多于此处所能涵盖的。有关更多详细信息，请参阅 `http://kubernetes.io/docs/user-guide/services/`。

### 列出服务

运行 `kubectl get service` 可获取服务列表。列表将显示服务、集群 IP 和外部 IP 地址（如果已分配）：

```
$ kubectl get service
NAME            CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
kubernetes      10.3.240.1    <none>        443/TCP          2h
wp-service      10.3.242.67   104.154.253.227 80/TCP, 443/TCP  1h
```

还可以运行 `kubectl describe service` 并指定服务名称来获取服务的详细描述。描述包括集群 IP、端口列表和入口端口：

```
$ kubectl describe service wp-service
Name:                     wp-service
Namespace:                default
Labels:                   app=wordpress
Selector:                 app=wordpress
Type:                     LoadBalancer
IP:                       10.3.242.67
LoadBalancer Ingress:     104.154.253.227
Port:                     http    80/TCP
NodePort:                 http    31342/TCP
Endpoints:                10.0.2.3:80
Port:                     https   443/TCP
```





NodePort:           https 30763/TCP
Endpoints:         10.0.2.3:443
Session Affinity:  None
No events.

### 介绍 DNS-SD

Kubernetes 自带一个名为 kube-dns 的插件，用于提供 DNS-SD。在 Kubernetes 的近期版本中，此功能默认启用。服务会被分配名称。预期如果 Pod 需要相互通信，它们将通过服务来进行。

生成的主机名采用 `service_name.namespace.svc.cluster.local` 形式，其中 `service_name` 是服务的名称，`namespace` 是服务所在的命名空间。例如，在 `prod` 命名空间中名为 `wordpress` 的服务将拥有一条 `wordpress.prod.svc.cluster.local` 的 A 记录。

## 使用卷

与原生 Docker 类似，Kubernetes 支持卷。主要区别在于 Kubernetes 在 Pod 级别支持卷。这意味着在 Pod 中配置的卷对 Pod 中的每个容器均可用，并可用于读写。其次，Kubernetes 卷可同时使用多种不同类型的网络存储。

卷分为两种形式，第一种是 **临时卷**，它仅在使用它的 Pod 存在期间保留。当 Pod 被删除时，卷也会随之删除。第二种是 **持久卷**，即使 Pod 被删除，它也能持久保留数据。

### 使用普通卷

Kubernetes 中的卷行为与原生 Docker 中的卷非常相似。它们预计仅在使用它们的 Pod 存在期间保留。Pod 中的每个容器都可以挂载该卷，并且可以在相同或不同的位置挂载。唯一限制是卷不能挂载在从其他卷挂载的文件系统上。换句话说，卷不能嵌套：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
spec:
  volumes:
    - name: test
      gcePersistentDisk:
        pdName: test-pd
        fsType: ext4
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - mountPath: /mnt
          name: test
```

这是一个使用名为 test-pd 的 GCE 持久磁盘的简单 Pod。卷在 volumes 部分定义。可以在那里定义所需数量的卷。nginx 容器将卷挂载在 /mnt。volumeMounts 中的名称必须与 volumes 中的名称匹配。

卷的选项将根据磁盘类型而有所不同。请注意，许多类型的网络存储必须在作为卷使用之前已存在。有关更多详细信息，请参阅 http://kubernetes.io/docs/user-guide/volumes/。

### 在持久卷中存储长期数据

顾名思义，持久卷是持久的。它们设计为在 Pod 被删除后仍然保留，并可由另一个 Pod 再次使用。通常，数据库及类似系统使用持久卷来存储数据。无论出于何种原因（例如更新后）重启 Pod，数据都会在那里等待它。

持久卷分为两部分。第一部分是 PersistentVolume 对象，它存储正在使用的物理存储的连接详细信息。第二部分是 PersistentVolumeClaim，它告诉 Kubernetes 将来会有某物想要使用该存储。带有声明的 Pod 基本上有权使用那块存储。

你可以将其类比为剧院的门票。PersistentVolume 对象是剧院中的一个座位。PersistentVolumeClaim 对象是允许进入剧院并坐在座位上的门票。

持久卷还允许一定程度地将卷与 Pod 抽象分离。PersistentVolume 对象存储访问物理存储的所有详细信息。Pod 无需关心这些信息。它只需使用 PersistentVolumeClaim 对象来访问存储。

### 创建 PersistentVolume

让我们先创建一个 `PersistentVolume` 对象。它使用与前面卷示例相同的 GCE 持久磁盘：

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: test-pd
  labels:
    name: test-pd
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  gcePersistentDisk:
    pdName: test-pd
    fsType: ext4
```

`spec` 部分中发生的有趣事情如下：

- 如果一个 Pod 对已经有写入声明的持久卷声明写入访问权限，则第二个声明将被拒绝。如果尝试对仅允许只读访问的卷获取写入访问权限，声明也会被拒绝。
- 接下来，`accessModes` 决定 Pod 允许声明的访问类型。继续使用剧院类比，这就像说 Pod 可以在主楼层或阳台获得门票。可设置三种模式：
  - ReadWriteOnce：仅单个节点可以以读写方式挂载此存储
  - ReadOnlyMany：多个节点可以以只读方式挂载此存储
  - ReadWriteMany：多个节点可以以读写方式挂载此存储
- 首先，定义存储容量；在此示例中为 10 GiB。可以预先创建多个不同大小的存储块，并让 Pod 声明其所需大小。
- 第三是 `persistentVolumeReclaimPolicy`。此设置决定在声明释放时对卷执行什么操作。有三个可能的值。请注意，并非每种存储类型都支持每种回收策略：
  - Retain：对卷不执行任何操作。如果不再需要卷，由集群管理员将其删除。
  - Recycle：物理存储保留，但 Kubernetes 会在再次使其可用之前对卷尝试 `rm -rf *`。
  - Delete：当声明释放时，卷将被删除。

最后一个字段配置存储。在此示例中，为名为 `test-pd` 的 GCE 持久磁盘。这将根据正在使用的存储而有所不同。

### 创建 PersistentVolumeClaim 资源

在任何 Pod 可以使用 `PersistentVolume` 资源之前，必须先创建 `PersistentVolumeClaim` 资源：

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test-pd-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  selector:
    matchLabels:
      name: test-pd
```

此示例创建一个名为 `test-pd-pvc` 的声明。`accessModes` 规范告诉 Kubernetes 此声明仅用于此 Pod 的写入访问权限。`resources` 部分表示此声明正在查找至少 10 GiB 的卷。实际映射到此声明的卷可能更大，但不会更小。最后，`selector` 匹配 `PersistentVolume` 上的标签。在此示例中，声明正在查找名称标签为 `test-pd` 的持久卷。

声明可能匹配多个卷。如果是这种情况，Kubernetes 确定提供的最接近匹配的卷将被使用。如果你试图连接到特定卷，请在持久卷上使用声明可以匹配的唯一标签。

### 将 PersistentVolumeClaim 用作卷

当 Pod 想要使用持久卷时，声明会列在 Pod 规范的 volumes 部分中。以下是之前的 nginx Pod。这次，它使用 PersistentVolumeClaim 来访问卷：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-pvc
spec:
  volumes:
    - name: test-pd
      persistentVolumeClaim:
        claimName: test-pd-pvc
  containers:
    - name: nginx
      image: nginx:1.11
      volumeMounts:
        - mountPath: /mnt
          name: test-pd
```

卷类型是 persistentVolumeClaim。claimName 属性必须与先前创建的 PersistentVolumeClaim 匹配。与之前的普通卷一样，Pod 中的卷名称（此处为 test-pd）在容器定义中用于将存储挂载到容器中。

![](img/02bead9c8ff4515f78dac0eb33163961_144_0.png)

> 使用 PersistentVolumes 还可以做很多其他事情，包括为 Pod 动态配置卷。有关更多详细信息，请参阅：http://kubernetes.io/docs/user-guide/persistent-volumes/。




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
