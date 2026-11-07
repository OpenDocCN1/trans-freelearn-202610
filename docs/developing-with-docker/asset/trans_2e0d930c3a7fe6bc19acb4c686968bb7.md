
## Kitematic

Kitematic 是一个可用于通过简洁而稳健的图形用户界面（GUI）运行容器的软件工具。2015 年，Docker 收购了 Kitematic 团队，希望吸引更多开发者，并希望将容器化解决方案推广给更多开发者以及更广泛的普通公众。

安装 Docker Toolbox 时，Kitematic 现在默认包含在 Mac OS 和 MS Windows 上。你可以使用它从 Docker Hub 舒适地搜索并获取所需的镜像。该工具也可以用于运行你自己的应用容器。使用 GUI，你可以编辑环境变量、映射端口、配置卷、查看日志，并对容器进行命令行访问。值得一提的是，你可以在 Kitematic GUI 和命令行界面之间无缝切换以运行和管理应用容器。Kitematic 非常方便，然而，如果你希望在处理容器时拥有更多控制权或只是想使用脚本——命令行将是更好的解决方案。事实上，Kitematic 允许你在 Docker CLI 和 GUI 之间来回切换。你在命令行界面所做的任何更改都将直接反映在 Kitematic 中。该工具使用简单，正如你在本章末尾将看到的那样，我们将在 Mac 或 Windows PC 上测试我们的设置。在本书的剩余部分，我们将使用命令行界面来操作 Docker。

## Docker Compose

Compose 是一个工具，可从命令行作为 `docker-compose` 执行。它取代了旧的 *fig* 工具。它用于定义和运行多容器 Docker 应用。虽然很容易想象一个多容器应用（例如一个容器中的 Web 服务器和另一个容器中的数据库），但这不是强制性的。因此，如果你决定你的应用可以适配于单个 Docker 容器，`docker-compose` 就没有用途了。在现实生活中，你的应用很可能会跨多个容器。使用 `docker-compose`，你可以使用 compose 文件配置应用的服务，使它们能够在隔离环境中一起运行。然后，使用单个命令，你可以创建并启动配置中的所有服务。对于多容器应用而言，`docker-compose` 对于开发和测试以及持续集成工作流非常有用。

我们将在本书稍后的第 6 章 *创建镜像* 中使用 `docker-compose` 创建 `multicontainer` 应用。

## Oracle VirtualBox

Oracle VM VirtualBox 是 Oracle 提供的用于 x86 计算机的免费开源虚拟化程序。安装 Docker Toolbox 时它将默认安装。它支持创建和管理运行 Windows、Linux、BSD、OS/2、Solaris 等的虚拟机。在我们的案例中，使用 VirtualBox 驱动的 Docker Machine 将使用 VirtualBox 创建并启动一个能够运行 `docker-engine` 的精简 Linux 发行版。值得一提的是，你也可以直接从 VirtualBox 本身运行这个极其精简的虚拟化 Linux。

使用 `docker-machine` 或 `Kitematic` 创建的每个 Docker Machine 在直接运行时都将在 VirtualBox 中可见并可启动，如下图所示：

![](img/0c87321919fa245e22f056c67ec70003_25_0.png)

你可以像其他虚拟化操作系统一样启动、停止、重置、更改设置并读取日志。

![](img/0c87321919fa245e22f056c67ec70003_26_0.png)

你可以在 Windows 或 Mac 上将 VirtualBox 用于 Docker 以外的其他用途。

## Git

Git 是一个分布式版本控制系统，广泛用于软件开发和其他版本控制任务。它强调速度、数据完整性以及对分布式、非线性工作流的支持。Docker Machine 和 Docker 客户端遵循 Git 的 pull/push 模型来从网络获取所需的依赖项。例如，如果你决定运行本地机器上不存在的 Docker 镜像，Docker 将从 Docker Hub 获取该镜像。Docker 在内部不使用 Git 进行任何资源版本控制。然而，它确实依赖哈希来唯一标识文件系统层，这与 Git 所做的事情非常相似。Docker 也从提交、推送和拉取的概念中汲取了最初的灵感。Git 也包含在 Docker Toolbox 安装包中。

从开发者的角度来看，有一些工具对程序员的日常工作特别有用，无论是面向 Java 爱好者的 IntelliJ IDEA Docker Integration Plugin，还是面向偏好 C# 的开发者的 Visual Studio 2015 Tools for Docker。它们让你可以直接从你喜欢的 IDE 下载和构建 Docker 镜像、创建和启动容器，并执行其他相关任务。我们将在下一章中更详细地介绍它们。

除了 Docker 发行包中包含的工具（对于较旧版本的 Windows 为 Docker Toolbox，或为 Docker for Windows 和 Docker for Mac）之外，还有数百种第三方工具，例如 Kubernetes 和 Helios（用于 Docker 编排）、Prometheus（用于统计监控），或用于管理集群的 Swarm 和 Shipyard。随着 Docker 受到更多关注，几乎每周都会出现越来越多的 Docker 相关工具。我们将尝试在第 9 章 Using Docker in Development 以及更多资源中简要介绍其中最有趣的工具。

但这些并不是你可用的唯一工具。此外，Docker 还提供了一套非常实用的 API。其中之一是用于管理镜像和容器的 Remote API。使用此 API，你将能够将镜像分发到运行时的 Docker 引擎。容器可以转移到另一台运行 Docker 的机器上，并在那里执行而无需担心兼容性问题。在创建 PaaS（平台即服务）架构时，这可能特别有用。还有 Stats API，它将公开容器的实时资源使用信息（如 CPU、内存、网络 I/O 和块 I/O）。此 API 端点可用于创建显示容器行为的工具，例如在生产系统上。

## Summary

到目前为止，我们已经理解了虚拟化与容器化之间的区别，而且我希望我们也能看到使用后者的优势。我们也知道有哪些组件可供我们安装和使用。让我们开始进入容器世界之旅，并通过安装软件直接付诸行动。

## 2

## Installing Docker

在本章中，我们将了解如何在 Windows、Mac OS 和 Linux 操作系统上安装 Docker。还将提供逐步说明，介绍如何在云中设置 Docker，并以 Amazon EC2 为例。接下来，我们将运行示例 `hello-world` 镜像以验证设置并检查安装过程后一切是否正常工作。本章将涵盖以下主题：

- 运行 Docker 的硬件要求
- 在 Windows 上安装
- 在 Mac OS 上安装
- 在 Linux 上安装
- 在 Amazon AWS 云上安装

Docker 安装相当直接，但有一些事项你需要关注以使其运行。我们会指出它们以使安装过程轻松无痛。

![](img/0c87321919fa245e22f056c67ec70003_28_0.png)

> 值得一提的是，Linux 是 Docker 的自然环境。Docker 引擎构建在 Linux 内核之上。要使其在 Windows 或 Mac OS 上运行，Linux 内核需要被虚拟化。

Docker Engine 可以通过使用专门用于运行 Docker 容器的轻量级 Linux 发行版在 Mac 和 MS Windows 操作系统上运行。它完全从 RAM 运行，仅重几十兆字节，并在几秒钟内启动。在安装主要 Docker 包——Docker Toolbox 期间，虚拟化引擎 VirtualBox 也将默认安装。因此，你的机器有一些特殊硬件要求。
