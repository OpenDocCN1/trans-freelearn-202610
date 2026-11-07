
## 硬件要求

使用 Docker，您需要一台相对较新的机器，支持硬件级虚拟化：基于 Intel 的 PC 需要 VT-x，AMD 处理器需要 AMD-V。

大多数 Mac 机器开箱即支持，但对于 PC，您需要确保其已启用，并可能在 BIOS 设置中开启。

不同 BIOS 的设置方式不同，只需查找 VT-x / AMD-V 开关即可。

在 Windows 8 中，您可以在任务管理器的性能选项卡中检查虚拟化支持：

|  |  |
|---|---|
| 虚拟化: | 已启用 |
| L1 缓存: | 256 KB |
| L2 缓存: | 2.0 MB |

如果您想在 Windows 7 中检查 PC 是否支持硬件级虚拟化，请查找 Microsoft 硬件辅助虚拟化检测工具。

这是一款免费的小型实用工具，用于检查系统是否支持虚拟化。

下载并运行它以查看报告：

![](img/0c87321919fa245e22f056c67ec70003_29_0.png)

如果您的报告显示硬件辅助虚拟化未启用，您需要检查机器的 BIOS 设置，也许硬件辅助虚拟化支持只是被关闭了。

在这种情况下，请将其打开并重新运行该工具。

如果您的 PC 不支持硬件辅助虚拟化，而您仍决定安装 Docker，则在启动虚拟化 Linux 发行版期间会出现错误。

此外，在 Windows PC 上安装时，您需要确保 Windows 操作系统为 64 位（x64）。

Docker 默认不会在 32 位系统上运行。

我们已从 `Chapter 1, Introduction to Docker` 了解到，默认安装包中可安装哪些 Docker 组件。

了解硬件要求后，让我们下载并安装软件。

## 在 Windows 上安装

Docker 的最新安装指南始终可在 `https://www.docker.com/` 网站上获取。

前往 *开始使用 Docker* 部分，并根据您的操作系统选择安装指南：Windows、Mac OS 或 Linux。

正如前一章所述，您在 Windows 上安装 Docker 有两种选择：可在几乎所有 Windows 版本上运行的 Docker Toolbox，或仅在 Windows 10 或更高版本上运行的 Docker for Windows。

此外，即使您运行的是 Windows 10，也有一些额外要求：必须是 64 位 Windows 10 专业版、企业版和教育版（1511 十一月更新，Build 10586 或更高版本）。

对于 Docker 的原生安装包（Docker for Windows 或 Docker for Mac），您可以选择经过充分测试且可靠的稳定通道，或包含 Docker 团队正在开发的一些实验性功能但可能不稳定且有漏洞的测试版通道。

我建议选择稳定通道。

安装包只是一个单一的 `installDocker.msi` 包。

直接运行它——这是一种无需提问的安装类型。

但是请注意，您需要在 Windows 机器上拥有管理员权限。

安装结束后，您会收到完成通知并被提示运行 Docker，如以下屏幕截图所示：

![](img/0c87321919fa245e22f056c67ec70003_31_0.png)

您现在或稍后可以从 Windows 菜单运行 Docker。

如果您运行它，片刻后您应该会在托盘中看到正在动画的 Docker 图标，表示 Docker 正在启动。

如果是这样，您应该会看到一个弹出消息，提示它已准备好使用：

![](img/0c87321919fa245e22f056c67ec70003_31_1.png)

从现在开始，您可以使用托盘区域中的鲸鱼图标与 Docker Engine 交互。

您需要注意，我们在 *第 1 章，Docker 简介* 中描述的 Kitematic 工具不再是 Docker for Windows 安装包的一部分（Docker Toolbox 中是如此）。

然而，Kitematic 与 Docker for Windows 兼容，也可以继续使用。

您可以单独下载并解压到 `C:\Program Files\Docker\Kitematic`。

Docker Toolbox 的设置略有不同。

Windows 和 Mac OS 的 Toolbox 包可在 `https://www.docker.com/products/docker-toolbox` 获取。

下载 **Docker Toolbox** 包后，运行它。

第一个屏幕会询问您是否允许提供匿名使用统计信息以帮助 Docker 开发者改进软件。

是否允许取决于您的隐私顾虑。

下一个屏幕显示可供安装的组件：

![](img/0c87321919fa245e22f056c67ec70003_32_0.png)

Docker Client for Windows 和 Docker Machine for Windows 是强制性的——没有它们您将无法做任何事情。

对于初始设置，最好保留所有选项的勾选状态。

在撰写本文时，Kitematic 工具处于 alpha 版本，但不用担心，它运行良好。

最后一个安装屏幕会询问您是否希望将 Docker 的二进制文件添加到路径或创建桌面快捷方式，如以下屏幕截图所示：

![](img/0c87321919fa245e22f056c67ec70003_33_0.png)

将 Docker 命令行工具放在路径中将来会非常方便，因此最好勾选此选项。

值得一提的是，您可以选择安装旧的 NDIS5 主机网络过滤驱动程序，而不是默认的 NDIS6。

如果您使用的是 Windows Vista 之前的旧版本 Windows，这可能有助于解决诸如减速或网络问题等一些问题。

如果您选择了 NDIS6 并随后发现问题，只需再次执行安装程序，然后改选 NDIS5 即可。

安装后，运行我们之前描述的 Kitematic 工具。

它将在启动 Docker Linux 虚拟机时显示进度窗口。

成功启动后，Kitematic 会要求您提供 Docker Hub 凭据。

您现在可以登录 **Docker Hub**，创建 Docker Hub 账户，或暂时跳过此过程。

这对于搜索和运行镜像不是强制性的。

我们将用它来创建账户，并在第 5 章 *查找镜像* 中大量使用 Docker Hub。

![](img/0c87321919fa245e22f056c67ec70003_34_0.png)

无论您决定执行还是跳过登录过程，您都将看到 Kitematic 的主窗口。

让我们简要讨论一下。

在左侧，有一个可运行的容器列表——如果您首次运行 Kitematic，则为空。

让我们找一个简单的镜像来运行。

只需在搜索框中开始输入 hello-world，即可列出名称中包含该短语的 Docker Hub 镜像。

尝试不同的搜索以查看 Hub 中有哪些内容。

您可以下载并运行诸如 MySQL 或 Mongo 之类的数据库、诸如 nginx 之类的 Web 服务器，以及更多、更多的内容，只需几次鼠标点击！

最好的是，您的机器上无需安装任何依赖项。

例如，您甚至不需要在机器上安装 Java 即可运行 Jboss 或 Tomcat。

这就是容器化的魔力，这只是开始。

来自官方仓库的镜像之一是著名的 **hello-world** 镜像。

点击它旁边的 **创建**：

![](img/0c87321919fa245e22f056c67ec70003_35_0.png)

现在魔术开始了。

Docker 将从 Hub 获取所选镜像并执行它。

从现在开始，您可以使用 Kitematic 启动、停止、重启和配置您的容器：

![](img/0c87321919fa245e22f056c67ec70003_36_0.png)

正如您在 **容器** 日志部分所看到的，后台发生了很多事情，只为打印 `Hello` from Docker 消息。

但当然，这不是通常的消息。

它来自容器化软件，与标准的 `hello world` 程序相比有很大不同。

首先，它检查 Docker daemon 是否在您的系统上运行，如果是则连接到它。

接下来，它会检查 hello-world 镜像是否在您的本地系统上。

如果没有——在您首次运行时就会如此——它会从 Docker Hub 获取镜像。

Docker 运行镜像并将输出流回给您查看。

让我们从 Docker CLI 做同样的事情。

点击 Kitematic 中的 **Docker CLI** 图标以执行 Windows PowerShell 命令提示符。

您现在不必担心命令行语法——我们将在下一章中解释它。

此外，我们将在整本书中使用命令行工具，因此您将很容易熟悉语法。
