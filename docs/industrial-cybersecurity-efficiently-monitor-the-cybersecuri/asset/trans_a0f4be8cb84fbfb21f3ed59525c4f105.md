

这次，Nikto 返回了更多信息。运行在此系统上的 Web 服务器被识别为 IIS 10.0，并且 ASP 4.0.30319 看起来也随之安装作为动态内容提供程序。Nikto 还检测到缺少安全头并标记了可以防止 XSS、点击劫持和 MIME 相关攻击的标志。输出还显示了各种 **开放源代码漏洞数据库 (OSVDB)** 发现。我们可以使用这些标识符通过 **vulners** 网站获取有关发现的漏洞的更多信息。例如，`https://vulners.com/osvdb/OSVDB:5646` 通过允许使用 **DELETE** 方法，让我们详细了解 Web 服务器的风险：

![](img/c8ffa9959721097701584fcc95101362_298_0.png)

图 7.9 – 开放漏洞数据库结果

我们可以通过这种方式了解如何解决发现的漏洞。

为了说明这一点，让我们使用 Nikto 扫描 172.25.100.210，如下所示：

```
pac@KVM0101011:~$ nikto -host 172.25.100.210
- Nikto v2.1.6
-------------------------------------------------------------
--------
+ Target IP:             172.25.100.210
+ Target Hostname:       172.25.100.210
+ Target Port:           80
+ Start Time:            2020-09-02 16:39:41 (GMT-6)
-------------------------------------------------------------
--------
+ Server: Microsoft-IIS/7.5
...
+ Retrieved x-aspnet-version header: 2.0.50727
...
```

在这里，我们可以看到 Nikto 发现运行在此系统上的 Web 服务器是 IIS 7.5，这是随 Windows 7 一起提供的默认 IIS 版本。这反过来支持了我们之前使用 smb-os-discovery Nmap 脚本扫描操作系统详细信息时得出的发现。此外，我们现在知道该主机上运行的 Web 服务使用 ASPNET 版本 2.0.50727，这是在漏洞验证工作中可以使用的信息。

## 使用 Nessus 进行大规模漏洞扫描

为了完成本练习，我想向你展示使用（付费）自动化漏洞扫描器如何能显著加快速度。我将使用 Nessus 的 Essentials 版。Nessus 扫描器可在此下载：https://www.tenable.com/downloads/nessus?loginAttempted=true。只需找到适合你系统的下载版本并从此处请求 Nessus Essentials 激活码：https://www.tenable.com/products/nessus/activation-code。我不会解释如何安装和运行 Nessus 的初始配置，因为网上有大量教程和资源可以帮助完成此操作。我假设你已安装扫描器，已激活，并且已更新。

让我们执行高级网络扫描：

- 1. 导航到 **Scans**，选择 **Create a New Scan**，然后选择 **Advanced Scan**：

图 7.10 – Nessus 扫描 – 新扫描 / 高级扫描窗口

- 2. 为扫描命名并指定我们要扫描的 IP 地址范围：

图 7.11 – Nessus 扫描 – 指定目标

Nessus Essentials 许可证仅允许我们扫描 16 个 IP 地址，因此一旦扫描器在主机发现过程中达到该数量，其余系统将被忽略。不过我们应该没问题，因为我们的 ICS 网络只有 16 个 IP 地址。

- 3. 接下来，切换到设置中的 **Discovery-Host Discovery** 部分并启用 **Scan Operational Technology devices**：

![](img/c8ffa9959721097701584fcc95101362_301_0.png)

图 7.12 – Nessus 扫描 – 扫描 OT 设备

4. 接下来，在 Assessment-Web Applications 下，启用 Scan web applications：

图 7.13 – Nessus 扫描 – Web 应用设置

5. 在 Assessment-Windows 下，启用 Request information about the SMB Domain：

图 7.14 – Nessus 扫描 – 请求 SMB 信息

6. 将其他设置保持为默认值并保存扫描。然后，从 My Scans 页面启动扫描：

## My Scans

![](img/c8ffa9959721097701584fcc95101362_303_0.png)

图 7.15 – Nessus 扫描 – 扫描运行中

Nessus 现在将执行其工作并尝试识别任何漏洞。必须理解的是，Nessus 仅从外部扫描，因此它只会发现通过网络暴露的服务和应用程序暴露的漏洞。你可以配置 Nessus 连接到终端设备（通过添加凭据），以便获得更深入的漏洞覆盖范围。

扫描可能需要一些时间才能完成。一旦完成，结果将可供审阅。从扫描结果的主机页面，我们可以看到所有扫描系统及其漏洞严重性的概览：

![](img/c8ffa9959721097701584fcc95101362_303_1.png)

图 7.16 – Nessus 扫描 – 扫描结果概览

让我们查看这些漏洞，它们显示在 Vulnerabilities 选项卡上：

## 高级 ICS 网络扫描

返回 My Scans

Hosts 16

Vulnerabilities 67

History 3

Filter

Search Vulnerabilities

67 Vulnerabilities

| Sev | Name | Family | Count |
|---|---|---|---|
| MIXED | Microsoft Windows (Multiple Issues) | Windows | 32 |
| CRITICAL | Microsoft Windows XP Unsupported Installation Det... | Windows | 2 |
| CRITICAL | Treck TCP/IP stack multiple vulnerabilities. (Ripple20) | Misc. | 2 |
| MIXED | SNMP (Multiple Issues) | SNMP | 12 |
| MIXED | Web Server (Multiple Issues) | Web Servers | 5 |
| HIGH | Flexera FlexNet Publisher < 11.16.2 Multiple Vulner... | Misc. | 5 |
| MIXED | SSL (Multiple Issues) | General | 38 |
| MIXED | Microsoft Windows (Multiple Issues) | Misc. | 15 |
| MIXED | TLS (Multiple Issues) | Service detection | 11 |
| MIXED | SMB (Multiple Issues) | Windows : User management | 4 |

图 7.17 – Nessus 扫描 – 扫描结果 – 漏洞组

Nessus 在 ICS 网络上运行的系统、服务和应用程序中发现了各种漏洞。如果我们查看第一个漏洞组的详细信息，例如，我们将看到在 Windows 机器中发现了哪些问题：

## 高级 ICS 网络扫描 / Microsoft Windows (Multiple I...

返回 Vulnerabilities

Hosts 16

Vulnerabilities 67

History 3

Search Vulnerabilities

13 Vulnerabilities

| Sev | Name | Family | Count |
|---|---|---|---|
| CRITICAL | Unsupported Windows OS (remote) | Windows | 3 |
| CRITICAL | MS08-067: Microsoft Windows Server Service Crafte... | Windows | 2 |
| CRITICAL | MS09-001: Microsoft Windows SMB Vulnerabilities R... | Windows | 2 |
| CRITICAL | MS05-027: Vulnerability in SMB Could Allow Remote ... | Windows | 1 |
| CRITICAL | MS06-040: Vulnerability in Server Service Could Allo... | Windows | 1 |
| HIGH | Microsoft Windows SMB NULL Session Authentication | Windows | 6 |
| HIGH | MS17-010: Security Update for Microsoft Windows S... | Windows | 3 |
| HIGH | Microsoft Windows SMBv1 Multiple Vulnerabilities | Windows | 1 |
| HIGH | MS06-035: Vulnerability in Server Service Could Allo... | Windows | 1 |
| MEDIUM | Microsoft Windows SMB LsaQueryInformationPolicy ... | Windows | 1 |
| INFO | WMI Not Available | Windows | 9 |

图 7.18 – Nessus 扫描 – 扫描结果 – Windows 机器

查看漏洞范围，从 ms05（约 2005 年）到 ms06（约 2006 年），我们有一些严重过时的系统。这主要是由于我们网络上有两台 Windows XP 机器和一台 Windows 7 机器所致。

请注意，Nessus 发现了一些我们之前使用 Nmap 发现的问题。有趣的是，当我们查看 MS17-010 漏洞的详细信息时，我们会注意到 Nessus 识别出的系统比 Nmap 多一个：

## Output

```
No output recorded.
```

| Port | Hosts |
|---|---|
| 445 / tcp / cifs | 172.25.100.203 172.25.100.212 172.25.100.220 |

图 7.19 – Nessus 扫描 – 扫描结果 – 额外易受 MS17-010 攻击的 Windows 机器

172.25.100.212 没有被 Nmap 检测到。我重新运行了扫描，它仍然显示为不 vulnerable。这只是表明我们不应该依赖单一工具或解决方案来覆盖所有方面。此时，我们可能应该查看 172.25.100.212 是否可被利用（对系统克隆进行渗透测试），以便确定这是否是 Nessus 的误报还是 Nmap 的漏报。我们将在第 17 章《渗透测试 ICS 环境》中详细讨论渗透测试。

正如你所见，使用漏洞扫描器大规模扫描各种问题是一种节省时间的做法。由于其本质，这种技术往往会发现比手动流程更多的问题。代价是它非常具有侵入性。Nmap 会在网络上传输大量流量并以可能导致甚至某些现代自动化设备崩溃的方式 interrogate 系统。建议仅在停机期间、生产停止时或在与生产网络非常相似的测试环境中执行这种级别的自动化漏洞扫描。

## 练习 2 – 手动检查工业计算机

在本第二个练习中，我们将手动检查基于 Microsoft Windows 的系统并学习如何收集系统/操作系统详细信息、已安装软件、启动项和用户等信息。如果在某些时候我们想要进行比较，这些信息是必不可少的。这使我们能够例如在某个时间点比较配置的用户与之前配置的用户，可能允许我们识别已添加到系统中的新管理员帐户。

## 获取 Windows 主机信息

Microsoft Windows 操作系统在各种位置跟踪各种信息。诀窍是找到一种在各种 Windows 版本上都能方便工作的方式来提取这些信息。有（外部）实用程序可以帮助这些工作，但我认为如果不是绝对必要，我们不应该在 ICS 设备上运行外部可执行文件。通过反复试验，我发现了一些内置于几乎所有今天的 Windows 操作系统中的工具，可以帮助我们从 Windows 机器中提取所需的信息。以下各节将介绍这些工具并展示如何使用它们。

