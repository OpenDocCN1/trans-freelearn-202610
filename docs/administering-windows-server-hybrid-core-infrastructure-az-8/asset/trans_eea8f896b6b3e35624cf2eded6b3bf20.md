

- `docker commit`: 此命令将对容器所做的更改提交到新的容器映像
- `docker stop`: 此命令停止正在运行的容器
- `docker rm`: 此命令删除现有的容器

Docker 方法允许您将映像作为代码存储，使用开发生命周期执行持续集成，并为升级和维护目的重新创建快速且可重复的映像，从而获得一致的结果。

容器还可以通过基于浏览器的 GUI 进行管理，该 GUI 称为 **Windows 管理中心 (WAC)**。必须安装 **容器扩展**，以便在 WAC 中查看容器管理。如以下屏幕截图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_368_0.png)

图 10.3 – 为 WAC 安装容器扩展

安装扩展后，如果主机上不存在 Docker，系统将提示您安装 Docker，如以下屏幕截图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_369_0.png)

图 10.4 – 通过 WAC 安装 Docker

以下屏幕截图显示了 WAC 容器管理体验：

![](img/cf35f46a8dd5f11e58349c1d81611b75_369_1.png)

图 10.5 – 使用 WAC 进行容器管理

现在，您可以指定要从 MCR 运行的映像。

## 维护容器映像

应定期更新容器映像，以确保它们接收通过代码和修补程序所做的更改。

基础 OS 层包含主机 OS 不共享的 OS 元素。它由两个映像组成：基础层和更新层。基础层是较大的层，但更改最少，所需维护最少；更新层较小。以下图表表示这些层：

![](img/cf35f46a8dd5f11e58349c1d81611b75_370_0.png)

图 10.6 – 容器映像更新层

在更新层中，每个代码更改都会创建一个新映像。基础 OS 层从更新层获取其更新。

在本节中，我们探讨了管理 Windows 容器。在下一节中，我们将完成一个动手练习，以巩固本章中涵盖的一些概念。

## 动手练习

为了在获得一些实用技能的同时支持您的学习，我们将利用从本章获得的概念和理解，并将其付诸实践。

我们将查看以下练习：

- 练习 – 在 Windows Server 上使用 PowerShell 安装 Docker
- 练习 – 在 Windows Server 上使用 WAC 安装 Docker

## 开始使用

要开始本节，您需要访问 Windows 服务器；它可以是物理机、虚拟机或 Azure IaaS VM。您需要访问提升的 PowerShell 会话，并以管理员访问权限安装 WAC。

让我们进入本章的练习。

## 练习 – 在 Windows Server 上使用 PowerShell 安装 Docker

在此练习中，我们将使用 PowerShell 在 Windows Server 上安装 Docker。为此，我们将使用 Windows Server 2019 Gen2 VM。

按照以下步骤开始：

1. 使用管理员帐户登录到用于此练习的 Windows Server 并打开提升的 PowerShell 会话。

2. 运行以下命令，从 Windows PowerShell 库安装 Docker-Microsoft PackageManagement 提供程序：

```
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
```

3. 使用 PackageManagement Windows PowerShell 模块安装最新版本的 Docker：

```
Install-Package -Name docker -ProviderName DockerMsftProvider
```

以下是命令的输出：

| 名称 | 版本 | 源 | 摘要 |
| --- | --- | --- | --- |
| Docker | 20.10.9 | DockerDefault | 包含用于 Windows Server 的 Docker EE。 |

图 10.7 – 安装 Docker

4. Docker 安装完成后，使用以下命令重新启动主机：

```
Restart-Computer
```

5. 重新登录服务器并再次打开提升的 PowerShell 会话。

6. 运行以下命令以确保 Docker 服务在主机上运行：

```
Start-Service docker
```

7. 运行以下命令以检查已安装的 Docker 版本：

```
docker version
```

```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\vmadmin> Start-Service docker
PS C:\Users\vmadmin> docker version
Client: Mirantis Container Runtime
 Version:           20.10.9
 API version:       1.41
 Go version:        go1.16.12m2
 Git commit:        591094d
 Built:             12/21/2021 21:34:30
 OS/Arch:           windows/amd64
 Context:           default
 Experimental:      true

Server: Mirantis Container Runtime
Engine:
 Version:          20.10.9
 API version:      1.41 (minimum version 1.24)
 Go version:       go1.16.12m2
 Git commit:       9b96ce992b
 Built:            12/21/2021 21:33:06
 OS/Arch:          windows/amd64
 Experimental:     false

PS C:\Users\vmadmin> _
```

图 10.8 – 检查 Docker 版本

至此，我们已完成此练习。此练习教会了我们如何在 Windows Server 上使用 PowerShell 安装 Docker。

## 练习 – 在 Windows Server 上使用 WAC 安装 Docker

在此练习中，我们将使用 Windows 管理中心 (WAC) 在 Windows Server 上安装 Docker。为此，我们将使用 Windows Server 2019 Gen2 VM。

按照以下步骤开始：

1. 登录到已安装 WAC 的 Windows Server，并使用管理员帐户启动 WAC。
2. 登录到 WAC 后，单击右上角工具栏中的齿轮图标以访问设置屏幕，然后单击 扩展：

![](img/cf35f46a8dd5f11e58349c1d81611b75_373_0.png)

图 10.9 – WAC 设置屏幕

3. 从扩展屏幕，搜索 容器扩展：

![](img/cf35f46a8dd5f11e58349c1d81611b75_373_1.png)

图 10.10 – WAC 容器扩展

4. 单击 容器扩展，然后单击 安装：

![](img/cf35f46a8dd5f11e58349c1d81611b75_374_0.png)

图 10.11 – WAC 正在安装扩展

5. 已安装的 容器扩展可以在 已安装的扩展 选项卡中查看：

### 扩展

安装扩展后，Windows 管理中心 可能会重新启动，暂时影响使用此 Windows 管理中心 实例的任何人。

![](img/cf35f46a8dd5f11e58349c1d81611b75_374_1.png)

图 10.12 – WAC 扩展已安装

6. 从左上角菜单导航中，选择 **所有连接**。然后，选择 **网关** 条目以建立连接：

![](img/cf35f46a8dd5f11e58349c1d81611b75_375_0.png)

图 10.13 – WAC 正在建立连接

7. 从左侧工具菜单，单击 容器：

![](img/cf35f46a8dd5f11e58349c1d81611b75_375_1.png)

图 10.14 – 打开容器屏幕

8. 页面加载后，单击 安装：

![](img/cf35f46a8dd5f11e58349c1d81611b75_376_0.png)

图 10.15 – WAC 正在安装 Docker

9. 等待片刻，正在安装 Docker。安装完成后，您的计算机将重新启动：

![](img/cf35f46a8dd5f11e58349c1d81611b75_376_1.png)

![](img/cf35f46a8dd5f11e58349c1d81611b75_376_2.png)

图 10.16 – Docker 已安装

10. 计算机重新启动后，启动 WAC。您将看到 Docker 已安装，并且您已准备好创建和管理容器：

![](img/cf35f46a8dd5f11e58349c1d81611b75_377_0.png)

图 10.17 – 从 WAC 访问 Docker

至此，我们已完成此练习。此练习教会了我们如何在 Windows Server 上使用 WAC 安装 Docker。现在，让我们总结本章。

## 摘要

本章通过涵盖 AZ-800 管理 Windows Server 混合核心基础架构：创建和管理容器，帮助您进一步了解容器技术的知识和技能。

首先，您了解了容器并将其与虚拟机进行了比较。然后，您学习了如何创建和管理 Windows Server 容器映像并执行容器网络。

这些新技能将使您的知识超越考试目标，为您从事以现实世界日常混合环境为重点的角色做好准备。

在下一章中，您将学习如何管理 Windows Server 虚拟机。

