

NodePort:           https 30763/TCP
Endpoints:         10.0.2.3:443
Session Affinity:  None
No events.

## 介绍 DNS-SD

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

