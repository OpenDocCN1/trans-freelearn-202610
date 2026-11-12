

## 安装 Rancher 服务器

Rancher 服务器可以添加到任何现有的 Docker 集群中。唯一要求是运行 Docker 1.10.3 或更高版本。测试用的快速安装只需运行 `docker run -d -p 8080:8080 rancher/server` 即可。之后，Web 界面将可在 Docker 主机的 8080 端口上访问。

对于永久性方案，Rancher 数据需要存储在卷中，或连接到外部 MySQL 数据库。卷可以是任何 Docker 卷，包括主机服务器上的目录。以下示例将 MySQL 数据存储在 `/var/lib/rancherdb` 中：

```
$ docker run -d -v /var/lib/rancherdb:/var/lib/mysql -p 8080:8080
rancher/server
```

对于外部数据库，创建包含连接信息的环境文件。CATTLE_DB_CATTLE_MYSQL_NAME 中设置的数据库必须已创建，CATTLE_DB_CATTLE_USERNAME 中设置的用户必须已被授予 Cattle 数据库的所有权限。如果架构不存在，Rancher 将自动创建它：

```
CATTLE_DB_CATTLE_MYSQL_HOST=mysql-host
CATTLE_DB_CATTLE_MYSQL_PORT=3306
CATTLE_DB_CATTLE_MYSQL_NAME=cattle
CATTLE_DB_CATTLE_USERNAME=cattle
CATTLE_DB_CATTLE_PASSWORD=cattle-password
```

Rancher Server 可以使用环境文件启动。在本示例中，它名为 `cattle.env`：

```
$ docker run -d --env-file cattle.env --name rancher-server -p 8080:8080
rancher/server
```

![](img/02bead9c8ff4515f78dac0eb33163961_186_0.png)

Rancher Server 可以安装在多个节点上以实现高可用性。有关更多信息，请参阅 http://docs.rancher.com/rancher/v1.2/en/installing-rancher/installing-server/。

## 安装 Rancher 代理

Rancher 启动后，可以使用 `docker-machine` 支持的任何上游提供商创建新主机。登录 Web UI，在基础架构菜单中转到 **主机**，然后单击 **添加主机**。向导将指导您将主机添加到云环境中：

![](img/02bead9c8ff4515f78dac0eb33163961_187_0.png)

也可以手动添加代理。在 **添加主机** 中选择 **自定义** 选项。将显示一个 Docker 命令，需要在每个添加到集群的 Docker 主机上运行。它看起来将类似于此处列出的命令：

```
sudo docker run -d --privileged -v
/var/run/docker.sock:/var/run/docker.sock -v
/var/lib/rancher:/var/lib/rancher rancher/agent:v1.0.2
http://172.17.8.101:8080/v1/scripts/781E07FE518B7181D563:1477180800000:N3gz
UQgoo3mFPcGkqEimBoNExFC
```

![](img/02bead9c8ff4515f78dac0eb33163961_188_0.png)

Rancher 代理在环境中所有主机之间创建 IPSEC 链接，以提供安全的覆盖网络。它还会监听 Docker 事件，并将有关运行容器和性能统计信息的定期更新发送到 Rancher 服务器。

## 安装 Rancher Compose

在 Rancher 中创建和管理服务所需的几乎所有操作都可以通过 Web 界面完成。但是，还有一个名为 `rancher-compose` 的命令行工具可供使用。它适用于 Windows、OS X 和 Linux。要安装 Rancher Compose，请执行以下步骤：

- 1. 要下载它，请单击 Web 界面右下角的 **下载 CLI**，并选择您的操作系统。
- 2. 解压下载的存档，并将 `rancher-compose` 工具放置在您的路径中。
- 3. Rancher Compose 需要 API 密钥来对 Rancher 服务器进行身份验证。可以在页面顶部的 API 部分中为环境或特定用户创建新密钥。
- 4. 单击 **添加环境 API 密钥** 或 **添加帐户 API 密钥** 以创建相应的密钥。您将看到访问密钥和密钥。
- 5. 将它们复制到安全位置。创建后无法获取密钥：

![](img/02bead9c8ff4515f78dac0eb33163961_189_0.png)

访问密钥、密钥和 Rancher URL 将分别使用 `--access-key`、`--secret-key` 和 `--url` 标志传递给 `rancher-compose`：

```
$ rancher-compose --url http://rancher:8080/ --access-key=CA... --secret-key=C4G...
```

也可以使用 `RANCHER_ACCESS_KEY`、`RANCHER_SECRET_KEY` 和 `RANCHER_URL` 环境变量设置密钥和 URL。将它们添加到客户端系统中 `.bash_profile` 的等效文件中：

```
export RANCHER_URL=http://rancher:8080/
export RANCHER_ACCESS_KEY=CA...
export RANCHER_SECRET_KEY=C4G...
```

### 介绍环境

Rancher 支持运行多个环境。安装后，将创建一个名为 `default` 的环境，它运行 Cattle 编排套件。可根据需要创建其他环境。环境彼此隔离。这样可以轻松地为开发、测试、生产或不同业务单元创建环境。

尽管 Cattle 是默认编排工具，Rancher 还能够创建运行 Docker Swarm、Kubernetes 和 Mesosphere 的新环境。这可能是在本地环境中安装这些系统的最简单方法。创建新环境后，添加新主机。环境的服务（例如 `kubectl` 或 Swarm 代理）将自动添加。

有关使用 Docker Swarm 的更多信息，请参阅第 4 章，使用 Docker Swarm 进行编排；有关 Kubernetes 的更多信息，请参阅第 5 章，使用 Kubernetes 部署和管理服务；有关 Mesosphere 的更多信息，请参阅第 6 章，使用 Mesosphere。

### 管理服务

Cattle 使用 Docker Compose 文件定义服务。自 1.1.4 版起，仅支持 Docker Compose 规范的版本 1。Rancher 1.2 正在添加对版本 2 的支持。

Rancher Compose 在标准 `docker-compose.yml` 文件的基础上有两种扩展方式。首先，可以向服务添加特殊标签以定义亲和性等内容。其次，可以使用名为 `rancher-compose.yml` 的另一个文件来定义 Rancher 特定设置（如运行状况检查）。

> > 以下部分介绍如何使用 Rancher CLI。所有以下操作也可以通过 Web 界面完成。

### 启动堆栈

从设计上讲，`rancher-compose` 工具与 `docker-compose` 非常相似。**堆栈** 是在 `docker-compose.yml` 文件和可选的 `rancher-compose.yml` 文件中定义的服务集合。Rancher 目录中有许多应用程序的预构建堆栈。除了快速部署服务外，它们还可以作为构建您自己的堆栈的绝佳示例。

要创建自己的堆栈，请为包含 docker-compose.yml 文件的应用程序创建一个目录。堆栈名称源自目录名称，但可以使用 -s 标志覆盖。创建 compose 文件后，在目录中运行 rancher-compose up。以下是启动 WordPress 堆栈的 docker-compose.yml 文件：

```
wordpress:
  image: wordpress
  env_file:
    - db.env
db:
  image: mysql:5.6
  env_file:
    - db.env
lb:
  image: rancher/load-balancer-service
  ports:
    - 80:80
  links:
    - wordpress:wordpress
```

此示例启动三个服务。第一个 wordpress 启动 WordPress 应用程序。第二个 db 是 MySQL 数据库。两个服务都使用 db.env 环境文件来加载启动所需的环境变量。环境文件在第 2 章，使用 Docker Compose 构建多容器应用程序中已介绍。

lb 服务为此应用程序配置 Rancher 负载均衡器服务。负载均衡器将在本章后面更详细地介绍。目前，只需知道它被配置为将端口 80 发送到 wordpress 服务即可。

与 docker-compose 类似，可以使用 -f 标志在命令行上指定多个 docker-compose.yml 文件。还可以设置 -d 标志在后台启动堆栈：

```
$ rancher-compose up -f docker-compose.yml -f production.yml -d
```

### 停止堆栈

服务使用 rancher-compose stop 停止。这必须在包含 docker-compose.yml 文件的同一目录中运行。默认情况下，rancher-compose 将停止所有运行中的服务：

