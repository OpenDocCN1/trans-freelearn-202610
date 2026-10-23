## 容器与 Docker 简介

在上一章中，你学习了如何测试 Cargo Tracker 应用。至此，Cargo Tracker 应用已准备好部署到不同的环境、机器和云中。

然而，如何确保环境已为 Cargo Tracker 做好准备，并且它能在其中运行呢？在本章中，你将了解如何使用容器和 Docker 来解决这个问题。我们将从什么是容器、容器能做什么以及如何创建容器开始。当你准备好容器的规范后，你将安装 Docker，并创建和运行容器。

在本章中，我们将涵盖以下主要内容：

- 什么是容器？
- Docker 简要介绍
- 安装 Docker
- 运行 Docker 容器

到本章结束时，你将清楚了解什么是容器，以及如何定义和创建它们。你还将为 Cargo Tracker 应用创建一个容器，并可在装有 Docker 的任何机器上运行它。

### 技术要求

本章要求你的系统上有一个 Cargo Tracker 应用，并且需要第 3、4 或 5 章中任一章节的 Cargo Tracker 应用成果。要编辑 Docker 文件，你可以使用任意文本编辑器。我们将在 Linux 机器上安装 Docker。如果你想按照这些说明操作，还需要一台 Linux 机器或虚拟机。你也可以按照 Docker 网站上的说明进行操作：https://docs.docker.com/engine/install/ubuntu/。

### 什么是容器？

在深入探讨什么是容器之前，让我们先看看没有容器之前的情况，以及容器解决了哪些问题。在容器出现之前，部署应用是一个更复杂、更耗时的过程。应用被部署到物理服务器或虚拟机上，开发人员必须考虑其应用所需的所有不同操作系统、库和其他依赖项，并加以支持。更复杂的是，这还必须在不同的阶段环境中完成——例如，开发、测试、验收或生产环境。

这意味着部署软件的过程通常是手动且容易出错的。开发人员必须在每台物理服务器或虚拟机上手动配置和安装所需的库。这导致了不同环境之间的不一致，以及由于依赖缺失、配置问题和所用依赖版本不匹配而导致的部署失败。

此外，在物理服务器上部署软件可能成本高昂，因为每台服务器都需要配置和维护，而扩容或缩容还需要额外购买硬件并进行配置。

现在，我们了解了不使用容器时的情况，接下来看看什么是容器，以及它如何解决这些问题。

容器是一种以可移植但最重要的是一致的方式来打包和部署应用的技术。容器是一个独立的可执行包，包含应用运行所需的一切——应用本身、运行时、依赖项、配置和系统工具。

容器提供了一种清晰且一致地隔离和管理应用及其依赖项的方式，使得在多个不同环境中运行和部署软件变得更加容易。这使部署更加一致，并减少了因依赖缺失和配置缺失而导致的部署失败次数，这些问题一旦你尝试在自己的机器上运行它们，就会立刻遇到。

容器已成为软件开发和部署中非常重要且不可或缺的工具。一种流行的容器技术是 Docker；在下一节中，我们将更深入地探讨 Docker 是什么，以及它如何运行我们创建的容器。

### 容器是如何创建的？

容器是最终结果，但它是如何创建的呢？容器是基于镜像构建的。镜像就像一个蓝图，告诉 Docker 如何创建容器。有了镜像，你可以创建任意数量的容器，而且它们都是一样的。Docker 将镜像用作组装容器的指令。




### 安装 Docker

在本节中，我们将在一台 Linux 机器上安装 Docker。第一步是获取 Docker 的副本。你可以从其网站下载：https://docs.docker.com/desktop/install/linux-install/#supported-platforms。在那里，你可以选择你使用的 Linux 平台/发行版。在接下来的示例中，我将使用在 Fedora 机器上安装 Docker 所需的命令，但你应遵循适用于你自己平台的说明。

第一步是安装 `dnf-plugins-core` 软件包，其中包含用于管理 DNF 存储库的命令。运行以下命令以安装此软件包：

```
sudo dnf -y install dnf-plugins-core
```

下一步是使用以下命令添加 Docker 存储库：

```
sudo dnf config-manager \
  --add-repo \
  https://download.docker.com/linux/fedora/docker-ce.repo
```

添加存储库后，我们将下载并安装 rpm 软件包。你可以在你平台的安装说明中找到该文件的最新版本。你也可以使用以下 URL 来使用与我们相同的版本：https://desktop.docker.com/linux/main/amd64/docker-desktop-4.17.0-x86_64.rpm?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64。当你拥有该 rpm 软件包后，使用以下命令在你的系统上安装它：

```
sudo dnf install ./docker-desktop-4.17.0-x86_64.rpm
```

在安装过程中，你需要回答一些问题；第一个问题涉及将被下载、升级和安装的依赖项，以及你是否同意这些更改。要同意，请按 Enter 键。

第二个问题是你是否希望导入 Docker 的 GPG 密钥；同样，按 Enter 键接受导入。现在，该工具将安装在你的 Fedora 机器上。

当安装完成时，你将在控制台中看到以下结果：

```
xclip-0.13-18.git11cba61.fc37.x86_64
xorg-x11-proto-devel-2022.2-2.fc37.noarch
xz-devel-5.4.1-1.fc37.x86_64
Complete!
```

当你看到此信息时，说明你的系统已安装 Docker。启动 Docker Desktop 应用程序有两种方式。你可以从“显示应用”界面启动它，或者运行以下命令来启动 Docker：

```
systemctl --user start docker-desktop
```

在某些情况下，了解系统上是否安装了 Docker，以及如果已安装则其运行的版本，会很有帮助。要检查系统上 Docker 的版本，请运行以下命令：

```
docker version
```

这将显示有关正在运行的 Docker 版本的许多信息，但输出中的这一行特别有趣：

```
Server: Docker Desktop 4.17.0 (99724)
```

这一行告诉你，你正在使用 Docker Desktop 版本 4.17.0 作为你的 Docker 容器的服务器。

### 运行 Docker 容器

在本节中，你将获取 Cargo Tracker 应用程序，为其构建一个 Dockerfile，并将其作为 Docker 容器在你的系统上运行。我们将首先创建一个你可以使用的 Dockerfile。使用 Dockerfile，我们将告诉 Docker 引擎如何为我们创建容器以及我们希望如何运行它。在容器运行后，我们将查看如何使用命令行界面来管理容器并查看 Cargo Tracker 应用程序生成的日志。创建此 Dockerfile 将是我们通往在 Docker 上运行 Cargo Tracker 旅程的起点。

#### 创建 Docker 容器

在 Docker 引擎上运行 Cargo Tracker 的第一步是创建一个 Dockerfile。Dockerfile 包含给 Docker 引擎的指令，说明如何创建我们想要的容器。开始时在 Cargo Tracker 应用程序的根目录下创建一个名为 Dockerfile 的文件。在 Dockerfile 内部，你需要添加以下两行代码：

```
FROM payara/server-full:6.2023.2-jdk17

COPY target/cargo-tracker.war $DEPLOY_DIR
```

第一行代码指定了我们要使用的父镜像。在这种情况下，我们选择在一个完整的 Payara 服务器版本 6.2023.2 之上构建我们的 Docker 镜像，该版本还支持 Java 17。这意味着我们的 Docker 容器将拥有运行 Cargo Tracker 应用程序所需的所有工具，从而无缝运行。

第二行代码是一个复制命令。该命令用于将 Cargo Tracker 应用程序的 WAR 文件复制到一个名为 `$DEPLOY_DIR` 的变量中。我们选择的父 Docker 镜像使用此变量来搜索要部署的应用程序。因此，通过在将此目录放置 Cargo Tracker 的副本，我们确保一旦容器创建，它将被自动部署。

创建 Dockerfile 后，下一步是使用它来告诉 Docker 引擎如何创建容器以及我们希望如何运行它。一旦容器启动并运行，我们就可以使用命令行界面来管理它并查看由 Cargo Tracker 应用程序生成的日志。这将使我们能够监控应用程序的性能并进行任何必要的调整，以确保其顺利运行。

#### 构建容器

下一步是告诉 Docker 引擎使用 Dockerfile 构建我们刚刚指定的容器。为了创建容器，我们将使用 Docker 客户端，它是一个让你与 Docker 守护进程交互并管理容器的命令行界面。使用终端，导航到你刚刚创建 Dockerfile 的 Cargo Tracker 应用程序根目录。在 Cargo Tracker 目录内，执行以下命令：

```
docker build -t cargo-tracker .
```

这告诉 Docker 引擎使用我们在前面步骤中创建的 Dockerfile 为 Cargo Tracker 应用程序创建容器。

如果此命令成功执行，你将看到类似于以下内容出现在控制台中：

```
[+] Building 0.5s (7/7) FINISHED
=> [internal] load build definition from Dockerfile 0.0s
=> => transferring dockerfile: 90B 0.0s
=> [internal] load .dockerignore 0.0s
=> => transferring context: 2B 0.0s
=> [internal] load metadata for docker.io/payara/server-full:6.2023.2-jdk17 0.4s
=> [internal] load build context 0.0s
=> => transferring context: 192B 0.0s
=> [1/2] FROM docker.io/payara/server-full:6.2023.2-jdk17@sha256:6b96c248d0e2cf18d6768883a2881389acfd743b13758b155724233713813f 0.0s
=> CACHED [2/2] COPY target/cargo-tracker.war /opt/payara/deployments 0.0s
=> exporting to image 0.0s
=> => exporting layers 0.0s
=> => writing image sha256:7d7ff9b0a3290c692db4bb3f80b42d96203d67d312f3be0cf6c968b22a6913c5 0.0s
=> => naming to docker.io/library/cargo-tracker
```

此输出告诉你，已经创建了一个名为 `cargo-tracker` 的镜像，你可以将其与 Docker 一起使用。

#### 运行容器

下一步是使用我们在前一步中构建的容器运行 Cargo Tracker 应用程序。要在终端中启动容器，请运行以下命令：

```
docker run -p 8080:8080 cargo-tracker
```

此命令将启动我们上一步创建的名为 `cargo-tracker` 的容器。该命令由三部分组成：

- `run`
- `-p 8080:8080`
- `cargo-tracker`

`run` 告诉 Docker 引擎我们要启动一个容器。`run` 之后的所有内容都是将传递给 Docker 引擎的参数。`-p 8080:8080` 告诉 Docker 引擎我们想要将容器内部的端口映射到外部。如果没有此设置，我们将无法访问我们在容器内部运行的 Payara 服务器。第一个 8080 是宿主机的端口号；第二个 8080 是容器内部的端口。使用此 `-p` 参数，我们现在可以使用 8080 端口连接到也在 8080 端口上运行的容器内部的 Payara 服务器。最后一个参数 `cargo-tracker` 是我们创建的容器的名称。

当你运行该命令时，你会直接在终端中看到容器内部发生的日志记录。几秒钟后，你会在终端中看到类似以下内容：

```
[#|2023-03-26T18:29:09.723+0000|INFO|Payara 6.2023.2||_ThreadID=216;_ThreadName=Thread-24;_TimeMillis=1679855349723;_LevelValue=800;|SSLParams =org.glassfish.admin.mbeanserver.ssl.SSLParams@78a4a59b|#]
[#|2023-03-26T18:29:09.741+0000|INFO|Payara 6.2023.2|javax.enterprise.system.jmx|_ThreadID=216;_ThreadName=Thread-24;_TimeMillis=1679855349741;_LevelValue=800;_MessageID=NCLS-JMX-00025;|SSLServerSocket[/0.0.0.0:8686 and [SSL: ServerSocket[addr=/0.0.0.0,localport=8686]] created|#]
[#|2023-03-26T18:29:09.802+0000|INFO|Payara 6.2023.2|javax.enterprise.system.jmx|_ThreadID=216;_ThreadName=Thread-24;_TimeMillis=1679855349802;_LevelValue=800;_MessageID=NCLS-JMX-00005;|JMXStartupService has started JMXConnector on JMXService URL service:jmx:rmi:///0.0.0.0:8686/jndi/rmi:///0.0.0.0:8686/jmxrmi|#]
```

这表明 Payara 服务器已启动，并且 Cargo Tracker 应用程序已部署。现在你可以通过访问以下 URL 来访问服务器：http://localhost:8080/cargo-tracker/。这与你在本地 Payara 服务器上运行 Cargo Tracker 应用程序时的 URL 相同。但是，你现在连接的不是本地 Payara 服务器，而是容器内部的 Payara 服务器。

当你点击 Cargo Tracker 应用程序的界面时，你会看到日志出现在控制台中。





### 以后台模式运行容器

你也可以使用以下命令以后台模式运行容器：

```bash
docker run -p 8080:8080 cargo-tracker -d
```

请注意，该命令包含一个额外的参数 -d。该参数告诉 Docker 引擎，我们希望以后台模式启动容器。当您以这种方式启动容器时，您将看不到容器的日志。相反，命令会返回您刚启动的容器的容器 ID。

要查看您正在运行的容器，可以使用以下命令：

```bash
docker ps
```

这将在控制台中打印类似以下内容：

```text
CONTAINER        ID     IMAGE          COMMAND                 CREATED       STATUS       PORTS                    NAMES
c94fa6f7accf     cargo-tracker   "/tini -- /bin/sh -c…"   2 minutes ago   Up 2 minutes   4848/tcp, 8181/tcp, 9009/tcp,
0.0.0.0:8080->8080/tcp   inspiring_morse
```

该概览显示以下信息：

- 容器 ID
- 镜像名称
- 命令
- 容器创建时间
- 容器状态
- 端口映射
- 容器名称

当您以后台模式运行容器时，您可以关闭终端，容器将继续在后台运行。

### 停止正在运行的容器

如果您想停止容器，可以使用 kill 命令，并将其作为容器 ID 的一部分传入。以下示例命令会杀死一个容器 ID 以 c 开头的容器：

```bash
docker kill c9
```

该命令在杀死容器后，会打印您传入的 ID。在这种情况下，您将在控制台中看到以下返回值：

```text
c9
```

当 kill 命令执行完毕后，您可以再次运行 docker ps 命令，并注意不再有容器在运行。控制台中的输出将如下所示：

```bash
$ docker ps
CONTAINER
ID      IMAGE      COMMAND      CREATED      STATUS      PORTS      NAMES
```

### 使用 Docker Compose

Docker Compose 是一个强大的工具，用于定义和运行多容器 Docker 应用。它使您能够在单个 YAML 文件中定义应用所需的服务、网络和卷。该文件定义了每个服务的容器、其配置和依赖关系。我们特别感兴趣的是将我们的配置存储在此文件中。这样做可以更轻松地启动容器，因为我们不必指定容器名称或要暴露的端口。我们只需在 docker-compose.yml 中定义一次即可。

#### 创建 Docker Compose 文件

要使用 Docker Compose，我们需要创建一个 Docker 可以使用的 docker-compose.yml 文件。进入 Cargo Tracker 应用的根目录，并创建一个名为 docker-compose.yml 的 YAML 文件。使 docker-compose.yml 的内容如下所示：

```yaml
version: "3.9"
services:
  cargo-tracker-app:
    build: .
    ports:
      - "4848:4848"
      - "8080:8080"
      - "8181:8181"
      - "9009:9009"
```

这就是 docker-compose.yml 文件中我们需要的一切。这个 Docker Compose 文件创建了一个名为 cargo-tracker-app 的服务，它在容器中运行，并在指定端口上暴露其服务。

在第一行，我们定义了此 Docker Compose 文件所使用的版本。该文件使用 3.9 版本。该文件定义了一个名为 cargo-tracker-app 的服务，该服务从当前目录（由 . 指定）构建。ports 部分定义了容器服务可以被访问的端口。在这种情况下，它将容器的 4848、8080、8181 和 9009 端口映射到宿主机上对应的端口，使其可以在外部访问。

build 部分表明该服务将从当前目录中的 Dockerfile 构建。这意味着 Docker Compose 将使用当前目录中的 Dockerfile，通过运行 `docker build` 命令为容器创建镜像。

#### 使用 Docker Compose

要启动 `docker-compose.yml` 中的所有服务，请运行以下命令：

```bash
docker compose up
```

这将使用当前目录中的 Dockerfile 为 Cargo Tracker 应用构建容器并启动它。因为我们在 Docker Compose 文件中指定了端口，所以我们不必在命令中指定它们。当您在控制台中看到以下文本时，您就会知道容器已启动并可以访问：

```text
cargotracker-cargo-tracker-app-1 | cargo-tracker was successfully deployed in 9,380 milliseconds. [#]
```

您现在可以访问以下 URL，查看容器和 Cargo Tracker 应用是否正在运行：`http://localhost:8080/cargo-tracker/`。

要查看所有端口是否已映射到宿主机上对应的端口，请运行以下命令：

```bash
docker ps
```

结果将如下所示：

```text
CONTAINER         IMAGE                       COMMAND                  CREATED         STATUS         PORTS                   NAMES
ID
93a1813c884c      cargotracker-cargo-tracker-app "/tini -- /bin/sh 30 minutes ago  Up 14 seconds  0.0.0.0:4848->4848/tcp,  cargotracker-cargo-tracker-app-1
-c ""                                        0.0.0.0:8080->8080/tcp, 0.0.0.0:8181->8181/tcp, 0.0.0.0:9009->9009/tcp
```

在端口部分下，您可以验证端口的映射方式是否与我们在 `docker-compose.yml` 文件中指定的一致。

#### 停止容器

停止您启动的容器与停止 Docker 中运行的其他容器的方式相同。使用 `docker kill` 命令，您可以停止使用 `docker compose up` 启动的容器。要停止上一个示例中的容器，可以使用以下命令：

```bash
docker kill 93
```

这将返回 93，容器将停止。要验证命令是否已生效，您可以再次运行 docker ps 命令。或者，您也可以使用以下命令：

```bash
docker-compose down
```

这将删除容器、配置文件中定义的网络以及任何默认的网络定义。

### 总结

在本章中，您了解了什么是 Docker，以及如何将其用于 Cargo Tracker 应用。您了解到容器是软件的可执行包，其中包含应用、依赖项以及应用运行所需的系统工具。之后，我们探讨了什么是 Docker 以及它如何使用容器。凭借这些知识，您为 Cargo Tracker 应用指定了第一个 Dockerfile。使用 Docker 命令行界面，您学会了如何构建容器并启动它。您还探索了使用后台模式启动容器的其他方式，以及如何停止容器。最后，您学会了如何创建 Docker Compose 文件以及如何启动它。

这些步骤对于我们接下来几章在云端运行 Cargo Tracker 应用非常重要。
