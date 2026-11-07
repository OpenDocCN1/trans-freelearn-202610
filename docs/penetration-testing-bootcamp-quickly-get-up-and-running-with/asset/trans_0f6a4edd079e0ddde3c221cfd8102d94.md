
使用名为 macof 的工具，我们可以尝试通过向我们连接到的交换机端口泛洪尽可能多的 MAC 地址，来检查适当的交换机端口控制是否已就位。

macof 的语法如下。查看 man 手册页或 --help 以获取可用选项的完整说明：

```
macof [-i interface] [-s src] [-d dst] [-e tha] [-x sport] [-y dport] [-n times]
```

在我的实验室中，为了测试这一点，我将运行以下命令来检查是否可以使用完所有的 TCAM 空间：

```
macof -i eth0 -d 192.168.33.1 -n 1000000
```

为了防止 MAC 地址泛洪，关键在于尽可能限制交换机端口允许的 MAC 地址数量。在 Cisco 设备上，这是通过 switchport port-security 系列命令完成的。以下是一个 Cisco 交换机的基本示例：

```
Switch(config)# interface gi 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 5
Switch(config-if)# switchport port-security mac-address sticky
```

## 基于无线网络的攻击

随着无线网络的普及，这些网络的安全性提升从未如此重要。与有线网络一样，无线网络也有其公平份额的用于审计和攻击各种主机和网络的工具。作为渗透测试人员，我们的职责是确保无线网络设置正确，并且范围内没有其他不应该存在的网络，同时确保它们不会干扰范围内的网络。

在以下部分，我将展示我在渗透测试期间使用的一些无线工具的示例。市面上有许多优秀的工具，我甚至可以专门写一本书来介绍它们，但我希望选择一个样本来展示各种测试策略。

### 使用 aircrack-ng 破解 WPA2

在我看来，aircrack-ng 是目前最优秀的无线渗透工具之一。它可以通过在无线接口上设置监听接口、嗅探流量和握手，然后尝试并破解这些密钥来破解 WPA 和 WEP 密钥。aircrack-ng 包含许多工具，包括 airbase-ng、airdecap-ng、airdecloak-ng 和 aireplay-ng。我将在我的无线测试中使用其中一些工具。

我将在实验室中使用 aircrack-ng 尝试破解一个使用 WPA 的无线访客网络，以确保他们没有使用容易猜测的密码短语。涉及几个步骤；让我现在带你了解一下。我假设 aircrack-ng 已经安装：

- 1. 我们需要启动无线监控接口。为此，我们需要使用 airmon-ng 命令，后跟计划使用的无线接口。在我的例子中，它是 wlan1。
- 2. 接下来，我们需要获取正在看到的可用 WLAN 列表。这样，我们就可以选择合适的 WLAN 进行数据包和握手捕获。要获取此列表，我们可以使用 airodump-ng 命令：

```
root@pi-kali:~# airmon-ng start wlan1
Found 2 processes that could cause trouble.
If airodump-ng, aireplay-ng or airtun-ng stops working after a short period of time, you may want to run 'airmon-ng check kill'
PID
Name
199 NetworkManager
445 wpa_supplicant
PHY     Interface       Driver      Chipset
phy0    wlan0           ??????      Broadcom 43430
phy1    wlan1           rt2800usb   Ralink Technology, Corp. RT5372
(mac80211 monitor mode vif enabled for [phy1]wlan1 on
[phy1]wlan1mon)

(mac80211 station mode vif disabled for [phy1]wlan1)
```

![](img/095df2cd1930baecbcecaa51ee3417aa_164_0.png)

对于可能造成干扰的任何进程，在继续之前最好先停止它们，以确保没有干扰。你可以使用 kill PID 命令，其中 PID 是该干扰进程的进程 ID。

```
root@pi-kali:~# airodump-ng wlan1mon
```

当我在实验室运行上述命令时，你可以通过以下截图快速看到我可用的所有 SSID：

![](img/095df2cd1930baecbcecaa51ee3417aa_165_0.png)

- 3. 一旦我找到想要尝试破解的 WLAN，我可以按 Ctrl + C 退出 airodump 命令。现在，我可以使用 airodump-ng 命令再次设置数据包和握手的捕获。不过这次，我们将使用一些其他命令行开关：

```
root@pi-kali:~# airodump-ng -c 1 -w SP-Guest-01 --bssid 8E:15:44:AA:4B:18 wlan1mon
```

在前面的代码中，`-c` 指定频道，`-w` 是 WLAN 名称，`-bssid` 是我们将要捕获流量的 WLAN 的 BSSID。最后，我们以我们设置的监控接口结束命令。在运行期间，它会让你知道哪些客户端已连接以及它捕获的流量。如果你幸运的话，也可能捕获到握手。

![](img/095df2cd1930baecbcecaa51ee3417aa_166_0.png)

如果你没有捕获任何握手，不用担心。你可以使用 `aireplay-ng` 命令 (`aireplay-ng -0 0 -a <BSSID>`) 监控接口强制客户端取消身份验证然后重新身份验证，这意味着你将看到握手。

这是我使用 airodump-ng 在实验室捕获的截图。请注意，它确实显示我获得了握手。我还可以看到我有两台客户端已连接：

![](img/095df2cd1930baecbcecaa51ee3417aa_166_1.png)

- 4. 一旦我们对无线捕获感到满意，我们可以按 Ctrl + C 停止它。现在，是时候使用 `aircrack-ng` 命令将该捕获与一个好的词表结合起来尝试破解该密码短语了。为了在我的实验室中实现这一点，我运行了以下命令：

```
root@pi-kali:~# aircrack-ng -w /usr/share/wordlists/wordlist.txt SP-Guest-01.cap
```

一旦该命令运行，它将使用指定的词表扫描捕获的文件。以下截图是在捕获文件正在通过词表尝试查找密码短语时拍摄的：

![](img/095df2cd1930baecbcecaa51ee3417aa_167_0.png)

如果一切成功，完成后你将看到密钥被打印出来。这只是展示了 aircrack-ng 的强大功能和多功能性。

### 使用 Kismet 监控空中

Kismet 是一个无线网络扫描实用程序。它在告知你无线网络的覆盖范围以及涉及的频道和数据包等项目方面做得非常好。然后你可以切换到各种窗口以查看更具体的资讯视图以及任何警报。

以下截图向我们展示了范围内的无线网络以及 Kismet 通过扫描获得的信息：

![](img/095df2cd1930baecbcecaa51ee3417aa_168_0.png)

通过选择频道视图，你可以看到当前空中的频道利用情况：

![](img/095df2cd1930baecbcecaa51ee3417aa_168_1.png)

你还可以查看空中发生的各种无线警报：

![](img/095df2cd1930baecbcecaa51ee3417aa_168_2.png)

通过突出显示警报，你可以看到该特定警报的更详细视图。如果某些内容不明确，这是一种查看更多细节的好方法：

```
Time: Feb 25 02:51:12
Alert: CHANCHANGE
BSSID: 38:D5:47:89:46:F1
Source: 38:D5:47:89:46:F1
Dest: FF:FF:FF:FF:FF:FF
Channel: 8
Text: Network BSSID 38:D5:47:89:46:F1 changed channel from 9 to 8
```

如你所见，Kismet 在它所做的事情上相当基础，但它在获取你需要了解空中发生情况的信息方面做得很好。

### 使用 wifite 攻击 WEP

Wifite 是典型的 WEP 和 WPS 攻击工具。它也是一个很棒的无线审计工具，提供有关范围内 SSID 及其加密类型等很棒的信息。但除了提供它看到的无线信息外，你还可以使用各种攻击场景尝试攻击无线网络。如果无线网络使用 WPS（无线保护设置），它将尝试使用各种攻击入侵它。虽然 WPS 让无线经验不足的家庭用户更容易设置和连接无线网络，但围绕 WPS 存在许多安全问题。基本上，在攻击几个小时后，PIN 就可以相当容易地被攻破，而且不幸的是，许多这些家庭设备默认启用此功能。Wifite 理解这一点，如果网络启用了 WPS，它将首先尝试使用各种攻击暴力破解。

现在，如果无线网络是 WEP，wifite 也会识别出来并尝试使用各种测试破解密钥，我将展示这些测试。

当你第一次运行 wifite 命令时，你将看到一个屏幕，显示它正在扫描各种网络。以下截图是我实验室中 wifite 启动时的截图：
