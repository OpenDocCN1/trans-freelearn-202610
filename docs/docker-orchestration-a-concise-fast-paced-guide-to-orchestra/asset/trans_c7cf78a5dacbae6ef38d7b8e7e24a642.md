

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
# cd /etc/secrets/
# ls
root wordpress
# cat root
root-password
```

### 登录到容器

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

### 使用部署

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

