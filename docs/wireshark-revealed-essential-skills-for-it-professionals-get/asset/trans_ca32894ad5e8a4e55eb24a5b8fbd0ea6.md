

![](img/da40f92928403866599198344271d63e_1381_0.png)

图 9.22：“跟踪 TCP 流”对话框中的查找工具

## USBPcap

USBPcap 在 Linux 和 Mac 用户中已经存在很长时间了，但对于 Windows 用户来说，这是他们第一次能够嗅探 USB 接口上的活动。那么，让我们快速了解这一最新功能，并尝试通过一个示例来理解如何使用它。按照以下步骤重现该场景：

- 1. 在 Windows 机器上成功安装 Wireshark 后，强烈建议你重启机器，因为 USBPcap 可能会带来一些麻烦。
- 2. 电脑重启后，打开命令提示符，将当前目录切换到 USBPcap 安装目录，该目录通常位于 C:\Program Files\USBPcap\。
- 3. 现在，使用 dir 命令列出目录内容，检查目录中是否存在 USBPcapCMD.exe。请参考下面的截图，该截图展示了这一步：
- 4. 在命令提示符中输入 USBPcapCMD.exe 以启动嗅探应用程序。

```
C:\Program Files\USBPcap\
```

```
dir
```

```
USBPcapCMD.exe
```

![](img/da40f92928403866599198344271d63e_1382_0.png)

图 9.23：USBPcap 安装目录

```
USBPcapCMD.exe
```

- 5. 成功启动后，系统会要求你选择要在哪个根集线器上嗅探流量，以及要将输出重定向到的跟踪文件的名称。请参考下面的截图：

![](img/da40f92928403866599198344271d63e_1383_0.png)

- 6. 按照提示，应用程序将开始在根集线器 1 上执行嗅探过程，并将捕获到的 USB 接口上的所有活动转储到 `abc.pcap` 文件中。
- 7. 现在，尝试将一些内容从电脑复制到 USB 驱动器，或者反过来。你很可能无法在命令提示符中看到任何实时活动，但实际上它在后台运行。
- 8. 无论何时想停止嗅探过程，都可以按 Ctrl + C。
- 9. 现在，是时候用 Wireshark 打开 `abc.pcap` 文件，看看跟踪文件中有什么了。请参考下面的截图：

![](img/da40f92928403866599198344271d63e_1384_0.png)

图 9.24：`abc.pcap` 跟踪文件

如你所见，我们捕获到了一个活动；它看起来与我们在网络数据包中看到的很相似。我们拥有所有熟悉的列，列出了各种详细信息，如时间、源、目标等。因此，我们成功地将可用 USB 接口上的活动转储出来，没有遇到任何技术麻烦。希望你能进一步研究，以更好地理解 USBPcap。

## 总结

新版 Wireshark 采用了新框架，为我们带来了全新且非常惊艳的图形用户界面。旧版基于 GTK 框架构建，而现在我们使用了 QT 框架，从普通用户的角度来看，差异主要体现在其外观和体验上。

滚动功能无疑是我们在所有主流应用程序中都见过的功能之一，但我们要向开发者致敬，他们提出了这样一个创意：当你试图查找特定内容时，在滚动条内显示跟踪文件的着色模式。这确实带来额外优势。

翻译功能使 Wireshark 更加国际化，并在个性化方面更贴近每位用户。由于许多 Wireshark 用户可能不习惯英语，现在他们可以将语言更改为自己的母语，这将使专业人员的分析过程更加高效。

图表是能够让人分辨正常与异常情况差异的功能之一，并且非常常用。现在，创建和自定义图表比以往任何时候都更容易，外观和体验也得到了极大改善。

“跟踪 TCP 流”对话框引入了一些新功能，让你可以查找 ASCII 字符串，并且还能轻松地在可用流之间切换；你无需关闭并重新打开对话框即可切换到不同的流。

USBPcap 陪伴我们已有相当长的时间，大多数 Linux 和 Mac 用户可能都知道这一点。你的网卡让你能够监听有线/无线信道的方式，类似于 USBPcap 选项让你能够监听你所拥有的 USB 端口的方式。这意味着现在 Wireshark 也可以跟踪 USB 接口上发生的活动。

## 练习题

- Q.1 试着找出 GTK 和 QT 框架之间的主要区别。你认为哪一个更好？
- Q.2 尝试使用翻译功能，将 Wireshark 中的系统默认语言更改为你选择的任何其他语言。
- Q.3 使用新版本和旧版本创建流图，观察你能找出两个图表之间的多少差异。
- Q.4 打开你之前拥有的任意捕获文件，并尝试确定其中有多少条 TCP 流。
- Q.5 找出一种方法来移除我们在本章前面创建的 ARP 协议的显示过滤器按钮。
- Q.6 尝试更改 ARP 数据包的着色规则，并检查你是否能在智能滚动条区域观察到差异。
- Q.7 在 Windows 机器上安装新版 Wireshark 后，尝试启动 USBPcap。然后，将内容从电脑复制并粘贴到 USB 设备，或反过来（将所有活动转储到 test.pcap 文件中）。
- Q.8 在 Wireshark 中打开最近捕获的 USB 接口活动跟踪文件 test.pcap，并尝试弄清楚列表窗格中列出的数据包说明了什么。具体来说，尝试分析源和目标列中显示的值。

## 参考书目

本课程融合了不同的项目和文本，并考虑到你的学习历程而打包而成。它包含以下 Packt 产品的内容：

- Wireshark Essentials - James H Baxter
- Network Analysis using Wireshark Cookbook - Yoram Orzach
- Mastering Wireshark - Charit Mishra

## 索引

### A

- TCP 异常
  - 示例 / [异常流量](Unusual traffic)
- access_denied / [如何操作……](How to do it...)
- ACK / [工作原理……](How it works...), [TCP 序列/确认机制的正常操作](Regular operation of the TCP Sequence/Acknowledge mechanism)
- 确认号字段 / [工作原理……](How it works...)
- ACK 数据包 / [WEP 开放密钥](WEP-open key)
- ACK 扫描 / [如何操作……](How to do it...)
- 主动模式（ACTV） / [分析 FTP 问题](Analyzing FTP problems)
- 地址解析协议（ARP） / [以太网帧与交换机](Ethernet frames and switches)
  - 关于 / [地址解析协议](Address Resolution Protocol), [TCP/IP 模型中的层](The layers in the TCP/IP model)
  - 欺骗 / [ARP 欺骗](ARP poisoning), [ARP 欺骗](ARP poisoning)
- 地址解析协议（ARP）过滤器 / [配置以太网、ARP、主机和网络过滤器](Configuring Ethernet, ARP, host, and network filters)
- Wireshark 的优势
  - 用户友好 / [为什么使用 Wireshark？](Why use Wireshark?)
  - 健壮性 / [为什么使用 Wireshark？](Why use Wireshark?)
  - 跨平台 / [为什么使用 Wireshark？](Why use Wireshark?)
  - 过滤器 / [为什么使用 Wireshark？](Why use Wireshark?)
  - 成本 / [为什么使用 Wireshark？](Why use Wireshark?)
  - 支持 / [为什么使用 Wireshark？](Why use Wireshark?)
- AirPcap 适配器
  - 关于 / [AirPcap 适配器](AirPcap adapters)
- 允许子解析器选项 / [更多内容……](There's more...)
- 任播地址
  - 关于 / [IPv6 地址类型](IPv6 address types)
- 基于应用程序的问题
  - 故障排除 / [对基于应用程序的问题进行故障排除](Troubleshooting application-based issues)
- 应用层攻击
  - 关于 / [工作原理……](How it works...)
- 应用程序攻击
  - 发现 / [发现暴力破解和应用程序攻击](Discovering brute-force and application attacks), [如何操作……](How to do it...), [更多内容……](There's more...)
- 应用层，OSI

