
## 安装 Docker

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

## 运行 Docker 容器

在本节中，你将获取 Cargo Tracker 应用程序，为其构建一个 Dockerfile，并将其作为 Docker 容器在你的系统上运行。我们将首先创建一个你可以使用的 Dockerfile。使用 Dockerfile，我们将告诉 Docker 引擎如何为我们创建容器以及我们希望如何运行它。在容器运行后，我们将查看如何使用命令行界面来管理容器并查看 Cargo Tracker 应用程序生成的日志。创建此 Dockerfile 将是我们通往在 Docker 上运行 Cargo Tracker 旅程的起点。

### 创建 Docker 容器

在 Docker 引擎上运行 Cargo Tracker 的第一步是创建一个 Dockerfile。Dockerfile 包含给 Docker 引擎的指令，说明如何创建我们想要的容器。开始时在 Cargo Tracker 应用程序的根目录下创建一个名为 Dockerfile 的文件。在 Dockerfile 内部，你需要添加以下两行代码：

```
FROM payara/server-full:6.2023.2-jdk17

COPY target/cargo-tracker.war $DEPLOY_DIR
```

第一行代码指定了我们要使用的父镜像。在这种情况下，我们选择在一个完整的 Payara 服务器版本 6.2023.2 之上构建我们的 Docker 镜像，该版本还支持 Java 17。这意味着我们的 Docker 容器将拥有运行 Cargo Tracker 应用程序所需的所有工具，从而无缝运行。

第二行代码是一个复制命令。该命令用于将 Cargo Tracker 应用程序的 WAR 文件复制到一个名为 `$DEPLOY_DIR` 的变量中。我们选择的父 Docker 镜像使用此变量来搜索要部署的应用程序。因此，通过在将此目录放置 Cargo Tracker 的副本，我们确保一旦容器创建，它将被自动部署。

创建 Dockerfile 后，下一步是使用它来告诉 Docker 引擎如何创建容器以及我们希望如何运行它。一旦容器启动并运行，我们就可以使用命令行界面来管理它并查看由 Cargo Tracker 应用程序生成的日志。这将使我们能够监控应用程序的性能并进行任何必要的调整，以确保其顺利运行。

### 构建容器

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

### 运行容器

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

