

defroutes macro.

## 将应用程序打包成 JAR 文件

你之前见过这个，但前面的应用程序也可以通过创建包含所有依赖项的 JAR 文件并运行该 JAR 文件的主类 main 方法来运行：

```
lein uberjar
java -jar target/withdocker-0.1.0-SNAPSHOT-standalone.jar
```

主入口点由 Leiningen 为你自动生成，通过生成位于 JAR 文件中的 META-INF/MANIFEST.MF 文件，其中 main-Class 条目取自 project.clj 中的 :main withdocker.handler：

```
Manifest-Version: 1.0
Built-By: niko
Created-By: Leiningen 2.6.1
Build-Jdk: 1.8.0_66
Main-Class: withdocker.handler
```

## 创建 Docker 容器

创建 JAR 文件并作为独立程序运行的思想实际上就是你将用来创建独立 Docker 镜像所做的事情。

我们在这里假设你之前见过 Dockerfile，但如果没有也不用担心！一切都很好。Dockerfile 就像好老的 Makefile 一样，描述了一系列步骤。Dockerfile 应放置在项目文件夹的根目录。

下面是一个带注释的 Dockerfile：

```
# a base image for the container, here something that has the java executable
FROM java:openjdk-8-jre
# copy the jar file inside the container to /app/withdocker.jar
COPY target/withdocker-0.1.0-SNAPSHOT-standalone.jar /app/withdocker.jar
# say the container will be exposing an app on port 8080
EXPOSE 8080
# what will be executed when you do, docker run ...
ENTRYPOINT [ "java", "-jar", "/app/withdocker.jar"]
```

假设你现在本地机器上已经运行了 Docker daemon，你可以构建名为 withdocker 的容器：

```
docker build . -t withdocker
```

如果一切顺利，你应该会看到一些很酷的输出：

```
Sending build context to Docker daemon 12.66 MB
Step 1 : FROM java:openjdk-8-jre
---> 76fd51ceaa2e
Step 2 : COPY target/withdocker-0.1.0-SNAPSHOT-standalone.jar /app/withdocker.jar
---> 10df21f2d7b2
Removing intermediate container 6444756f7aaf
Step 3 : EXPOSE 8080
---> Running in 0496cc294447
---> 56446e3f92fe
Removing intermediate container 0496cc294447
Step 4 : ENTRYPOINT java -jar /app/withdocker.jar
---> Running in 030972a7ac62
---> cea5a78eab27
Removing intermediate container 030972a7ac62
Successfully built cea5a78eab27
```

作为提醒，前面的输出中的每个步骤都对应 Dockerfile 中的一行。

现在让我们启动我们刚烤好的容器：

```
docker run withdocker
```

而且...Woohoo！Jetty 启动了！

```
2016-08-03 16:28:10.912:INFO:oejs.Server:jetty-7.x.y-SNAPSHOT
2016-08-03 16:28:11.257:INFO:oejs.AbstractConnector:Started SelectChannelConnector@0.0.0.0:8080
```

啊...正如你可能已经注意到的，页面不会打开。你仍然需要告诉 Docker 它需要映射的端口；在做这件事的同时，

让我们把它作为后台进程：

```
docker run -d -p 8080:8080 --name withdocker withdocker
```

这将告诉 Docker 将容器的 8080 端口映射到本地机器的 8080 端口，从而使其对外部世界可见。

Hello Docker World...

## 如何操作...

现在你已经制作了一个漂亮的 Docker 镜像，你准备好征服世界并将你的镜像部署到不同的地方。

正如你一些朋友之前所做的那样，通常第一个要攻克的据点是 Docker Hub，网址为 https://hub.docker.com/。

如果还没有，请在那里创建一个账户，然后回来继续。

从命令行，我们将首先登录账户，按照这里显示的小配方操作：

```
nicolassmacbook% docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username (hellonico):
Password:
Login Succeeded
```

### Note

请注意，期望的登录用户名是 Docker Hub ID，而不是使用的电子邮件地址。

构建 Docker 容器会稍微长一点，镜像名称前面加上前缀：

```
docker build . -t hellonico/withdocker
```

这个 hellonico 前缀将告诉 Docker 容器准备好将镜像推送到 Docker Hub。说到推送...

```
nicolassmacbook% docker push hellonico/withdocker
```

```
The push refers to a repository
[docker.io/hellonico/withdocker]
4ec68560f7e5: Pushing [===================>]
] 3.442 MB/7.456 MB
b5d7c200f6f3: Layer already exists
```

Docker 镜像现在可以在 Docker Hub 上找到：

![](img/d7b631352a782ee50160cf8eb995152f_666_0.png)

现在，由于这个配方假设你有一台设置了 docker 的第二台机器，让我们尝试在那里运行镜像。由于我们稍微更改了镜像名称以将其上传到 Docker Hub，命令现在如下所示：

```
docker run -d -p 8080:8080 --name withdocker
hellonico/withdocker
```

由于镜像未在本地构建，你会得到一些友好的提醒和一些免费下载：

```
Unable to find image 'hellonico/withdocker:latest' locally
latest: Pulling from hellonico/withdocker
5c90d4a2d1a8: Downloading
[============================================>          ] 40.89 MB/51.35 MB
```

除了花费几秒钟找到 docker IP 之外，这在那里也很好地打开了！

![](img/d7b631352a782ee50160cf8eb995152f_667_0.png)

很棒。现在变得有趣了。让我们看看如何跳过 Docker Hub 步骤并直接部署到远程机器。

## 直接连接到远程 Docker

运行任何 Docker 命令时，你可以要求 Docker 客户端连接到本地 daemon 或远程 daemon。

默认 daemon 运行在 Unix socket 上，通常位于 /var/run/docker.sock 附近。

如果你记得我们在 Debian 机器上做的配置更改，或者如果你还没有做，这里有一个快速提醒。你会看到我们要求 daemon 将自身绑定到 Unix socket 和 TCP socket：

```
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
```

Docker daemon 现在能够接收来自本地客户端或将使用 TCP 协议的远程 Docker 客户端的命令。

要要求客户端与远程协议通信，需要设置 DOCKER_HOST 属性并为其分配远程 Docker daemon 运行的位置的值：

```
export DOCKER_HOST="192.168.1.4:2376"
```

现在任何命令都将针对远程主机，因此运行简单的 ps 将显示在远程 daemon 上运行的容器：

```
docker ps
CONTAINER ID      IMAGE                     COMMAND
CREATED           STATUS                    PORTS
NAMES
556224b9435e      hellonico/withdocker      "java -jar
/app/withd"      9 minutes ago             Up 9 minutes
0.0.0.0:8080->8080/tcp    withdocker
```

所以现在，即使构建镜像也会在远程 daemon 上完成：

```
docker build . -t hellonico/withdocker
```

这太酷了。这意味着从代码仓库的不同分支部署代码到远程环境现在可以在几秒钟内完成：

```
lein uberjar && docker build . -t hellonico/withdocker
```

作为留给读者的练习，更新 withdocker/handler.clj 为更有趣的内容，再次构建容器，然后使用以下命令运行 Docker 镜像：

```
docker run -d -p 8081:8080 --name withdockerremote
hellonico/withdocker
```

新的镜像已被创建，现在运行在 8081 端口：

![](img/d7b631352a782ee50160cf8eb995152f_668_0.png)

## 还有更多...

最后，为了以一种有趣轻松的方式结束这个配方，我们将看到在远程 PAAS 上启动你的 Docker 镜像之一是多么容易。

我们在这里要看的提供商名为 [sloppy.io](sloppy.io)，但我们还没有与他们达成任何广告协议，所以请将其视为纯粹简单的介绍：

![](img/d7b631352a782ee50160cf8eb995152f_669_0.png)

一旦你获得了全新的、暂时免费的新账户，创建一个新项目：

