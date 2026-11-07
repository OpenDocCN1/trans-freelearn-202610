
看起来应该还有另一个选项，允许在容器启动期间自动映射镜像中（即在 Dockerfile 中）暴露的端口。就是它：-P 选项（这次是大写 P）会将一个动态分配的主机端口映射到所有由 Dockerfile 中的 EXPOSE 指令暴露的容器端口。让我们在表格中简要总结一下这些选项：

| 指令 | 含义 |
|---|---|
| EXPOSE | 这表示在指定的端口上有服务可用。这在 Dockerfile 中使用，使暴露的端口对其他容器开放。 |
| --expose | 这与 EXPOSE 类似，但用于运行时。 |
| -p | 这指定了一个端口映射规则，将容器上的端口与主机机器上的端口映射。这使得端口从 Docker 外部开放。 |
| -P | 这将主机机器上的动态分配端口映射到使用 EXPOSE 或 --expose 暴露的所有端口。 |

请注意，我们将以守护进程方式启动 nginx，Docker 会返回容器 ID 以供将来参考，这样你就可以使用 kill 停止容器或使用 inspect 命令查看其详细信息。如果我们现在执行 ps 命令来列出活动和正在运行的容器，在 PORTS 列中我们会看到发生的端口映射。在我们的例子中，Docker 将 nginx 暴露的端口号 80 映射到主机机器上的端口号 32773，并将暴露的端口号 443 映射到主机机器上的端口号 32772：

```
docker run --name nginx -d -P nginx
```

![](img/0c87321919fa245e22f056c67ec70003_94_0.png)

![](img/0c87321919fa245e22f056c67ec70003_95_0.png)

确定当前端口映射的最快方法就是使用 ps 命令（正如你从前几章记得的，它用于查看正在运行的容器的列表）。

-p 选项另一方面给了我们更多控制。在这种情况下，Docker 不会自动选择任何端口——由你决定主机上的哪些端口应映射到容器的端口。-p 选项的语法相当直接：你只需输入容器暴露的端口号、冒号，然后是希望在主机机器上映射的端口：

```
docker run -p <containerPort>:<hostPort> <image ID or name>
```

如果需要，你也可以使用 Dockerfile 中的 EXPOSE 指令（例如 EXPOSE 7000-8000）或 run 命令将一整段端口暴露给其他容器：

```
docker run --expose=7000-8000 <container ID or name>
```

当然你随后可以通过 Docker 的 run (https://docs.docker.com/engine/reference/commandline/cli/) 命令将一段端口发布到主机机器：

```
docker run -p 7000-8000:7000-8000 <container ID or name>
```

好吧，让我们使用 stop 命令停止 nginx 并启动另一个容器，这次将容器端口 80（在 Dockerfile 中暴露）映射到我们主机机器的端口号 8080：

```
docker run --name nginx -d -p 8080:80 nginx
```

![](img/0c87321919fa245e22f056c67ec70003_95_1.png)

启动后，你可以通过链接 http://localhost:8080 在主机机器的本地地址和映射的端口 8080 访问默认的 nginx 欢迎页面。

![](img/0c87321919fa245e22f056c67ec70003_96_0.png)

作为 ps 命令的替代方案，你可以执行 port 命令，使用容器 ID 作为参数（容器启动期间 Docker 返回的 ID）来确定已映射的端口。以下是我在我的情况下的命令：

```
docker port e1926dc061c5125b5aa3fd2685bcda4cf26ee99def4b2a1ae5e53721b88231c1
```

结果，Docker 将输出映射，说明容器中的端口号 80 已映射到主机机器上的端口号 8080：

![](img/0c87321919fa245e22f056c67ec70003_96_1.png)

你也可以使用 inspect 命令查看有关暴露的（以及已映射的，如果有的话）端口的信息：

```
docker inspect e1926dc061c5125b5aa3fd2685bcda4cf26ee99def4b2a1ae5e53721b88231c1
```

如果你滚动到 Ports 部分，你会找到镜像暴露的端口列表（在 nginx 的情况下将是 TCP 443 和 TCP 80）以及在主机机器上对应的映射端口，如果你决定使用 -p 或 -P 选项运行容器：

![](img/0c87321919fa245e22f056c67ec70003_97_0.png)

请注意，nginx 镜像暴露了两个端口，但我们只映射了其中一个，端口号 80。你可以使用 -P 选项运行容器以自动映射所有暴露的端口（到 Docker 主机端口范围内的一个随机高端口），或者你可以有多个 -p 选项分别映射它们中的每一个。如果需要，你甚至可以像这样将多个主机机器端口映射到容器中的单个暴露端口：

```
docker run --name nginx -d -p 8080:80 -p 8081:80 nginx
```

![](img/0c87321919fa245e22f056c67ec70003_97_1.png)

-p 标志可以多次使用来配置多个端口。

端口映射是一个很棒的功能。它为你提供了灵活的配置可能性，让你的容器对外部世界开放。事实上，如果你希望你的容器化 Web 服务器、数据库或消息服务器能够与其他服务器通信，这是不可或缺的。

Docker 网络是一个相当新的功能。然而 Docker 还有另一个可能派上用场的特性：容器之间的链接。让我们来了解一下它们。

## 链接容器

Docker 包含一种机制，允许将多个容器链接在一起。已链接的容器会将连接信息从一个容器发送到另一个容器。在创建链接时，源容器的信息会被发送到目标容器。这种用例的一个很好的例子是将 Web 应用程序容器链接到数据库容器。链接后，数据库容器会向 Web 应用程序容器暴露一些信息（如数据库名称、连接 URL、用户名和密码）。要将容器链接在一起，我们在 run 命令中使用 --link 选项。如果我们将一个容器链接到另一个容器，幕后会发生两件事。首先，Docker 会自动更新已链接容器的 /etc/hosts 文件，以便引用我们链接到的容器。

让我们以示例来尝试。首先，让我们在桥接网络上运行 MySQL 容器：

![](img/0c87321919fa245e22f056c67ec70003_98_0.png)

接下来，让我们运行第二个容器，同样是最新版的 Ubuntu，但这次将其与 MySQL 容器链接：

```
docker run -it --name=ubuntu --link mysql:mysql ubuntu
```

请注意 MySQL 容器必须正在运行。

![](img/0c87321919fa245e22f056c67ec70003_98_1.png)

你只能链接到正在运行的容器。

你可以看到 Docker 自动调整了 Ubuntu 容器的 /etc/hosts 文件，为 MySQL 实例插入了一个条目：

```bash
$ docker run -it --name=ubuntu --link mysql:mysql ubuntu
root@60886fca65fb:# more /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.17.0.2      mysql 5d22b0ea87d4
172.17.0.3      60886fca65fb
root@60886fca65fb:#
```

/etc/hosts 链接是持久的——如果重启源容器并分配新的 IP 地址，已链接容器的 /etc/hosts 文件将自动使用源容器新的 IP 地址进行更新。

创建链接时发生的第二个重要事情，是从我们链接到的容器传输一些暴露的环境变量。看看我们的已链接 Ubuntu 容器吧；它神奇地知道有关已链接 MySQL 数据库的位置和设置的大量信息，这都要归功于 --link 选项（当然，也要归功于正确定义的 MySQL 镜像，暴露了这些信息）：

```bash
root@60886fca65fb:# env | grep MYSQL
MYSQL_ENV_MYSQL_ROOT_PASSWORD=my-secret-pw
MYSQL_ENV_GOSU_VERSION=1.7
MYSQL_ENV_MYSQL_VERSION=5.7.13-1debian8
MYSQL_ENV_no_proxy=*.local, 169.254/16
MYSQL_NAME=/ubuntu/mysql
MYSQL_PORT_3306_TCP_PROTO=tcp
MYSQL_PORT_3306_TCP_ADDR=172.17.0.2
MYSQL_ENV_MYSQL_MAJOR=5.7
MYSQL_PORT=tcp://172.17.0.2:3306
root@60886fca65fb:#
```
