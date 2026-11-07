
![](img/095df2cd1930baecbcecaa51ee3417aa_170_0.png)

wifite 开始查找无线网络后，它会开始显示它们，并附带各种信息，例如加密类型、ESSID、信号强度和 WPS。大量可用于审计的宝贵信息。Wifite 将继续停留在下面的屏幕上，直到你通知它列表看起来不错并且你已准备好进行攻击/审计。要实现这一点，你需要按 Ctrl + C：

![](img/095df2cd1930baecbcecaa51ee3417aa_170_1.png)

按 Ctrl + C 后，列表将不再更新，并且会提示你想要攻击的目标。一旦你知道要攻击和审计的网络，就输入分配给它的编号，然后按 Enter：

| 编号 | ESSID | 信道 | 加密 | 信号强度 | WPS? | 客户端 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | \x00 | 1 | WEP | 99db | no | client |
| 2 | (48:F8:B3:4B:D2:6B) | 1 | WPA | 99db | no |  |
| 3 | blizzard | 11 | WPA2 | 87db | no |  |
| 4 | \ | 11 | WPA2 | 86db | no |  |
| 5 | \ | 11 | WPA2 | 84db | no |  |
| 6 | SP | 1 | WPA2 | 54db | no | clients |
| 7 | hechler | 1 | WPA2 | 47db | wps | clients |
| 8 | Cisco48030 | 6 | WPA2 | 46db | wps | clients |
| 9 | zoom | 1 | WPA2 | 40db | wps |  |
| 10 | Xbox One | 3 | WPA2 | 38db | no | clients |
| 11 | DIRECT-ae-HP M426... | 1 | WPA2 | 29db | wps |  |

是否启用 WPS 将决定攻击的顺序。如果启用了 WPS，它将尝试破解它并获取 PIN。如果未启用 WPS，它将尝试破解 WEP 密钥（如果启用了 WEP）。以下是针对启用了 WPS 的网络进行的攻击示例：

```
[+] 1 target selected.

[0:00:00] initializing WPS Pixie attack on ________________
[0:47:39] WPS Pixie attack: Starting Cracking Session. Pin count: 0, Max pi...
(^C) WPS Pixie attack interrupted
[0:00:00] initializing WPS PIN attack on ________________
[0:01:24] WPS attack, 0/0 success/ttl,
(^C) WPS brute-force attack interrupted
[0:08:20] starting wpa handshake capture on ______
[0:08:19] new client found: 1C:9E:46:AD:94:BE
[0:08:19] new client found: F8:CF:C5:7C:C8:C0
[0:08:09] new client found: 44:1C:A8:05:2A:AE
[0:08:09] new client found: 00:BB:3A:A6:D2:62
[0:08:09] new client found: A4:F1:E8:4A:32:78
[0:07:43] sending 5 deauth to F8:CF:C5:7C:C8:C0... sent
```

对于没有 WPS 且仅使用 WEP 的网络，以下是我的实验室的截图：

![](img/095df2cd1930baecbcecaa51ee3417aa_172_0.png)

我认为 wifite 是一个很棒的工具，可以确保无线网络按照策略设置，并且密钥和 PIN 无法被破解。如果任何测试成功，wifite 将打印出相关信息。

## Bluetooth probing

随着越来越多的支持蓝牙的设备进入企业，蓝牙扫描变得越来越重要。不仅因为支持蓝牙的设备数量众多，而且黑客会在网络或企业内部放置自己的设备，利用蓝牙窃取数据。蓝牙便宜且易于使用。这正是黑客的完美驱动因素。野外已发现蓝牙窃取设备，因此像 Visa 这样的信用卡公司建议定期进行蓝牙扫描，类似于过去的无线测试。

由于了解网络空间中有哪些支持蓝牙的设备非常重要，我将进行一些蓝牙发现、测试和攻击，以了解可以利用环境中的哪些信息。我将使用各种不同的蓝牙实用工具来尽可能多地获取有关环境的信息。

## Bluelog

Bluelog 是一个蓝牙扫描器，可在区域内查找蓝牙设备的迹象。它是了解周围环境的好工具。了解周围有什么有助于识别可能隐藏的恶意蓝牙设备。

Bluelog 被定义为蓝牙站点勘测工具，因此让它在固定位置长时间运行将使你能够获得该区域的良好概况。这正是我将如何运行它的方法。命令本身并不过于复杂，但有一些可用选项：

### Basic options:
### 基本选项：
- `-i <interface> 设置扫描设备，默认是 "hci0"`
- `-o <filename> 设置输出文件名，默认是 "devices.log"`
- `-v 详细模式，将发现的设备打印到终端`
- `-q 安静模式，关闭非必要的终端输出`
- `-d 启用守护进程模式，Bluelog 将在后台运行`
- `-k 终止已运行的 Bluelog 进程`
- `-l 启动 "Bluelog Live"，默认禁用`

### Logging options:
### 日志选项：
- `-n 将设备名称写入日志，默认禁用`
- `-m 将设备制造商写入日志，默认禁用`
- `-c 将设备类别写入日志，默认禁用`
- `-f 使用“友好”设备类别，默认禁用`
- `-t 将时间戳写入日志，默认禁用`
- `-x 混淆发现的 MAC 地址，默认禁用`
- `-e 使用 CRC32 编码发现的 MAC 地址，默认禁用`
- `-b 启用 BlueProPro 日志格式，见 README`

### Advanced options:
### 高级选项：
- `-r <retries> 名称解析重试次数，默认是 3`
- `-a <minutes> 健忘模式，Bluelog 将在 X 时间后忘记设备`
- `-w <seconds> 扫描窗口（秒），见 README`
- `-s 仅 Syslog 模式，无日志文件，默认禁用`

在运行 bluelog 之前，你需要确保系统能够看到蓝牙接口并且它正常工作。你可以使用 hciconfig 来实现。以下是我的系统的输出：

```
root@pi-kali:~# hciconfig
hci0:      Type: Primary  Bus: USB       BD Address: 00:1A:7D:DA:71:10  ACL MTU: 310:10  SCO MTU: 64:8   UP RUNNING      RX bytes:1232  acl:0  sco:0
events:74  errors:0     TX bytes:2858  acl:0  sco:0  commands:74  errors:0
```

你可以看到我有我的 hciX 接口（就我而言是 hci0）。你也可以看到它有一个 MAC 地址。现在我们已经验证蓝牙接口正在工作，我们可以开始扫描我们的区域。

以下是我在实验室中运行的命令和输出。由于我的设备上有多个蓝牙接口，我将指定要使用的设备。我还将指定写入结果的文件以及日志选项，以写入时间戳、设备名称、制造商和类别。

```
bluelog -i hci0 -o /root/bt-log.txt -vnmct
```

以下是我的实验室的输出：

```
root@pi-kali:~# bluelog -i hci0 -o /root/bt-log.txt -vnmct
Bluelog (v1.1.2) by MS3FGX-----------------------------------Initializing
device...OK
Opening output file: /root/bt-log.txt...OK
Writing PID file:
/tmp/bluelog.pid...OK
Scan started at [02/22/17 01:53:30] on 00:1A:7D:DA:71:10.
```

按 Ctrl + C 结束扫描：

```
[02/22/17 01:56:02] 58:40:4E:50:D1:0E, JB's iPhone, 0x7a020c
[02/22/17 01:56:17] 8C:DE:52:1F:F5:07, SRS-BTM8, 0x240414
[02/22/17 01:58:05] C0:CE:CD:0F:D4:BA, Ryan's iPhone, 0x7a020c
```

根据我的发现，你可以看到我的区域是一个蓝牙密集区域。重要的是我需要检查这个列表，如果有任何奇怪之处，就进一步调查。但是，作为渗透测试人员，你的工作是运行、观察、记录并报告。

我没有看到这被定义为策略或要求，但我绝对建议定期执行这些检查。类似的 WiFi 检查由 PCI 等组织要求。加入这些检查也不会有坏处，有助于增强安全态势。

## Btscanner

Btscanner 是另一个很棒的蓝牙扫描工具，但与 bluelog 相比它具有更多功能。我喜欢使用 btscanner，因为你可以查看找到的设备并查看这些设备的许多信息。

从命令行运行 btscanner 非常简单。在 CLI 上，输入以下命令：

```
btscanner
```

运行此命令将打开一个简单的 GUI。GUI 底部有一个菜单，显示你可以执行的操作。接下来你想要做的就是输入 I 进行 inquiry 扫描。这将启动环境扫描。随着设备的发现，它们将开始在窗口上显示。以下是我的实验室中找到的内容的截图：

![](img/095df2cd1930baecbcecaa51ee3417aa_175_0.png)

btscanner 的一个非常方便的功能是，你可以在设备上使用上下箭头，然后按回车。你将被带到该特定设备的详细信息和描述。这些信息在诊断设备是否合法时非常有用。

