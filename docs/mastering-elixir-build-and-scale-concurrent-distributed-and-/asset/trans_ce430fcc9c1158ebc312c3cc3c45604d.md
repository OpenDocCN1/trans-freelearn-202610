
```
  - name: GOOGLE_STORAGE_CREDENTIALS
    valueFrom:
      secretKeyRef:
        name: google-storage-credentials
        key: base64-credentials
  - name: DB_USER
    valueFrom:
      secretKeyRef:
        name: cloudsql-postgres-credentials
        key: username
  - name: DB_PASS
    valueFrom:
      secretKeyRef:
        name: cloudsql-postgres-credentials
        key: password
```

还记得我们说过同一个 Pod 中的容器共享相同的 IP 和端口空间吗？这就是为什么我们告诉在 `elixir-drip-prod` 容器中运行的 ElixirDrip 应用连接到在 *本地* 端口 5432 上运行的数据库的原因，尽管这是运行在同一 Pod 中另一个容器里的 Cloud SQL 代理：

```
$ cat deploy/elixir-drip-deployment-prod.yml
apiVersion: extensions/v1beta1
kind: Deployment
  # ...
template:
  # ...
spec:
  volumes:
    - name: cloudsql-proxy-creds-volume
      secret:
        secretName: cloudsql-proxy-credentials
  containers:
    - image: gcr.io/cloudsql-docker/gce-proxy:1.11
      name: cloudsql-proxy
      command: ["/cloud_sql_proxy",
                "--instances=intense-talent-188323:europe-west1:elixir-drip-production=tcp:5432",
                "--credential_file=/secrets/cloudsql/credentials.json"]
  ports:
    - name: postgres
      containerPort: 5432
  volumeMounts:
    - name: cloudsql-proxy-creds-volume
      mountPath: /secrets/cloudsql
      readOnly: true
```

Cloud SQL 代理容器使用 Google 提供的 Docker 镜像，并通过传递 /secrets/cloudsql/credentials.json 凭据文件来启动代理。该文件存在是因为我们在 /secrets/cloudsql 路径中将 cloudsql-proxy-creds-volume 挂载到容器内部，并且我们在创建 secret 时说过 cloudsql-proxy-credentials secret 只有一个 credentials.json 文件。

在我们能够部署 Pod 之前，我们需要在生产数据库中运行项目的数据库迁移。如果不运行它们，ElixirDrip 应用在容器启动时会失败，因为它期望找到具有特定表和关系的数据库。

为了解决这个问题，我们将定义并稍后创建一个名为 elixir-drip-migrations 的 Kubernetes Job，负责运行迁移。

## 创建 Kubernetes Job

我们的 elixir-drip-migrations Job 配置文件将与之前的 Deployment 配置非常相似，但我们不会运行容器的默认命令（即在 Dockerfile.deploy 上设置的 sh -c bin/$APP_NAME foreground），而是覆盖应用程序容器启动时运行的命令（即 sh -c bin/$APP_NAME migrate_up）。我们还告诉 Kubernetes 永不重启这类 Job，并且无论如何都应在 300 秒后终止该 Job：

```
$ cat deploy/elixir-drip-migrations-prod.yml
apiVersion: batch/v1
kind: Job
metadata:
  generateName: elixir-drip-migrations
  namespace: production
  labels:
    env: production
spec:
  activeDeadlineSeconds: 300
  template:
    metadata:
      labels:
        env: production
    spec:
      restartPolicy: Never
      volumes:
        # ...
      containers:
      - image: gcr.io/intense-talent-188323/elixir-drip-prod:manual_build
        name: elixir-drip-migrations
        command: ["sh", "-c", "bin/$APP_NAME migrate_up"]
        env:
          # ...
```

通常，当 Kubernetes 检测到相应的 Pod 容器已结束时，Job 就会结束。运行完迁移后，elixir-drip-migrations 容器将返回一个退出代码（希望为 0），因此按预期终止。这里的问题在于 Cloud SQL 代理容器，它会无限期运行，即使在另一个容器结束之后也是如此。因此，我们依赖 activeDeadlineSeconds 来终止 Pod。

因为我们使用了 metadata.generateName 字段，Kubernetes 会为 elixir-drip-migrations Job 名称附加一个随机后缀。通过这种方式，我们可以确保永远不会有两个名称相同的 Job。

其余的配置文件完全相同，因为我们运行的是完全相同的版本来运行数据库迁移（请回忆 migrate_up 任务是我们之前实现的自定义发布任务）。让我们运行迁移：

```
$ kubectl -n production create -f deploy/elixir-drip-migrations-prod.yml
job.batch "elixir-drip-migrationskv6tm" created
```

```
$ kubectl -n production get jobs
```

| 名称 | 期望 | 成功 | 年龄 |
|---|---|---|---|
| elixir-drip-migrationskv6tm | 1 | 0 | 4s |

```
$ kubectl -n production get pods
```

| 名称 | 就绪 | 状态 | 重启次数 | 年龄 |
|---|---|---|---|---|
| elixir-drip-646fb5cc94-7d2fs | 2/2 | 运行中 | 0 | 1d |
| elixir-drip-646fb5cc94-dmt2c | 2/2 | 运行中 | 0 | 1d |
| elixir-drip-646fb5cc94-pwgsq | 2/2 | 运行中 | 0 | 1d |
| elixir-drip-migrationskv6tm-lzx2n | 2/2 | 运行中 | 0 | 7s |

过了一会儿，我们看到其中一个 Pod 容器已完成：

```
$ kubectl -n production get pods
NAME READY STATUS RESTARTS AGE
  # ...
elixir-drip-migrationskv6tm-lzx2n 1/2 Completed 0 56s
```

```
$ kubectl -n production logs elixir-drip-migrationskv6tm-lzx2n elixir-drip-migrations
Loading ElixirDrip umbrella app...
Starting dependencies...
Starting ElixirDrip repo...
Running migrations for ElixirDrip...
Success!
18:08:46.048 [info] Already up
```

我们现在终于能够将 Pod 部署到生产环境了：

```
$ kubectl -n production apply -f deploy/elixir-drip-deployment-prod.yml
deployment.extensions "elixir-drip" configured
$ kubectl -n production get pods
```

| 名称 | 就绪 | 状态 | 重启次数 | 年龄 |
|---|---|---|---|---|
| elixir-drip-646fb5cc94-7d2fs | 2/2 | 终止中 | 0 | 1d |
| elixir-drip-646fb5cc94-dmt2c | 2/2 | 运行中 | 0 | 1d |
| elixir-drip-646fb5cc94-pwgsq | 2/2 | 运行中 | 0 | 1d |
| elixir-drip-68b7944c95-c4nhd | 0/2 | 创建容器中 | 0 | 5s |
| elixir-drip-68b7944c95-ct6k7 | 0/2 | 创建容器中 | 0 | 5s |
| elixir-drip-68b7944c95-kqqcv | 0/2 | 创建容器中 | 0 | 5s |

这里我们使用 kubectl 命令，因为如果对象已存在，它会更新具有相同名称的 Kubernetes 对象，这正是我们想要的。过了一会儿，只有新的 Pod 在运行：

```
$ kubectl -n production get pods
```

| 名称 | 就绪 | 状态 | 重启次数 | 年龄 |
|---|---|---|---|---|
| elixir-drip-68b7944c95-c4nhd | 2/2 | 运行中 | 0 | 3m |
| elixir-drip-68b7944c95-ct6k7 | 2/2 | 运行中 | 0 | 3m |
| elixir-drip-68b7944c95-kqqcv | 2/2 | 运行中 | 0 | 3m |

要访问我们的应用程序，我们只需要借助 Kubernetes Service 将其暴露给世界。

## 将 Pod 暴露给世界

在本节中，我们将创建一个 Kubernetes Service，负责暴露我们刚刚创建的 Pod。Service 根据 selector 部分的标签值将请求路由到特定的 Pod：

```
$ cat deploy/elixir-drip-loadbalancer-prod.yml
apiVersion: v1
kind: Service
metadata:
  name: elixir-drip-loadbalancer
  namespace: production
  labels:
    env: production
spec:
  type: LoadBalancer
  selector:
    app: elixir-drip
    env: production
  ports:
    - port: 80
      targetPort: 4000
```

我们将通过负载均衡器的 80 端口到达的请求路由到带有 app=elixir-drip 和 env=production 标签的 Pod 的 4000 端口。创建像我们这样的 LoadBalancer Service 会使用云提供商的负载均衡器来暴露服务。这意味着一个 TCP 负载均衡器将在 Kubernetes 之外运行并拥有自己的 IP，并将请求路由到我们的 Pod。

![](img/c636f4cfb07f25f1db535995e50706ed_490_0.png)

使用 LoadBalancer Service 时，Kubernetes 会自动创建路由请求到 Pod 所需的对象，即 NodePort 和 ClusterIP。负载均衡器的工作方式取决于云提供商，但每个部署的服务始终拥有自己的负载均衡器，因此仅依赖负载均衡器可能成本高昂。更好的替代方案是使用 Kubernetes Ingress，它能够执行 SSL 终止、HTTP 路由等操作，并且可以在服务之间共享。在本书中，我们将只使用 LoadBalancer Service。

如果我们现在检查现有的服务，我们会看到以下内容（我们使用 --watch 选项，因此 kubectl 会在其中一个对象更新时更新其输出）：

```
$ kubectl create -f deploy/elixir-drip-loadbalancer-prod.yml
service "elixir-drip-loadbalancer" created

$ kubectl -n production get service --watch
```

| 名称 | 类型 | 集群 IP | 外部 IP | 端口 | 年龄 |
|---|---|---|---|---|---|
| elixir-drip-loadbalancer | 负载均衡器 | 10.43.254.164 | <待定> | 80:32153/TCP | 9s |
| elixir-drip-loadbalancer | 负载均衡器 | 10.43.254.164 | 35.205.152.228 | 80:32153/TCP | 1m |

一开始，负载均衡器 IP 为待定（可能是因为尚未配置），但在几分钟后，我们终于获得了外部 IP。我们现在可以从外部访问我们的应用程序，如下所示：

```
$ curl http://35.205.152.228
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="">
```
