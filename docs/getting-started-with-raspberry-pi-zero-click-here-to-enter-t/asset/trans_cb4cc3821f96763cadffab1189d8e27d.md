

**Tightvncserver** 是一个允许你远程查看完整的 Raspberry Pi Zero 的应用程序。安装完成后，你需要在 Raspberry Pi Zero 的终端窗口中输入 `vncserver` 来启动服务器。然后系统会提示你输入密码，再提示你验证密码，然后询问你是否想要一个只读密码。记住你输入的密码，你需要它通过 VNC Viewer 远程登录。

你需要为远程计算机提供一个 VNC Viewer 应用程序。在我的 Windows 系统上，我使用一个名为 **RealVNC** 的应用程序。启动该应用程序时，会出现以下截图：

![](img/324d126a7e2da696837a3e066326bd61_37_0.png)

输入 **VNC Server** 地址，即你的 Raspberry Pi Zero 的 IP 地址，然后点击 **Connect**。你会看到一个弹出窗口，如下图所示：

![](img/324d126a7e2da696837a3e066326bd61_37_1.png)

## Getting Started with Raspberry Pi Zero

输入你在启动 vncserver 时刚刚输入的密码，然后你应该会看到一个类似以下截图的 Raspberry Pi 图形界面：

![](img/324d126a7e2da696837a3e066326bd61_38_0.png)

现在你可以访问系统的所有功能，尽管如果你进行图形密集型数据传输，速度可能会变慢。

> 有办法让 vncserver 在启动时自动启动。我没有使用过它们；我更喜欢在需要应用程序运行时从 SSH 应用程序输入 vncserver 命令。这样可以保持运行的应用程序最少，更重要的是，减少了安全风险。如果你想在每次启动时启动它，互联网上有多个地方展示如何配置。尝试以下网站：http://www.havetheknowhow.com/Configure-the-server/Run-VNC-on-boot.html。

要从运行例如 Ubuntu 的远程 Linux 计算机查看此 Raspberry Pi 桌面，你可以输入 `sudo apt-get install xtightvncviewer`，然后使用 `xtightvncviewer 10.25.155.110:1` 启动它并提供所选密码。

Linux 具有图形界面的查看器，例如 **Remmina Remote Desktop Client**（选择 **VNC-Virtual Network Computing protocol**），它可以替代 `xtightvncviewer` 使用。以下是 **Remote Desktop Viewer** 的截图：

![](img/324d126a7e2da696837a3e066326bd61_39_0.png)

确保 vncserver 在 Raspberry Pi Zero 上运行。最简单的方法是通过 SSH 登录并在提示符下运行 vncserver。现在，在 **Remote Desktop Viewer** 上点击 **Connect**。在 **Protocol** 选择下，填写屏幕如下，选择 **VNC**，你应该会看到以下截图：

![](img/324d126a7e2da696837a3e066326bd61_40_0.png)

现在，输入 **Host** 互联网地址，确保在末尾包含 :1，然后点击 **Connect**。你需要输入设置的 vncserver 密码，如下图所示：

![](img/324d126a7e2da696837a3e066326bd61_41_0.png)

现在你应该能够看到 Raspberry Pi 的图形屏幕。要从 Mac 或其他 Apple 设备图形化访问 Raspberry Pi Zero，你仍然可以使用 Real VNC Viewer，更多信息请参阅 https://www.realvnc.com/products/。

## Getting Started with Raspberry Pi Zero

我喜欢与 Windows 一起使用的最后一款软件是名为 WinSCP 的免费应用程序。要下载和安装此软件，请访问 https://winscp.net/ 并按照说明下载和安装。安装后，运行程序。它将打开以下对话框：

![](img/324d126a7e2da696837a3e066326bd61_42_0.png)

点击 New，你将得到以下截图：

![](img/324d126a7e2da696837a3e066326bd61_42_1.png)

在这里，在主机名选项卡中填写 IP 地址，在用户名选项卡中填写 pi，并在密码栏中填写密码（不是 vncserver 密码）。点击 Login，你应该会看到警告显示，如下图所示：

![](img/324d126a7e2da696837a3e066326bd61_43_0.png)

主机计算机再次不认识远程计算机。点击 Yes，应用程序将显示以下截图：

![](img/324d126a7e2da696837a3e066326bd61_43_1.png)

![](img/324d126a7e2da696837a3e066326bd61_45_0.png)

点击 Scan 选择器会扫描连接到网络的所有设备。你也可以在 Linux 中执行此操作；Linux 中用于 IP 扫描的一个应用程序称为 Nmap。要安装 Nmap，输入 sudo apt-get install nmap。要运行 Nmap，输入 sudo nmap -sp 10.25.155.1/154，扫描器将扫描从 10.25.155.1 到 10.25.155.154 的地址。

> 有关 Nmap 的更多信息，请参阅：http://www.linux.com/learn/tutorials/290879-beginners-guide-to-nmap。

这些扫描器会让你知道哪些地址正在使用，这应该能让你在不输入 ipconfig 的情况下看到你的 Raspberry Pi 地址。

## Getting Started with Raspberry Pi Zero

你的系统具有许多功能。随意尝试系统——尝试了解其中已有的内容以及从软件角度需要添加的内容。请记住，你的 USB 端口功率有限，因此请确保你熟悉插入 Raspberry Pi 的配件的功耗需求。对于许多项目，你可能需要使用带电的 USB 集线器。

## Summary

恭喜！你已完成旅程的第一阶段。你的 Raspberry Pi Zero 已启动并运行。不会让这块硬件在垃圾箱里积灰。它现在已准备好以各种有趣的方式连接到各种有趣的设备。到现在为止，你已经安装了 Raspbian 操作系统，学会了如何连接所有适当的外部设备，甚至掌握了如何远程访问系统，这样你需要的唯一连接就是电源线和 LAN 线。

现在，你已准备好开始指挥你的 Raspberry Pi Zero 执行某些操作。下一章将介绍 Linux 操作系统和 Emacs 文本编辑器。它还将展示 Python 和 C/C++ 编程语言中的一些基本编程概念。然后，你就可以将开源软件添加到廉价硬件上并开始构建你的机器人项目。

## 2
### Programming Raspberry Pi Zero

现在你的系统已启动并运行，你的 Raspberry Pi Zero 已准备好执行某些操作。这需要你创建自己的程序或编辑他人的程序。在本章中，你将学习如何编辑文件以创建可在 Raspberry Pi Zero 上运行的程序。

在本章中，我们将涵盖以下主题：

- Basic Linux commands and navigating the filesystem on Raspberry Pi Zero
- Creating, editing, and saving files on Raspberry Pi Zero
- Creating and running Python programs on Raspberry Pi Zero
- Some of the basic programming constructs in Python on Raspberry Pi Zero

你可以通过将键盘、鼠标和显示器连接到 Raspberry Pi Zero，或通过 vncserver 或 SSH 远程登录来创建和运行本章中讨论的程序。

## Powering up Raspberry Pi Zero with Linux

完成第 1 章 Getting Started with Raspberry Pi Zero 中讨论的任务后，你将拥有一个运行名为 Raspbian 的 Linux 版本的工作 Raspberry Pi Zero。因此，启动你的 Raspberry Pi Zero 并使用有效的用户名和密码登录。如果你打算通过 SSH 或 vncserver 远程登录，现在就建立连接。首先，你将快速浏览 Linux。这不会很深入，但你将只是浏览一些基本命令。

## Programming Raspberry Pi Zero

登录后，打开一个终端窗口。如果你使用键盘、鼠标和显示器登录，或使用 vncserver，你会通过在屏幕顶部选择图标集选择中的 Terminal 应用程序找到终端，如下图所示：

![](img/324d126a7e2da696837a3e066326bd61_48_0.png)

如果你使用 SSH，你已经在终端模拟器程序中了。无论哪种方式，终端应该看起来类似于以下截图：

![](img/324d126a7e2da696837a3e066326bd61_48_1.png)

