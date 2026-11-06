
## wlan0 接口

当我们添加无线适配器时，ifconfig 会使用 wlan 而不是 eth 前缀来表示这些接口。所有细节都相同，但有助于直观区分有线网络和无线网络。

## 回环（LO）接口

这被称为回环地址，因为它指的是本地设备，并且有一个保留范围，从 127.0.0.1 开始，到 127.255.255.254 结束。这是一个虚拟接口，绕过本地硬件接口和规则。它通常因安全原因和在软件测试期间而使用。例如，您可能只允许 MySQL 对 localhost 的 root 访问（这将包括 IPv4 的 127.0.0.1 或 IPv6 的 ::1）。这意味着只有计算机上的用户或服务才会被授予访问此资源的权限。它是保留的，localhost 也不可以 127.x.y.z 分配给互联网上的用户。

## 无线配置（Wi-Fi）

Wi-Fi 是一种方便的方式，让您的 Raspberry Pi 在无线网络内的无线环境中运行。对于服务器来说，这并不是理想的解决方案，因为传输数据时延迟会增加，而且许多 Wi-Fi 网络会受到同一频率上其他网络产生的噪声影响。

有许多类型的无线适配器可用，并非所有驱动程序都包含在 Raspbian 中。您可能需要安装特定的驱动程序，但这通常是一个简单的过程。谈到无线配置时，您需要针对适配器上使用的芯片，而不是销售它的最终厂商。

## 推荐的无线适配器

最稳定且价格实惠的芯片组之一是 Realtek RT8191，它支持 802.11n 标准。它也兼容较旧的 802.11b/g 规范，以防您的路由器不支持更新的 802.11n 规范。然而，它不支持 5 GHz 频率或 40 MHz 双频带。

![](img/a8753cacc9d454dfc09eda1ed5012bd3_39_0.png)

> 在 http://elinux.org/RPi_USB_Wi-Fi_Adapters 上有兼容适配器的全面列表。

多方比较以找到最佳价格，最好不要选择那些声称是 Raspberry Pi 唯一和最佳的昂贵适配器。

![](img/a8753cacc9d454dfc09eda1ed5012bd3_40_0.png)

> 您应该注意，默认情况下，无线和有线网络不能以组合模式工作。配置 Wi-Fi 后，您需要移除有线连接并重新启动 Raspberry Pi。这将正确配置无线接口。连接有线连接通常会在 Raspberry Pi 启动后使无线连接掉线。

## 从桌面设置

在桌面上使用菜单栏中找到的 Wi-Fi 配置工具是几秒钟内连接到无线网络最方便的方式。如果您刚刚安装了 Raspbian 并计划使用 Wi-Fi 而不是以太网连接，建议您从这里开始。您的 SSID，即无线接入点的可读名称，必须由您的路由器广播才能工作。如果您将其隐藏，则应改用控制台方法。按照以下步骤设置桌面：

- 1. 在右侧菜单栏末尾左键单击网络图标。
- 2. 所有未隐藏的网络都将显示。
- 3. 左键单击您想要连接的网络。
- 4. 输入您的密码并单击确定。
- 5. 经过几秒钟后，图标将停止闪烁。
- 6. 您可以通过右键单击网络图标来调整网络设置。

## 从控制台设置

如果您计划以无头模式运行 Raspberry Pi（即未连接显示器或键盘），默认情况下您不需要访问桌面，也无法使用桌面网络实用程序。

在 Raspbian 的最新发行版中，SSH 服务默认启用，您可以 SSH 进入 Raspberry Pi。如果您刚刚加入 Wi-Fi 网络，可以调整其设置。这是在使用以太网设置 Wi-Fi 时的意图。现在您可以通过 SSH 登录，并输入以下命令：

```
sudo nano /etc/network/interfaces
```

这将打开包含的文件编辑器 nano，并向您展示当前的接口配置。我们关注最后几行。如果您看不到某些行，可以手动输入：

```
auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
auto wlan0

iface wlan0 inet dhcp
    wpa-ssid "ssid"
    wpa-psk "yourpassword"
```

要保存文件，请按 CTRL + X，然后按 Y，最后按 Enter 完成。

allow-hotplug 命令将按其名称所述执行：它允许您插拔分配给 wlan0 接口的无线适配器。Auto wlan0 告诉 Raspbian 根据您提供的设置自动配置接口。

我们需要用路由器详细信息替换 SSID 和密码，保持文本在引号内。这是可以使用最基本的配置。

不过此配置有一个问题：如果无线与路由器断开连接，接口不会自动重新启动。有许多脚本试图解决这个问题。配置要求您了解有关无线配置的额外详细信息。下一节提供了一种完全自动化的方式来执行此操作。我们将使用 wpa-supplicant，它现在默认随 Raspbian 安装。在 /etc/network/interfaces 中，我们将最后几行更改为以下内容：

```
allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp
```

我们需要进入 /etc/wpa_supplicant 目录，创建文件并编辑 wpa_supplicant.conf：

```
sudo touch wpa_supplicant.conf
nano wpa_supplicant.conf
```

## 下载示例代码

您可以从 http://www.packtpub.com 的账户下载本书的示例代码文件。如果您在其他地方购买了本书，可以访问 http://www.packtpub.com/support 并注册以直接将文件通过电子邮件发送给您。

您可以通过以下步骤下载代码文件：

- 使用您的电子邮件地址和密码登录或注册我们的网站。
- 将鼠标指针悬停在顶部 **SUPPORT** 选项卡上。
- 单击 **Code Downloads & Errata**。
- 在 **Search** 框中输入书名。
- 选择您要下载代码文件的书籍。
- 从下拉菜单中选择您购买本书的来源。
- 单击 **Code Download**。

![](img/a8753cacc9d454dfc09eda1ed5012bd3_42_0.png)

您也可以通过单击 Packt Publishing 网站上书籍网页上的 **Code Files** 按钮来下载代码文件。可以通过在 **Search** 框中输入书名来访问此页面。请注意，您需要登录到您的 Packt 账户。

下载文件后，请确保使用最新版本解压缩或提取文件夹：

- Windows 的 WinRAR / 7-Zip
- Mac 的 Zipeg / iZip / UnRarX
- Linux 的 7-Zip / PeaZip

在 nano 中，一个简单的文本编辑器，我们需要输入以下内容：

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="ssid"
    proto=WPA RSN
    key_mgmt=WPA-PSK
    pairwise=CCMP TKIP
    group=CCMP TKIP
    psk="password"
}
```

配置文件包含有关连接的一些额外详细信息。supplicant 的工作方式是它将尝试使用为 proto、pairwise 和 group 定义的参数进行连接。您可能需要调整 key_mgmt，但 WPA-PSK 是现代无线路由器最常见的配置。

此方法还允许您在路由器不广播其 SSID 时连接到路由器。

> 普遍的误解是禁用 SSID 广播可以保护您的网络。即使您关闭了此功能，使用普通无线网卡仍有工具可以毫不费力地找到 SSID 名称。最佳的安全措施是使用最新的加密协议和复杂的密码。

## 使用 wicd-curses

使用 wicd-curses 包是在控制台中设置无线甚至有线接口的更简单方法。该包允许您编辑高级选项，例如静态 IP、DNS 和主机名。后台还运行一个守护进程，如果无线信号中断会自动重新连接接口，这是在信号较差区域维护无线网络的更可靠方式。要使用此包，您应该在 wpa_supplicant 接口文件中注释掉任何设置：

> 安装此包将安装许多其他依赖包，并可能占用超过 8 MB 的额外空间。一些来源还声称运行此守护进程会使用更多开销，但这听起来是为了易用性和可靠性而付出的公平价格，尤其是在 Raspberry Pi 2 上。

```
sudo apt-get install wicd-curses
sudo wicd-curses
```

您将获得一个控制台屏幕，其中列出可用的无线接入点。使用光标键选择您的接入点，然后按右箭头键编辑其属性。

> 如果您看到未检测到无线网络的消息，请尝试按 P 打开首选项窗口，并在无线接口字段中输入 wlan0。

一旦进入无线网络配置首选项屏幕，您只需在密钥字段中输入密码即可。按 F10 保存设置。
