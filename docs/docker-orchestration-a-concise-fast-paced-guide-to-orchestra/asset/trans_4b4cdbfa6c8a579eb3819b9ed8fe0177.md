

## 删除应用程序

当服务不再需要时，可以使用 `dcos marathon app remove` 将其移除。应用程序不必停止即可移除。Marathon 会停止运行中的容器，并从列表中移除应用程序：

```
$ dcos marathon app remove simple-nginx
```

## 强制 Marathon 拉取镜像

在集群中运行容器时最大的问题之一，就是确保正在运行的镜像是最新的。请记住，Docker 缓存的镜像是从注册表下载的。如果容器使用 `nginx` 镜像启动，而发布了修复安全漏洞的新版本，该主机上的容器将继续运行存在漏洞的版本。

Marathon 提供 `forcePullImage` 选项，强制节点在每次启动应用程序时都拉取新镜像。它位于容器规范的 `docker` 部分中。以下示例演示其工作原理：

```
"docker" : {
    "image" : "nginx:latest",
    "forcePullImage" : true
}
```

确保您的注册表能够处理负载。如果有大量拉取镜像的应用程序启动，则可能会使注册表不堪重负。

## 使用环境变量

某些 Docker 镜像需要使用环境变量才能运行。例如，官方 MySQL 镜像使用它们来设置初始 root 密码。Marathon 使用 `env` 标签将环境变量传递给任务。在此示例中，正在设置环境变量 `MYSQL_ROOT_PASSWORD` 和 `MYSQL_DATABASE`：

```
{
    "id": "/mysql",
    "instances": 1,
    "cpus": 1,
    "mem": 1024,
    "env": {
        "MYSQL_ROOT_PASSWORD": "password",
        "MYSQL_DATABASE": "mydb"
    },
    "container": {
        "docker": {
            "image": "mysql:5.7",
            "forcePullImage": true,
            "network": "HOST"
        }
    }
}
```

## 缩放应用程序

Marathon 使调整应用程序任务数量变得非常容易。这不仅允许扩展以应对增加的负载，还允许进行蓝绿部署，这将在本章后面介绍。

假设有一个名为 nginx 的应用程序正在运行一个实例。要将其增加到两个实例，只需运行 dcos marathon app update：

```
$ dcos marathon app update nginx instances=2
Created deployment 8dbf3d16-44d3-4478-9366-9743658dcfcd
```

此示例将实例数设置为两个。同样，应用程序也可以扩展或缩减。Marathon 会尝试启动或停止所需的实例数量，以满足新要求。

请确保您的应用程序支持运行多个实例。特别是数据库，对其他进程更改其文件的反应不佳。

## 检查应用程序健康状况

Marathon 与 Kubernetes 非常相似，支持为服务设置一个或多个健康检查选项，以确保其在可用于处理请求之前运行正常。有运行自定义检查命令、检查 TCP 连接或检查特定路径 HTTP 状态的选项。

## 使用 TCP 和 HTTP 检查

以下是 MySQL 的 TCP 检查示例。`protocol` 选项指示这是 TCP 检查。`portNumber` 定义要连接的端口。或者，可以将其替换为 `portIndex`，它将检查应用程序定义中 `ports` 或 `portDefinition` 列表的索引。索引从零开始，因此第一个条目是 0，第二个条目是 1，依此类推：

```
"healthChecks": [
{
"protocol": "TCP",
"portNumber": 3306,
"gracePeriodSeconds": 300,
"intervalSeconds": 60,
"timeoutSeconds": 20,
"maxConsecutiveFailures": 3
}
]
```

有许多可用于每种检查类型的设置，如下所示：

- `gracePeriodSeconds`: 在抛出错误前等待的秒数。它允许 MySQL 等需要启动时间的容器在抛出错误前完成初始化。如果在宽限期结束前检查通过，应用程序会立即标记为健康。
- `intervalSeconds`: 检查之间等待的秒数。
- `timeoutSeconds`: 如果在指定的秒数内没有响应，则检查失败。
- `maxConsecutiveFailures`: 如果健康检查连续失败指定次数且没有成功，则任务会被终止并重启。
- `taskKillGracePeriodSeconds`: 当任务失败次数过多时，Marathon 会发送 `SIGTERM` 信号。如果在经过指定秒数后任务仍未停止，Marathon 将发送 `SIGKILL` 任务。默认为三秒。

HTTP 和 HTTPS 协议检查给定路径的 HTTP 状态码。可以设置 `path` 选项，它定义要测试的路径。默认值为 /。HTTP 和 HTTPS 检查还使用前面描述的 `portNumber` 和 `portIndex` 选项。HTTPS 协议用于检查启用 SSL 的网站。它不检查 SSL 证书的有效性。

默认情况下，只有 200-399 范围内的状态码被视为健康。您可以通过将 `ignoreHttp1xx` 设置为 `true` 来配置检查以忽略 100-199 的状态码。其他所有状态码都被视为失败。

## 使用命令检查

也可以使用自定义命令检查健康状态。如果命令以真值退出，则检查被视为成功，通常在 sh 中为 0。其他任何情况都被视为失败。以下示例向应用程序添加命令检查：

```
{
  "id" : "nginx-health",
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
          "servicePort" : 80,
          "name" : "web"
        }
      ]
    }
  },
  "upgradeStrategy" : {
    "minimumHealthCapacity" : 1,
    "maximumOverCapacity" : 1
  },
  "healthChecks" : [
    {
      "protocol" : "COMMAND",
      "command" : {
        "value" : "your_check_health_command.sh"
      }
    }
  ]
}
```

命令传递给 /bin/sh -c "”。命令中的任何双引号都必须转义。为使事情更加复杂，转义字符（\）也必须转义。包含双引号组件的命令类似于以下内容：

```
"healthChecks" : [
  {
    "protocol" : "COMMAND",
    "command" : {
      "value" : "your_check_health_command.sh "Foo""
    }
  }
]
```

## 部署滚动更新

现有应用程序使用 dcos marathon update 进行更新。此命令之前用于调整应用程序实例数量。它也可用于更新应用程序以使用新镜像：

```
{
  "id" : "nginx",
  "instances" : 1,
  "cpus" : 0.25,
  "mem" : 64.0,
  "container" : {
    "type" : "DOCKER",
    "docker" : {
      "image" : "nginx:1.10",
      "network" : "HOST"
    }
  }
}
```

这是一个简单的 Nginx 应用程序。

请注意，镜像标签设置为 nginx:1.10。碰巧 1.11 已发布，现在是时候更新应用程序了。有两种方法可以做到这一点。

最简单的方法是在 Web 界面中编辑应用程序。在 **容器设置** 中，将 **容器镜像** 改为 `nginx:1.11` 并单击 **部署更改**。将创建一个新部署，该部署将创建运行 `nginx:1.11` 镜像的新任务并结束旧任务：

![](img/02bead9c8ff4515f78dac0eb33163961_157_0.png)

第二种方法是使用 `dcos marathon update`。如果应用程序已在 JSON 文件中定义，此方法效果最佳。创建新文件或编辑定义 Nginx 应用程序的现有文件。将镜像标签更改为 `nginx:1.11`：

```
{
  "id" : "nginx",
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
