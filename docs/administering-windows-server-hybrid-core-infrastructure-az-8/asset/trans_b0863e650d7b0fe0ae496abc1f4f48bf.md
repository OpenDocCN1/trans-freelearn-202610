

![](img/cf35f46a8dd5f11e58349c1d81611b75_363_0.png)

图 10.1 – 物理机与虚拟机与容器

前面的图示表明，简而言之，提供相同数量的应用所需的物理服务器更少。

仔细观察，我们可以看到，传统的每台服务器运行单个应用的方式意味着我们需要两台物理服务器来提供两个应用。该图示表明，借助虚拟化，相同的两个应用现在只需要一台物理服务器。不过，我们仍需为每个虚拟机购买、维护、保护和支持操作系统，这可能会带来相当大的成本和运维负担。然而，使用容器时，我们提供相同数量的应用，并且仍然使用与虚拟化相同数量的物理服务器，但只需要一份操作系统副本。

总而言之，使用容器时，我们对软件进行虚拟化；这允许多个计算实例（容器）共享单个主机操作系统的底层主机软件资源。通过这样做，我们能够实现更高的效率、规模和敏捷性。

本节介绍了容器，并将其与虚拟机和传统物理服务器模型进行了比较，以帮助你了解在容器中托管和执行应用程序代码的价值和优势。在下一节中，我们将介绍 Docker。

## Docker 简介

Docker（https://www.docker.com）是一个开源的容器化平台，它将应用程序的代码打包成我们在本章前面几节中介绍的标准化计算单元。由于 Docker 是开放标准，它可以在所有主要操作系统类型、任何基础设施和任何云平台上运行。

Docker 平台的两个核心组件如下：

- Docker 引擎：这是一个轻量级、安全的运行时环境，可运行在基于 Windows 的操作系统、Linux 和 macOS 上
- Docker 客户端：这是引擎的命令行界面 (CLI)，并在主机计算机上运行

Docker 可以使用包管理在 Windows Server 上安装。PowerShell PackageManagement 模块在 Windows Server 2019 及更高版本中可用。

Docker 还可以使用 PowerShell DockerMicrosoftProvider 模块进行安装。

我们将在本章的实操练习部分学习如何安装 Docker。

在本节中，我们介绍了 Docker。在下一节中，我们将介绍保护 Windows Server 容器。

## 保护 Windows Server 容器

本节在隔离模式的背景下探讨安全性。

Windows Server 容器提供两种隔离模式：进程隔离和 Hyper-V 隔离。每个容器与主机操作系统之间创建的隔离程度区分了这些隔离模式。让我们逐一介绍每种模式：

- 进程隔离（这是传统的隔离模式）：
  - 容器与主机之间共享相同的内核
  - 每个容器都有用户模式
  - 启动 Docker 容器时，使用 --isolation=process 命令开关
- Hyper-V 隔离：
  - 使用这种隔离模式，每个容器都有自己的内核
  - 每个容器在虚拟机内运行
  - 每个容器与主机之间提供硬件级别的隔离
  - 启动 Docker 容器时，使用 --isolation=hyperv 命令开关

以下图示展示了这两种隔离模式：

![](img/cf35f46a8dd5f11e58349c1d81611b75_365_0.png)

![](img/cf35f46a8dd5f11e58349c1d81611b75_365_1.png)

图 10.2 – 隔离模式

进程隔离是 Windows Server 主机上 Windows 容器的默认模式。在 Windows 10 主机上运行的 Windows 容器使用 Hyper-V 隔离。

在本节中，我们在 Windows 容器的隔离背景下探讨了安全性。在下一节中，我们将介绍 Windows 容器中的网络。

## Windows Server 容器的网络

使用 Windows Server 容器进行网络的概念在功能上与虚拟机类似。一个虚拟网络适配器 (vNIC) 连接到容器，该容器连接到 Hyper-V 虚拟交换机 (vSwitch)。可以通过 Docker 创建五个网络驱动程序或模式来支持 Windows，它们如下：

- L2bridge 网络驱动程序
- L2tunnel 网络驱动程序
- NAT 网络驱动程序
- Overlay 网络驱动程序
- Transparent 网络驱动程序

有关每个驱动程序的更多详细信息，请参阅以下 Microsoft 文档文章：https://docs.microsoft.com/en-gb/virtualization/windowscontainers/container-networking/network-drivers-topologies。

根据物理网络和主机网络要求，应选择最适合您需求的驱动程序。

您可以运行 `docker network ls` 命令来列出可用网络。

NAT 是 Docker 引擎首次运行时创建的默认网络类型。它使用 **WinNAT** Windows 组件和内部 vSwitch。

有两个 vSwitch 可用，它们如下：

- **内部 vSwitch**：这不直接连接到容器主机上的网络适配器
- **外部 vSwitch**：这直接连接到容器主机上的网络适配器

在本节中，我们介绍了 Windows Server 容器中的网络。在下一节中，我们将介绍创建 Windows Server 容器映像的过程。

## 创建 Windows Server 容器映像

基础映像为容器提供操作系统的基础层；所有容器都将基于该映像。基础映像包含用户模式操作系统文件、运行时文件、必需的应用程序依赖项以及任何必需的杂项配置文件。

Microsoft 为 Windows Server 容器提供以下基础映像：

- **Windows**：这包括完整的 Windows API 和系统服务，但不包含服务器角色。它在 Windows Server 2022 上不可用。这是最大的映像 (3.4 GB)。
- **Windows Server**：这包括完整的 Windows API，比 Windows 映像具有更多的服务器功能，支持 GPU，并且没有 IIS 限制。它需要 Windows Server 2022，并且比 Windows 映像略小 (3.1 GB)。
- **Server Core**：这与 Nano Server 是最常用的映像。它包括 Windows API 的子集，包括 **.NET Framework** 和大多数服务器角色，以及中等大小的映像。它最适合 **直接迁移** 场景。
- **Nano Server**：这包括 **.NET Core** API 和一些服务器角色；它是一个超轻量映像。关键的映像差异是 API 表面显著更小；PowerShell、WMI 和 Windows 服务堆栈不包括在内。

以下信息总结了针对给定场景应使用的适当基础映像：

- *应用程序是否需要完整的 .NET Framework？* 您应该使用 **Windows Server Core** 基础映像。
- *该应用是否基于 .NET Core？* 您应该使用 Nano Server 基础映像。
- *您的应用是否需要 Windows Server Core 中缺少的依赖项？* 您应该使用 Windows 基础映像。
- *是否需要 GPU 加速支持？* 您应该使用 Windows Server 基础映像。

这些基础映像可以在 Docker Hub 上找到：https://hub.docker.com/_/microsoft-windows-base-os-images。

可以通过运行以下 PowerShell 命令找到可用的所有 Microsoft 容器基础映像列表：

```
docker search Microsoft
```

这些映像可以从 **Microsoft 容器注册表 (MCR)** 下载。

要下载基础映像，可以使用 `docker pull` 命令。以下是示例：

```
docker pull mcr.microsoft.com/windows/servercore:ltsc2022
```

下载映像后，可以运行以下命令来验证现在本地可用的哪些映像并显示元数据：

```
docker image ls
```

在本节中，我们介绍了使用 Docker 进行基于 Microsoft 的容器创建。在下一节中，我们将介绍管理 Windows Server 容器映像。

## 管理 Windows Server 容器映像

Windows 容器通过 Docker 创建、运行和管理。**Docker 引擎** 提供有助于自动化创建过程的工具。实现此功能的 Docker 组件如下：

- **Docker 文本文件**，Dockerfile：此文件包括创建容器映像的说明。这包括识别将要使用的基础映像、创建映像时将要运行的命令，以及识别新实例的映像部署时间。
- **Docker 命令**，`docker build`：此命令调用 Dockerfile，随后触发映像创建过程。

除了自动化之外，Docker 命令还可以手动运行。以下是一些常用命令供参考：

- `docker images`：此命令列出容器主机上安装的映像
- `docker run`：此命令使用容器映像创建容器

