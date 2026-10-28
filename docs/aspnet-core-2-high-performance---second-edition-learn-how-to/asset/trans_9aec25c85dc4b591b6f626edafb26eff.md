

一旦你创建了应用程序，VS 将在后台恢复 NuGet 包。完成此操作后，按 F5 在调试器中构建并运行你的应用。默认浏览器应打开包含 ASP.NET Core 模板站点的页面，并且 VS 将显示性能跟踪的图表：

![](img/5e416ac29f1bccaad147c6829853fd77_64_0.png)

恭喜！你刚刚创建了一个 ASP.NET Core 2 Web 应用。

## Mac

苹果电脑一直备受青睐，价格也与之相符。它们在开发者中非常受欢迎，尤其是 Web 开发者。也许这是因为它们是唯一可以运行你可能想要测试的所有东西的机器。你可以在虚拟机中运行 Windows 和 Linux，但苹果的 macOS（以前称为 OS X，发音为 OS ten）不会在任何非苹果硬件上运行（没有一些说服力）。

这也可能是由于 iPhone 和 iPad 的 meteoric 崛起，以及随之而来的生态系统。如果你想为 iOS 和 Android 开发应用，那么唯一合理的选择是使用 Mac——尽管，渐进式 Web 应用正在蚕食传统上由原生应用占据的大部分领域。

## Visual Studio for Mac

VS Mac 是 Xamarin Studio 的重新命名，但它也得到了扩展，并采用了大量常规 VS 的功能和代码。它仍然最适合制作 Xamarin 跨平台移动和桌面应用，但它也支持 .NET Core 和 ASP.NET Web 应用。

首先，下载你需要的文件。这包括用于构建应用的软件，例如 VS for Mac 或 VS Code。VS Mac 以 DMG 磁盘映像的形式提供，其中包含安装程序，而 VS Code 则以可解压即用的 ZIP 文件形式提供。你还需要 .NET Core SDK，它以需要安装的 PKG 文件形式提供。你可能还想要一本方便的指南手册：

![](img/5e416ac29f1bccaad147c6829853fd77_65_0.png)

### 安装 VS Mac

VS for Mac 由最新版本的 macOS（High Sierra）支持，但它也适用于较旧的版本，例如 OS X El Capitan（10.11）和 macOS Sierra（10.12）。

2. 在下一个屏幕中，选择 .NET Core 2.0 作为目标框架：

![](img/5e416ac29f1bccaad147c6829853fd77_70_0.png)

3. 最后，添加项目名称并完成向导：

![](img/5e416ac29f1bccaad147c6829853fd77_70_1.png)

你也可以通过打开包含它的文件夹，在 Visual Studio Code 中处理此解决方案：

![](img/5e416ac29f1bccaad147c6829853fd77_72_0.png)

## Visual Studio Code

Visual Studio Code 运行于 Windows、Mac 和 Linux。VS Code 更像是一个文本编辑器而不是 IDE，但它是一个非常强大的编辑器，并且模糊了编辑器与完整 IDE 之间的界限。它支持 IntelliSense 代码补全、构建代码以及调试应用。

它使用 Web 技术（特别是 TypeScript）构建，并在基于独立 Web 浏览器的应用托管环境中运行。然而，它比类似的 Electron 桌面应用（如 Atom 或 Slack）快得多。Azure Web 界面用于编辑代码的相同引擎也在使用。

为了处理巨大的数据文件，最好坚持使用原生文本编辑器，例如 Notepad++（Windows 上）或 Sublime Text。

## Linux

Linux 在桌面市场之外是一个非常流行的操作系统。Android（由 Google 收购的移动操作系统）基于它构建，并且运行在世界上大多数智能手机上。有时它是使用专用硬件的计算机的主要桌面选项，例如 Raspberry Pi。Linux 在嵌入式设备上非常流行，部分原因是它轻量级，通常对资源的需求远低于典型的 Windows 或 macOS 安装。

Linux 也是 Web 服务器最流行的操作系统，这正是我们在这里最感兴趣的。可以说 Linux 是地球上最流行的操作系统，尽管许多用户不会直接与之交互。

![](img/5e416ac29f1bccaad147c6829853fd77_73_0.png)

严格来说，Linux 仅指操作系统的内核。Windows 的等效部分是 NT 内核。许多不同的发行版都是以 Linux 内核为核心构建的，并将其与许多其他（通常是 GNU）应用程序捆绑在一起。你甚至可以使用类似 Buildroot 的东西自己制作一个精简版本。

一些流行的发行版的例子是 Ubuntu 和 CentOS（Community Enterprise OS，是 Red Hat Enterprise Linux 的免费版本）。Linux 发行版可以根据它们使用的软件包管理类型进行广泛分类。当尝试找到要安装的软件的正确版本时，这很有用，尽管你通常可以直接通过发行版的包管理器获取流行的应用程序。

![](img/5e416ac29f1bccaad147c6829853fd77_73_1.png)

如果你以前从未使用过包管理器，那么你会惊讶于它比手动安装软件（或甚至像在 Windows 或 Mac 上通常那样使用 App Store）容易得多。

许多发行版基于 Debian，包括 Mint、Ubuntu 和 Raspbian（Raspberry Pi 的官方操作系统）。这些使用基于 Debian 的软件包格式和 APT 包管理器。Debian 的最新版本称为 Stretch，.NET Core 支持它。它还将其用于官方的 .NET Docker 镜像。

![](img/5e416ac29f1bccaad147c6829853fd77_73_2.png)

Debian 版本以《玩具总动员》角色命名；上一个版本称为 Jessie。

其他发行版使用 **Red Hat Enterprise Linux (RHEL)** RPM 软件包系统和 yum 包管理器。这些包括 CentOS 和 YellowDog，仅举几例。还有 **SUSE Linux Enterprise Server (SLES)**，这是另一种不同的发行版，但 .NET Core 也支持它。

还有许多其他发行版，太多无法在此提及。Linux 不是像 Windows 或 Mac 那样的单一操作系统。我们甚至还没有涵盖基于 BSD 的系统，它们使用不同的内核。这些包括 FreeBSD、OpenBSD 和 NetBSD。甚至 macOS 也是基于 BSD 的，具有其 NeXTSTEP 遗产（运行 CERN 第一个 Web 服务器的操作系统）。

这里的大多数说明也适用于 Windows 和 Mac 的命令行。工具是相同的；甚至 VS 中的图形工具也使用与命令行 `dotnet` 实用程序相同的模板。

## 在 Linux 上开始使用 .NET Core

我们将稍微改变一下，展示 .NET Core 的多功能性和跨平台性。在下面的部分中，我们将演示如何在 Azure 上运行的 Ubuntu Server VM 上仅使用命令行且没有窗口管理器开发一个 .NET Core 应用。

这与你在任何基于 Debian 的发行版上设置的方式非常相似，包括 Ubuntu、Mint 和 Raspbian（用于 Raspberry Pi）。Raspberry Pi 使用定制的 ARM 处理器而不是传统的 Intel x86 CPU 并不重要。当 Mono 最初移植到 Pi 时，专用硬件基于浮点实现存在问题，但这在 .NET Core 中不再是问题。

我们将使用命令行文本编辑器，这里争论 Emacs 与 vi 是不明智的，所以使用你喜欢的那个。如果使用平板电脑（例如带有蓝牙键盘的 iPad）通过 SSH 或 Mosh 连接到云服务器，这种设置可能是一个不错的工作流程。然而，如果你在本地工作并且想要在你选择的 X11/Wayland 桌面环境中使用图形编辑器，那么你可以运行 VS Code 或 JetBrains Rider。毫无疑问你也会大量使用终端窗口。

## 安装 .NET Core 2

本指南中使用的是 Ubuntu Server 16.04 LTS，但这些说明也适用于其他版本。使用 **Advanced Package Tool (APT)**，我们可以非常轻松地安装 .NET Core，但首先我们需要添加一个自定义源，因为 .NET Core 不在默认存储库中。

首先我们需要将 .NET Core 包源添加到 APT 使用的列表中。包源位于 Azure 上，因此我们将此 URL（加上一些其他信息）写入 apt-get 使用的源文件夹中的文件。我们通过将 echo 的标准输出重定向到它，使用以下命令来完成此操作：

```
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
```

所有软件包都使用公钥加密进行签名以验证其完整性（请阅读后面的章节了解更多信息）。我们使用 apt-key 命令从 Ubuntu 密钥服务器导入 .NET Core 包的公钥：

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

我们需要更新我们的源，以便最新的更改可以被获取，使用以下简单命令：

```
sudo apt-get update
```

update 命令与 upgrade 命令不同，但它们通常一起使用。虽然 update 只是刷新 APT 关于软件包的信息，但 upgrade 允许你非常轻松地将机器上所有未手动安装的软件更新到最新版本。这可以节省很多时间。

现在我们可以使用以下命令安装 .NET Core：

```
sudo apt-get install dotnet-sdk-2.0.0
```

如果 SDK 包最终进入默认源列表，那么这就是你需要做的全部。

如果你正在尝试预览版本，那么该包将带有预览标签后缀（例如，-preview2-006497）。

在滚动的文字墙之后，假设你没有收到任何错误并且看到遥测通知，你就可以开始了。输入 dotnet 以查看使用信息，输入 dotnet --version 以查看你当前运行的版本。

