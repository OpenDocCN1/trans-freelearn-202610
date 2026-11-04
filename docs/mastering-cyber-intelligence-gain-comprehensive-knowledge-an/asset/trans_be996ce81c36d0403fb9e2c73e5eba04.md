
| 阶段 | 发现 |
|---|---|
| 侦察 | 网站页面：event-mogul/about 页面。LinkedIn、Twitter。电子邮件地址及研究 affiliations |
| 武器化 | research_tech.pdf (嵌入式 JavaScript) |
| 投递 | 钓鱼 (email)。194.150.215.153 |
| 利用 | 挂钩运行进程 (explorer.exe) |
| 安装 | read_sl.exe (在 explorer.exe 上), executable.1640.exe, ~/.exe (outlook 临时文件夹) |
| 命令与控制 (C2) | 125.19.103.198 Sys/files/ |
| 行动 | 46.168.5.140, 数据外泄 - FTP, HTTP POST/GET (RAR docs project cobra), C0d0s123 (密码) |

图 15.1 – 入侵分析的简化版网络杀伤链模型

分享威胁情报使网络威胁的防御更有效。在成功分析后，CTI 分析师应选择正确的格式和平台在内部和外部传播结果。本节介绍 IOC 共享以及使用 YARA 检测恶意流量。

## 用例一 – 使用 YARA 开发 IOC

YARA 常用于恶意软件检测和流量监控。其结构已在第 14 章，威胁情报报告与传播中介绍。CTI 分析师应熟悉基本的 YARA 规则及其应用。本用例帮助 CTI 分析师熟悉使用 YARA。

**用例场景：** 你为 ABC 公司进行了入侵分析并识别出与攻击相关的 IOC。战术团队要求你创建一个 YARA 规则以在未来检测类似攻击和恶意软件。

**用例目标：** 本用例的目标是练习针对 IOC 生成和验证 YARA 规则以保护系统。完成此练习后，你应能够编写可在特定条件下检测网络事件的规则。

解决本用例的步骤如下：

- 1. **确保 YARA 正常工作：** 登录 SIFT VM（或 Ubuntu 机器）并测试 YARA。测试 YARA 的基本方法是创建一个虚拟规则。打开终端并执行 echo "rule <rule name> { condition: true}" > <yaratest_rule>，然后执行 yara <rule name> <rule name> 命令。输出显示在下图中：

![](img/d47f26598025b2da017dd41fcbb6e602_466_0.png)

图 15.2 – 测试 Yara 规则 – 示意图

输出显示 yaratest 规则与 yaratest_rule 文件匹配。如果未安装该工具，请在终端执行 sudo apt install yara 命令。使用 yara --help 命令查看可为 YARA 规则配置的语法和选项。一般语法如下所示：

```
yara [Option]... [Namespace:]rules_file... File | Dir | PID
```

- 2. **创建规则以检测 IOC：** 我们创建规则以检测以下 IOC：46.168.5.140 和 125.19.103.198 IP 地址以及 12cf6583f5a9171a1d621ae02b4eb626 MD5 哈希。使用文本编辑器创建规则并将其保存为 .yara 文件，如下图所示：

![](img/d47f26598025b2da017dd41fcbb6e602_466_1.png)

图 15.3 – 入侵检测的 Yara 规则代码

该规则定义了三个变量、两个 IP 地址和 ddraw.dll 字符串。条件定义为如果任何字符串与流量文件、内存转储、文件或进程匹配，则应生成警报。

- 3. **针对内存转储验证规则：** 我们使用在 *第 10 章，威胁建模与分析 – 实际用例* 中执行的入侵分析的内存转储。执行以下命令：

```
yara -s analysis.yara memorydump.vmem
```

命令和输出显示在下图中：

![](img/d47f26598025b2da017dd41fcbb6e602_467_0.png)

图 15.4 – YARA 规则针对内存转储验证的说明

-s 选项用于打印匹配的字符串。可以在搜索时添加 nocase 参数以移除大小写敏感性。字符串搜索的另一个重要参数是 wide 属性。它允许在字符串以每个字符两个字节编码时仍进行搜索。

输出显示 YARA 规则中的指标与内存转储中的指标匹配。它还提供了扫描文件中内存地址。我们可以看到 125.19.103.198 已位于两个不同的内存地址 0xc00887f 和 0xc08a778 中，并且 ddraw.dll 字符串可在六个不同的内存地址上找到。现在，我们知道该规则将在内存中检测到这些 IOC。

- 4. **针对文件验证规则：** 我们测试规则根据哈希值检测恶意文件。我们使用来自 *第 10 章，威胁建模与分析 – 实际用例* 的 executable.1640.exe 文件。执行以下命令：

```
yara -f analysis.yara forensic_files/
```

输出显示在下图中。

```
jovan@siftworkstation: ~/Downloads
$ yara -f analysis.yara forensics_files/
intrusion_detection forensics_files//executable.1640.exe
intrusion_detection forensics_files//1640.dmp
intrusion_detection forensics_files//memorydump.vmem
```

图 15.5 – YARA 规则针对目录验证的说明

-f 选项用于快速扫描并匹配字符串。如果当前目录是要扫描的目录，请使用 yara -f . 命令。我们可以看到规则已检测到三个包含与条件匹配的部分的文件。请注意，可执行文件位于内存转储和 1640.dmp 文件中。现在，我们可以确定我们的 YARA 规则将检测到恶意文件。

> **重要说明 – 规避技术**
> 诸如 **高级持续性威胁 (APTs)** 之类的威胁采用复杂的规避技术。成功攻击和数据泄露的增长证明，包括防病毒、入侵检测和防火墙在内的安全系统可能无法检测到恶意软件和 IOC。因此，建议更新 YARA 规则数据库，频繁扫描系统以查找 IOC，并确保 *CTI 程序有效。*

作为 CTI 分析师，你已学习如何使用 YARA 创建 IOC 并对其进行验证。建议在投入生产前先验证规则。对于更复杂的 YARA 规则、使用库和外部文件以及使用 Python 进行自动化，请参阅官方文档页面：https://bit.ly/3tZxggj。

## 用例二 – 使用 Anomali STAXX 共享情报

Anomali STAXX 基于 STIX 和 TAXII 共享标准。它已在上一章中介绍，其中包含作为独立基础设施安装平台的基本步骤。我们将在本用例中研究 CTI 分析师如何利用 STAXX 执行情报共享。

**用例场景：** ABC 公司希望你为与严重依赖 STIX/ TAXII 标准的其他组织建立威胁情报协作而设置 STAXX 基础设施。此外，CTI 经理希望你使用已安装的基础设施发布之前的入侵分析的输出。

**用例目标：** 本用例的目标是熟悉 STAXX 平台并使用广泛使用的 STIX/ TAXII 标准发布和共享威胁情报。Anomali STAXX 促进威胁情报的共享和收集。

用于解决本用例的步骤如下：

- 1. **下载 STAXX OVA 文件：** 从网站 (https://bit.ly/3nZcBoO) 下载 STAXX **开放虚拟设备** (**OVA**) 文件并将其保存到本地计算机。
- 2. **导入并配置 STAXX VM：** 将 OVA 文件导入虚拟化环境。本例中我们将使用 VirtualBox。成功导入设备后，启动 VM。控制台用户名和密码分别为 anomaly 和 anomalistaxx。你将在首次登录时被要求更改密码。控制台窗口显示在下图中：

![](img/d47f26598025b2da017dd41fcbb6e602_469_0.png)

图 15.6 – 成功安装后 Anomali STAXX 控制台窗口

请注意访问链接：https://10.0.2.15:8080。10.0.2.15 是 VM **网络地址转换 (NAT)** 适配器 IP 地址。它不能通过主机直接访问。你应通过导航到 **VM | 网络 | 适配器 1 | 高级 | 端口转发** 来启用 NAT 端口转发。添加规则，如下图所示，然后单击 **确定** 完成配置：

![](img/d47f26598025b2da017dd41fcbb6e602_470_0.png)

图 15.7 – STAXX VM 的端口转发配置

或者，你可以使用 *桥接适配器* 设置代替 NAT 以允许主机和 VM 之间直接通信。

- 3. **访问 Web 界面：** 打开 Web 浏览器并导航到 https://127.0.0.1:8383，接受证书消息，你应被重定向到如下图所示的 Anomali STAXX 平台登录页面。用户名和密码分别为 admin 和 changeme，你将在首次登录时被要求更改密码：
