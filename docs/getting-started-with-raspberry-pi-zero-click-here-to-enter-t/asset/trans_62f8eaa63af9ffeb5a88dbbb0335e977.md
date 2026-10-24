

## 使用 Raspberry Pi Zero 入门

如果你使用的是 Windows，你需要使用像 7-Zip 这样的归档程序来解压文件，可在 http://www.7-zip.org/ 获取。这会得到一个带有 .img 扩展名的文件，可以将该文件写入你的卡。接下来，你需要一个可以将映像写入卡的程序。我使用 Image Writer for Windows。你可以在 https://www.raspberrypi.org/ 网站下载部分的顶部找到此程序的链接。将你的卡插入 PC，运行此程序，你应该会看到以下内容：

![](img/324d126a7e2da696837a3e066326bd61_26_0.png)

选择设备卡并下载映像；它应该看起来像这样：

![](img/324d126a7e2da696837a3e066326bd61_26_1.png)

然后，点击 **Write** 按钮。这需要一些时间，可能长达 15 分钟，但完成后，退出程序，你就拥有了带有映像的 microSD 卡。

如果你使用的是 Linux，你需要解压文件然后将其写入卡。你可以用一条命令完成所有操作。但是，你需要找到卡的 /dev 设备标签。你可以用 `ls -la /dev/sd*` 命令来做。如果你在此之前插入卡，可能会看到如下截图：

![](img/324d126a7e2da696837a3e066326bd61_27_0.png)

插入卡后，你可能会看到如下截图：

![](img/324d126a7e2da696837a3e066326bd61_27_1.png)

注意你的卡在 sdb。现在进入你下载了归档映像文件的目录，并使用以下命令：

```bash
sudo dd if=2015-11-21-raspbian-jessie.img of=/dev/sdX
```

`2015-11-21-raspbian-jessie.img` 命令将替换为你下载的映像文件，而 `/dev/sdX` 将替换为你的卡 ID，在此示例中为 `/dev/sdb`。请务必指定正确的设备，因为这可能会覆盖你驱动器上的数据。此外，这可能需要几分钟时间。一旦文件写入完成，弹出卡，你就可以将其插入主板并通电了。

## 使用 Raspberry Pi Zero 入门

确保你的 Raspberry Pi Zero 未通电并将 SD 卡安装到插槽中。然后为设备供电。设备启动后，你应该会看到以下屏幕：

![](img/324d126a7e2da696837a3e066326bd61_28_0.png)

你已经可以正常运行了！

> 请注意，如果你使用带电的 USB 集线器，它可能为你的 Raspberry Pi 提供足够的电力，但在某些情况下可能无法提供你所需的所有电力。我强烈建议你使用不同的电源，一个用于你的 Raspberry Pi Zero，一个用于你的集线器。

你还需要做一件事情来完成配置。为此你需要进入 raspi-config 应用程序。所以，通过点击左上角看起来像一个小电脑屏幕的图标打开一个终端窗口。

现在，输入 sudo raspi-config。你应该会在屏幕上看到此应用程序：

![](img/324d126a7e2da696837a3e066326bd61_29_0.png)

你需要扩展文件系统以占用整张卡。选择 **1 Expand Filesystem**，按 *Enter* 键，你会看到以下屏幕：

![](img/324d126a7e2da696837a3e066326bd61_29_1.png)

你还需要为此过程连接一个带电的 USB 集线器，以便你可以同时访问键盘和鼠标以及 USB 无线 LAN 设备。现在，将设备连接到带电的集线器。

启动系统，然后通过输入 sudo nano /etc/wpa_supplicant/wpa_supplicant.conf 编辑 wpa_supplicant.conf 文件。你需要将其更改为如下所示：

![](img/324d126a7e2da696837a3e066326bd61_31_0.png)

这里的 wpa-ssid 和 wpa-psk 值当然必须与你的无线接入点要求相匹配。重新启动后，你的设备应该已连接到无线网络。你可以通过查看屏幕右上角来判断是否已连接，你应该会看到以下内容：

![](img/324d126a7e2da696837a3e066326bd61_32_0.png)

你现在可以从互联网下载你想要安装的任何附加功能。

如果你想连接到实际的 LAN 有线连接，你需要一个从 USB 到 LAN 的设备。此站点 http://elinux.org/RPi_USB_Ethernet_adapters 列出了许多不同的可能性。以下是此类设备之一的图像：

![](img/324d126a7e2da696837a3e066326bd61_32_1.png)

以这种方式连接 Raspberry Pi Zero 实际上非常简单。只需将 USB 到 LAN 适配器插入带电的 USB 集线器，连接一条活动 LAN 电缆，你就应该可以访问互联网。

## 从主机 PC 访问你的 Raspberry Pi Zero

一旦你为设备建立了互联网网络连接，你就可以从主机计算机访问它。有三种方式可以从远程计算机访问你的系统：

- 第一种是通过名为 **SSH** 的终端界面。
- 第二种方式是使用名为 **VNC server** 的程序。这允许你远程打开一个图形用户界面，该界面镜像 Raspberry Pi 上的图形用户界面。
- 最后，你可以通过名为 **WinSCP** 的程序传输文件，该程序是专门为此目的而定制的。你也可以使用名为 **SCP** 的程序用于 Linux。

所以，首先，确保你的基本系统已启动并正常运行。打开一个终端窗口并检查你的单元的 IP 地址。无论你如何与系统通信，你都需要它。通过使用 `ifconfig` 命令来完成。它应该会产生以下截图：

![](img/324d126a7e2da696837a3e066326bd61_33_0.png)

你需要 inet addr，它显示在前一截图的第三行，以便通过以太网联系你的板。如果你使用无线设备来访问互联网，你的 ifconfig 将如下所示：

![](img/324d126a7e2da696837a3e066326bd61_34_0.png)

与 wlan0 连接关联的 inet addr，在本例中为 10.10.0.31，是你将用于访问 Raspberry Pi 的地址。

你还需要在远程计算机上运行 SSH 终端程序。SSH 终端是一个 Secure Shell（SSH）连接，这意味着你将能够通过在远程计算机上输入命令来访问你的板并向其发出命令。来自 Raspberry Pi Zero 的响应将出现在远程计算机终端窗口中。

> 如果你想了解有关 SSH 的更多信息，请访问：https://www.siteground.com/tutorials/ssh/。

如果你运行的是 Microsoft Windows，你可以下载一个替代应用程序。我个人最喜欢的是 PuTTY。它是免费的，并且在保存配置方面做得非常好，这样你就不必每次都输入它。在搜索引擎中输入 putty，你很快就会来到一个支持下载的页面，或者你可以前往 http://www.putty.org/。

将 PuTTY 下载到你的 Microsoft Windows 机器上。然后，运行 `putty.exe`。你应该会看到一个配置窗口，其外观类似于以下截图：

![](img/324d126a7e2da696837a3e066326bd61_35_0.png)

在 **Host Name** 空格中输入前一页的 `inet addr`，并确保 **SSH** 选择已选中。你可能希望将此配置保存为 Raspberry Pi，以便每次都可以重新加载。

## 使用 Raspberry Pi Zero 入门

当你点击 **Open** 时，系统将尝试通过 LAN 连接在你的 Raspberry Pi 上打开一个终端窗口。第一次这样做时，你会收到有关 RSA 密钥的警告，因为两台计算机彼此不认识。因此 Windows 会抱怨一台它不认识的计算机即将以相当亲密的方式连接。只需点击 **OK**，你应该会得到一个带有登录提示的终端，如以下截图：

![](img/324d126a7e2da696837a3e066326bd61_36_0.png)

现在你可以登录并向你的 Raspberry Pi Zero 发出命令。如果你想从 Linux 机器上执行此操作，过程甚至更简单。打开一个终端窗口，然后输入 `ssh pi@xxx.xxx.xxx.xxx`，其中 `xxx.xxx.xxx.xxx` 是你的设备的 `inet addr`。这将带你进入 Raspberry Pi Zero 的登录屏幕，其外观应与前一截图相似。

**SSH** 是与你的 Raspberry Pi Zero 通信的一个非常有用的工具。然而，有时你需要对系统进行图形化查看，而你不一定想连接到显示器。你可以在远程计算机上通过使用名为 **vncserver** 的应用程序获得此功能。你需要在 Raspberry Pi Zero 上安装此版本的应用程序，通过在 Raspberry Pi Zero 上的终端窗口中输入 `sudo apt-get install tightvncserver`。顺便说一下，这正是使用 SSH 的绝佳机会。

