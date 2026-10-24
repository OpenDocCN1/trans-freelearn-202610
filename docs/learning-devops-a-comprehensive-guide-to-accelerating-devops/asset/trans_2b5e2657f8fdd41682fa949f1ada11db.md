

> 注意
> 关于 WSL 的更多信息，请阅读此处的文档：
> https://docs.microsoft.com/en-us/windows/wsl/
> install
> 关于 Docker Desktop 要求的更多详细信息在此处指定：
> https://docs.docker.com/desktop/windows/install/

要安装 Docker Desktop（它与适用于 Windows 和 macOS 的 Docker 安装程序是相同的二进制文件），请执行以下步骤：

1. 首先，通过单击安装文档页面 https://docs.docker.com/desktop/windows/install/ 上的 **Docker Desktop for Windows** 按钮下载 Docker Desktop，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_290_0.png)

图 9.3 – Docker Desktop 下载链接

2. 下载完成后，单击下载的 **可执行文件 (EXE)**。
3. 然后，执行单个配置步骤，即安装 WSL 2 后端所需组件的可能性，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_290_1.png)

## 配置

![](img/d3344b39c888aab0b832625465f837c9_290_2.png)

图 9.4 – Docker Desktop 配置

在我们的示例中，我们将勾选此选项，以使用 WSL 2 作为后端安装 Windows 组件。

4. 安装完成后，我们将收到确认消息和一个关闭安装的按钮，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_291_0.png)

图 9.5 – Docker Desktop 安装结束

5. 最后，要启动 Docker，请启动 Docker Desktop 程序。通知栏中将出现一个图标，指示 Docker 正在启动。然后，它将通过一个小窗口要求您登录 Docker Hub。Docker Desktop 的启动步骤如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_291_1.png)

图 9.6 – 从 Docker Desktop 登录 Docker Hub

就这样！我们已在 Windows 上安装并启动了 Docker。

要在其他操作系统上安装 Docker，可以在 https://docs.docker.com/get-docker/ 阅读每个操作系统的文档。之后，您可以从此页面选择所需的目标操作系统，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_292_0.png)

要检查 Docker 安装，请打开终端窗口（在 Windows PowerShell 终端上也适用）并执行以下命令：

`docker --help`

您应该能看到类似这样的内容：

![](img/d3344b39c888aab0b832625465f837c9_293_0.png)

图 9.8 – docker --help 命令

如上图所示，该命令显示 Docker 客户端工具中可用的不同操作。

在详细了解 Docker 命令的执行之前，了解 Docker 的概念非常重要。

## Docker 元素概览

在执行 Docker 命令之前，我们将讨论 Docker 的一些基本元素，即 **Dockerfile**、**容器** 和 **卷**。

首先，需要知道 **Docker 镜像** 是 Docker 的基本元素，由名为 Dockerfile 的文本文档组成，其中包含我们要容器化的二进制文件和应用程序文件。

**Docker 注册表** 是共享 Docker 镜像的集中式存储系统。此注册表可以是公共的（如 Docker Hub 的情况），也可以是私有的，例如 **Azure Container Registry (ACR)** 或 JFrog Artifactory。

容器是从 Docker 镜像执行的实例。可以在容器中拥有同一镜像的多个实例，应用程序将在其中运行。最后，卷是物理位于主机操作系统（即容器外部）上的存储空间，如果需要，可以在多个容器之间共享。此空间将允许存储持久元素，例如文件或数据库。

要操作这些元素，我们将使用命令行，这些命令行将在本章后续内容中讨论。

在本节中，我们讨论了 Docker Hub 和创建帐户的不同步骤。然后，我们查看了在本地安装 Docker Desktop 的步骤，最后，我们以 Docker 元素概览结束。

我们现在将开始使用 Docker，我们将要查看的第一个操作是从 Dockerfile 创建 Docker 镜像。

## 创建 Dockerfile

一个基本的 Docker 元素是名为 **Dockerfile** 的文件，其中包含构建 Docker 镜像的逐步说明。

要了解如何创建 Dockerfile，我们将查看一个示例，该示例允许我们构建包含 Apache Web 服务器和 Web 应用程序的 Docker 镜像。

让我们开始编写 Dockerfile。

## 编写 Dockerfile

要编写 Dockerfile，我们将首先创建一个 **超文本标记语言 (HTML)** 页面，它将是我们的 Web 应用程序。因此，我们将创建一个新的 `appdocker` 目录，并在其中创建一个 `index.html` 页面，其中包含在网页上显示欢迎文本的示例代码，如下所示：

```
<html>
<body>
<h1>Welcome to my new app</h1>
This page is test for my demo Dockerfile.<br />
Enjoy ...
</body>
</html>
```

然后，在同一目录中，我们创建一个 Dockerfile（不带扩展名），其内容如下，我们将在稍后详细说明：

```
FROM httpd:latest
COPY index.html /usr/local/apache2/htdocs/
```

要创建 Dockerfile，请从 FROM 语句开始。必需的 FROM 语句定义基础镜像，我们将使用它来构建 Docker 镜像——任何 Docker 镜像都是从另一个 Docker 镜像构建而来。此基础镜像可以保存在 Docker Hub 或另一个注册表中，例如 JFrog Artifactory、Nexus Repository 或 ACR。

在我们的代码示例中，我们使用标记为最新版本的 Apache httpd 镜像，https://hub.docker.com/_/httpd/，并使用 FROM httpd:latest Dockerfile 指令。

然后，我们使用 COPY 指令执行镜像构建过程。Docker 将我们刚刚创建的本地 index.html 文件复制到镜像的 /usr/local/apache2/htdocs/ 目录中。

> 注意
> 此 Dockerfile 和 HTML 页面的源代码可在此处找到：
> https://github.com/PacktPublishing/Learning-
> DevOps-Second-Edition/tree/main/CHAP09/appdocker.

我们刚刚查看了 Dockerfile 的 FROM 和 COPY 指令，但还有其他指令，我们将在下一节中介绍。

## Dockerfile 指令概览

我们之前提到，Dockerfile 文件由指令组成，我们还查看了使用 FROM 和 COPY 指令的具体示例。还有其他指令将允许您构建 Docker 镜像。以下是可用于此目的的主要指令概览：

- FROM：此指令用于定义我们镜像的基础镜像，如上一节 _编写 Dockerfile_ 中详细示例所示。
- COPY 和 ADD：用于将一个或多个本地文件复制到镜像中。ADD 指令支持额外的两个功能，即引用 **统一资源定位符 (URL)** 和提取压缩文件。

> 注意
> 关于 COPY 和 ADD 之间差异的更多详细信息，您可以阅读此文章：https://nickjanetakis.com/blog/docker-tip-2-the-difference-between-copy-and-add-in-a-dockerfile.

- RUN 和 CMD：这些指令将命令作为参数，该命令将在镜像构建期间执行。RUN 指令创建一个层，以便可以缓存和版本化。CMD 指令定义在调用运行镜像时要执行的默认命令。CMD 指令可以在运行时通过提供额外参数来覆盖。

您可以在 Dockerfile 中编写以下 RUN 指令示例来执行 apt-get 命令：

```
RUN apt-get update
```

使用上述指令，我们更新镜像中已存在的 apt 软件包并创建一个层。我们还可以使用 CMD 指令，如以下示例所示，该示例将在执行期间显示 docker 消息：

```
CMD "echo docker"
```

- ENV：此指令允许您实例化可用于构建镜像的环境变量。这些环境变量将在容器的整个生命周期中持续存在，如下所示：

```
ENV myvar=mykey
```

上述命令将 mykey 值设置为容器中的 myvar 环境变量。

- WORKDIR：此指令给出容器的执行目录，如下所示：

```
WORKDIR usr/local/apache2
```

这就是 Dockerfile 指令的概览。还有其他常用指令，例如 EXPOSE、ENTRYPOINT 和 VOLUME，您可以在 https://docs.docker.com/engine/reference/builder/ 的官方文档中找到它们。

