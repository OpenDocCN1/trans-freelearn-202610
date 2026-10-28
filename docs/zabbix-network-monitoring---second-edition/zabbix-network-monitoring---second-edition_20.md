## 故障排除

安装和配置 Zabbix 对某些用户来说可能一帆风顺，而对另一些用户则可能问题不断。问题出现的原因因人而异——可能是库有缺陷、发行版打包不佳、Zabbix 配置不够直观，甚至可能是 Zabbix 自身偶尔存在的 bug。在这里，我们将介绍新用户在执行各种任务时遇到的常见问题：

- 设置初始 Zabbix 安装
- 使用 Web 前端
- 监控不同设备
- 配置阈值和告警

如果你遇到比这些更复杂的问题，我们还将继续提供更详细的调试说明，包括：

- Zabbix 日志文件格式
- 重新加载服务器和代理配置缓存
- 控制正在运行的守护进程
- 观察各个守护进程执行的工作

## 本章简介

编写本章颇费了一番心思。自 2001 年以来我一直在使用 Zabbix，要描述的问题案例实在太多了。我常常在半夜醒来，想到一个值得收录的绝妙问题和解决方案，但到早晨却忘得一干二净。不过这样一来，本章就会永无止境。因此需要有所取舍，我也只好不情愿地接受了。本章并不旨在帮助你解决这辈子（或使用 Zabbix 时）可能遇到的每一个问题，而是尝试帮助解决更常见的问题，并为后续调试提供一些提示和入手点。

## 常见问题

以下是你可能遇到的一些问题：

### 安装

安装过程中有几个常见的障碍，其中一些是由隐藏得很深的因素引起的。

### 编译

- 问：我正尝试在 64 位系统上编译 Zabbix。我已经安装了相应的开发包，但 Zabbix 却说这些包不存在。
- 答：请再次确认安装的是 64 位开发包，而不仅仅是 32 位的。
- 问：我正尝试从 SVN 检出编译 Zabbix，但配置脚本出现以下错误：
  `syntax error near unexpected token `IKSEMEL,iksemel,'`
- 答：安装 `pkg-config` 包，然后重新运行命令以生成配置脚本。
- 问：我正尝试编译 Zabbix，但编译失败了。
- 答：缩小可能的原因范围会很有帮助。请确认你没有使用 `--enable-static` 进行编译，该选项已知会导致编译问题。如果未使用该选项编译仍失败，请检查源目录中 `config.log` 文件的内容，其中通常包含准确的错误详细信息。

## 前端

- **问：** 我已经安装了 Zabbix 前端。默认用户名和密码是什么？
- **答：** 用户名是 Admin，密码是 zabbix。
- **问：** 我正在从 SVN 检出设置 Zabbix。在前端切换语言时，什么反应也没有。
- **答：** 在前端目录的 locale 子目录中有一个 `make_mo.sh` 脚本。它从翻译源 po 文件编译所需的 mo 文件——运行该脚本即可。注意，这需要 Gettext 工具，并且之后可能需要重启 Web 服务器。

## 后端

- **问：** Zabbix 运行正常，但部分/所有图形都不显示。
- **答：** 请参阅 Apache 错误日志以获取更多详细信息。通常，这是由于 PHP 脚本的内存限制过低所致——如果是这种情况，请将 `memory_limit` 参数设置为更大的值并重启 Web 服务器。另一个可能的原因是 `conf/zabbix.conf.php` 文件损坏——请检查该文件是否包含任何奇怪字符，尤其是文件末尾。
- **问：** 复杂视图（如包含许多元素的聚合图形）有时无法加载。可能是什么原因导致的？
- **答：** 与上一个问题类似，请确认 PHP 内存限制未被超过。此外，检查 PHP 脚本超时时间（`max_execution_time` 参数），如有必要则增大该值。
- **问：** 我的图形有断档。
- **答：** 不只是图形，数据库中的数据也缺失了。要解决此问题，需要找出导致数据丢失的原因。常见原因如下：
  - **网络问题：** 如果网络不可靠，数据就会丢失。
  - **被监控设备过载：** 例如，如果你添加了一台多端口交换机，并频繁监控每个端口上的多个监控项，请尝试增大间隔并禁用不需要的监控项。
  - **Zabbix 服务器过载：** 通常问题出在数据库。检查 Zabbix 数据库服务器的系统负载，尤其是 `iowait`。
- **问：** 我已经安装并运行了 Zabbix，但突然又显示安装界面。
- **答：** 检查 `conf/zabbix.conf.php` 文件是否可访问。
- **问：** `conf/zabbix.conf.php` 文件存在，但我仍然看到安装界面。
- **答：** 在某些发行版软件包中，前端可能期望配置文件位于 `/etc/zabbix/web` 或类似位置。请查阅软件包文档。
- **问：** 我试图打开一个包含许多元素的大页面，但页面还没加载完就刷新了。如何解决这个问题？
- **答：** 在你的用户配置文件中增大刷新周期。虽然这不会提高页面加载速度，但至少可以让页面有机会完整加载。
- **问：** 我服务器上的时钟是正确的，但前端显示的时间不正确。
- **答：** 检查 PHP 配置中时区是否设置正确。
- **问：** Zabbix 服务器正在运行，但前端声称它没有运行。
- **答：** 这可能是由多种因素造成的：
  - 检查 `conf/zabbix.conf.php` 文件——前端会使用其中指定的服务器地址和端口来查询 Zabbix 服务器进程。
  - 确保没有防火墙阻止前端到 Zabbix 服务器的连接。
  - 确保 SELinux 没有阻止前端到 Zabbix 服务器的连接。
  - 确保至少启用了一个 trapper 进程——它们负责接受前端连接。也有可能是 trapper 进程数量不足，无法处理所有请求。如果服务器不可用的消息只是偶尔出现，则尤其可能是这种情况。请像我们在第 22 章 Zabbix 维护中所做的那样，监控 trapper 进程的繁忙率。
- **问：** 我遇到了一个这里未列出的前端问题。
- **答：** 检查 Apache 错误日志和 PHP 日志——这些日志通常能让你了解原因。另外，转到**管理 | 用户或用户组**，将你的用户添加到**已启用调试模式**的用户组中。之后，所有前端页面的右下角都会出现一个小的**调试**控件。点击它会显示该特定页面的许多详细信息，包括所执行的确切 API 和 SQL 查询。调试模式可能会占用更多资源——如果启用调试模式后某些前端页面停止工作，请尝试将其禁用。

## 被锁定在前端之外

无论是新用户还是老用户，一个常见的错误就是把自己锁定在前端之外。这可能是由多种方式造成的，但我们更关心的是如何重新进入。

- 问：我忘记了密码，不断尝试登录，直到 Zabbix 前端不再响应。
- 答：默认情况下，连续五次登录失败后 Zabbix 会拒绝访问 30 秒，因此只需等待 30 秒。你可以自定义 includes/defines.inc.php 中的以下值：
  - ZBX_LOGIN_ATTEMPTS：失败尝试多少次后 Zabbix 拒绝访问
  - ZBX_LOGIN_BLOCK：拒绝访问的时长（秒）
- 问：我忘记了 Admin 用户的密码，或者我的任务是管理一个不知道 Admin 用户密码的 Zabbix 安装。
- 答：你可以通过直接修改数据库来轻松重置 Admin 用户密码：
```
mysql> update zabbix.users set passwd=MD5('somepassword') where alias='Admin';
```
> 当然，请将 somepassword 替换为其他字符串。请记住，MySQL 默认会将控制台命令保存在 ~/.mysql_history 文件中，因此你可能希望将密码设置为临时版本，并在之后通过前端更新。
- 问：我更改了身份验证方法，但并没有按计划生效，现在我无法登录了。
- 答：你可以通过编辑数据库来恢复 Zabbix 的内部身份验证方法：
```
mysql> update zabbix.config set authentication_type='0' where configid='1';
```
身份验证类型 0 是内部类型。另外，其他类型包括 1（LDAP）和 2（HTTP）。Zabbix 期望 config 表中只有一条 configid 为 1 的记录。

## 监控

有时监控某项内容进展顺利，有时就是行不通。





## 常规监控

- 问：我添加了一个主机或监控项，但在**监测 | 最新数据**中看不到它。
- 答：检查那里的筛选器是否包含该主机或其主机组。确保**显示无数据监控项**复选框已勾选，且其他筛选选项没有排除你要查找的监控项。
- 问：我可以在**最新数据**中看到我的主机，新值也在不断到来——但它却不在**监测 | 概览**中。
- 答：**概览**可能被设置为显示触发器——请确认该主机是否配置了触发器。没有触发器的主机不会在触发器模式下显示。

## 使用 Zabbix agent 进行监控

- 问：我尝试使用被动 Zabbix agent 检查来监控一台主机，但不起作用。
- 答：Zabbix agent 监控项无法工作的常见原因包括：
  - Zabbix agent 守护进程没有运行。很简单，对吧？不过，还是先检查它是否确实在运行。
  - Zabbix 守护进程没有监听正确的端口或接口。你可以在被监控主机上运行 `netstat -ntpl` 来检查 Zabbix agent 守护进程监听的端口和接口。agent 守护进程的默认端口是 `10050`。
  - agent 守护进程配置文件中的服务器 IP 地址不正确。检查配置文件，确保 server 指令指定的是 Zabbix server 将要连接过来的 IP。
  - 网络问题导致服务器无法正常连接到 agent 守护进程。这包括本地和网络防火墙阻断连接，也包括某些网络设备或配置实际上改变了 Zabbix server 出站连接的源 IP 地址。在 Zabbix server 上执行 `telnet <被监控主机 IP> 10050` 来测试连通性。如果你自定义了 agent 监听端口，请在该命令中使用该端口。如果连接未能建立，请按网络问题排查。如果连接立即被关闭，说明 Zabbix agent 守护进程并未将该连接视为来自配置文件中设置的 IP 地址。请注意，在某些情况下，你可能实际上需要使用 IPv6 地址，因为 Zabbix agent 会将其作为传入连接之一接收。

- 问：我尝试使用主动 Zabbix agent 检查来监控一台主机，但不起作用。
- 答：主动监控项稍微复杂一些。以下是一些需要验证的事项：
  - 像普通监控项一样检查网络连通性——在被监控机器上执行 `telnet <Zabbix server IP> 10051`。如果你自定义了 agent 监听端口，请在该命令中使用该端口。
  - 在期望主动监控项返回结果之前，请确保等待 Zabbix server 刷新其配置缓存，并且 agent 守护进程配置文件中 `RefreshActiveChecks` 选项指定的时间已经过去。如果你想强制 agent 从服务器重新加载监控项列表，请重启 agent。
  - 检查 agent 守护进程配置文件中 `Hostname` 选项指定的主机名是否与前端中为该主机配置的主机名一致。请注意，这不是 IP 地址或 DNS 名称；只有主机名才有效——它也不是可见名称，而是所谓的**技术主机名**。与 Zabbix 中几乎所有其他内容一样，它区分大小写。
  - 确保你想要发送主动检查（或从中获取主动检查）的 Zabbix server 已列在 agent 守护进程配置文件的 `ServerActive` 选项中。

![](img/f5313a804532fabacc8d07cc8607eeba_711_0.png)

- 问：我验证过可以在被监控主机上获取到值，但 Zabbix agent 却提示不支持或返回错误数据。
- 答：有几种可能的情况：
  - 你以 root 身份检查诸如进程数之类的内容或使用 `zabbix_agentd -t` 语法，但各种权限限制（包括 `grsecurity` 和 `SELinux`）可能会阻止 Zabbix agent 进行访问。这包括 Zabbix agent 将唯一运行进程数显示为 `0`，即使以 root 身份访问时你能看到实际数量。

- 另一种本地进程数与 Zabbix agent 返回结果不同的情况：各种解释型进程（例如 Python 或 Perl 进程）在 agent 看来可能只是解释器进程，仅带有用户进程作为参数。已知会出现此问题的进程包括 amavisd 和 xend。在这些情况下，你可以使用不同的方法，例如使用 `proc.num[python, , ,xend]` 监控项键。这将查找参数中包含 xend 字符串的 Python 进程。

- 被监控的实例不存在。例如，如果你使用 `net.if.in[eth0,bytes]` 键请求某个指标，而 Zabbix agent 声称不支持，请验证 eth0 接口是否确实存在。

- 另一台服务器配置了具有相同主机名的主动 Zabbix agent，并且也在为该主机发送数据。

- 问：我修改了 agent 守护进程配置文件中的某个参数，但它忽略了我的更改。
- 答：请检查以下几点：

  - 确认修改的那一行没有被注释掉。
  - 确保你编辑的是正确系统上的文件。
  - 检查 Zabbix agent 守护进程是否使用了修改后的配置文件。所有 Zabbix 守护进程在启动时都会记录它们正在使用的配置文件。
  - 检查 Include 指令。要特别注意那些包含目录中所有文件的指令，以及嵌套的 include。
  - 确保你正确重启了守护进程。请注意，仅运行 zabbix_daemon 或 zabbix_daemon restart 并不会重启守护进程。

某些发行版的软件包可能会提供配置文件以及指向该文件的便捷符号链接。如果你在符号链接上使用 `sed -i`，它不会编辑目标文件——而是会用普通文件替换该符号链接。某些版本的 sed 可能提供名为 `--follow-symlinks` 的选项来编辑目标文件。

- 问：我看到配置文件为某个参数指定了一个值，但 Zabbix agent 使用的却是另一个值。
- 答：请参考上一个问题的答案，尤其是关于确保在正确的系统上编辑正确的文件，以及 Include 指令不会覆盖参数第一个实例的部分。

- 问：我尝试在 Windows 系统上使用主动监控项或自动注册，但自动获取的主机名是全大写的，并且被截断为 15 个字符。
- 答：在 agent 守护进程配置文件中设置 `HostnameItem=system.hostname[host]`。我们在*第 14 章，监控 Windows* 中讨论过这个问题。
- 问：我验证了某个监控项在运行 `zabbix_agentd -t` 或 `-p` 时能按预期工作，但在前端检查值时却不行。
- 答：手动运行 `zabbix_agentd` 时，用户和环境很可能与正在运行的守护进程不同，因此权限和环境值也会不同。检查该监控项预期执行的详细操作，以及在使用 Zabbix agent 守护进程权限时什么因素可能阻止其成功执行。不要直接以 root 身份测试 `zabbix_agentd`。最好的方法是使用 `zabbix_get` 对正在运行的 agent 守护进程进行测试。
- 问：我可以在 Zabbix Server 中或使用 `zabbix_get` 获取监控项的值，但当我用 `zabbix_agentd -t` 或 `-p` 测试时，却收到错误 `[m|ZBX_NOTSUPPORTED] [Collector is not started.]`。
- 答：某些监控项（包括 `system.cpu.util` 和 `proc.cpu.util`）的值是由正在运行的 agent 计算的，因为它们在提供有效值之前需要多个采样。此类监控项只有在 Zabbix Server 或 `zabbix_get` 查询 agent 守护进程时才能工作。

## 用户参数

以下列表详细列出了与用户参数相关的常见问题：

- 问：我的用户参数不起作用。
- 答：导致用户参数失效的常见原因如下：
  - 环境缺失是设置用户参数时最大的绊脚石之一。Zabbix agent 不会显式初始化环境细节，例如 `HOME` 变量或其他信息。这可能导致无法读取所需的配置文件以及其他问题。请确保按需设置环境，可以直接在用户参数中设置变量，也可以在包装脚本中设置。
  - 还有一点，如果你以 root 身份运行命令进行测试，Zabbix 用户受限的权限会让你难以排查，因此务必以 Zabbix 用户的身份测试用户参数。如果某个检查需要 root 访问权限，请通过 `sudo` 配置访问权限。





## SNMP 设备

- 返回脏数据也很容易破坏数据检索。使用用户参数检索数据时，请确保其中不包含使其不适合存储的字符（例如为浮点数据类型监控项返回 `26.6 C`），也不包含其他奇怪字符（例如数据字符串末尾带有 CR/LF 换行符）。
- 默认情况下，agent 监控项会在三秒后超时。在大多数情况下，不建议增加此超时时间，不过如果将用户参数变量用作主动式监控项，这样做可能是相当安全的。请记住，主动式监控项不是并行处理的——只有一个 agent 进程在工作，一次一个监控项。请考虑改用 `zabbix_sender` 来处理此类监控项。

- **Q:** 我的 SNMP 监控项不工作。
- **A:** 请仔细检查 SNMP 版本和团体字符串是否设置正确。指定错误的 SNMP 版本通常会导致超时，从而加大调试难度。当然，还要从 Zabbix 服务器使用 `snmpwalk` 和 `snmpget` 命令检查常规连通性和权限。
- **Q:** 我的 SNMP 监控项要么完全不工作，要么频繁失败。
- **A:** 也许您的设备不能正确支持 `SNMP GETBULK`——请尝试在主机属性中为 SNMP 接口禁用批量获取（bulk get）支持。
- **Q:** 我导入了模板，但 LLD 失败，错误信息为 `Invalid SNMP OID: pairs of macro and OID are expected.`
- **A:** Zabbix SNMP LLD 的键（key）语法在 Zabbix 2.4 中发生了变化。不幸的是，XML 导入过程并未相应更新，导致导入的 LLD 规则使用了旧语法。有关键语法的详细信息，请参阅*第 12 章，自动化配置*。
- **Q:** 我添加了 MIB 文件，它们在命令行工具中工作正常，但 Zabbix Server 似乎忽略了这些 MIB 文件。
- **A:** 请确保重启服务器守护进程——MIB 定义仅在启动时加载。

> 此外，请确保你不会因为过于频繁地查询大量值而使被监控设备过载。

- **Q:** 我的 SNMP 监控项可以工作，但特定设备上的某些 OID 不工作，尽管数据出现在 `snmpwalk` 输出中。
- **A:** 尝试对这些 OID 使用 `snmpget`。已知某些 UPS 的固件存在缺陷，导致这些指标无法通过 `GET` 请求工作，但可以通过 `snmpwalk` 所使用的 `GETNEXT` 请求正常工作。如果属于这种情况，请升级设备固件。
- **Q:** 我在 `/etc/snmp/snmp.conf` 中列出了我想使用的所有 SNMP MIB，但 Net-SNMP 工具并未全部正确使用它们。
- **A:** 某些 Net-SNMP 版本会悄悄将此文件中的行截断为 1024 个字符（包括换行符）。请尝试将选项拆分为多行，使单行不超过 1023 个可打印字符。
- **Q:** 我正在监控网络流量，但返回的数据不正确。
- **A:** 如果是高速接口，请确保使用 64 位计数器 OID，例如 `ifHCInOctets` 和 `ifHCOutOctets`。
- **Q:** 我正在向 Zabbix 添加 SNMP 设备，但添加新设备会停止对先前设备的监控。如果我使用 `snmpget` 查询每台设备，它们仍然会按预期响应。
- **A:** 如果是 SNMPv3，请确保所有设备都有唯一的 `snmpEngineID` 变量。

## IPMI 监控

- **Q:** 我无法让 IPMI 监控项工作。
- **A:** 当 IPMI 监控项无法工作时，有几点需要检查：
  - 确保 Zabbix 服务器已配置 IPMI 支持。这很简单，但容易被忽略。
  - 检查服务器配置文件中的 `StartIPMIPollers` 选项是否设置为默认值 `0`。如果是，请将其设置为 `1` 并重启 Zabbix Server。
  - 确保传感器名称正确。你可以使用 `IPMItool` 获取传感器名称，并且必须使用 `IPMItool` 输出中显示的名称，保留空格，不要加引号。
  - 检查是否使用了最新的 OpenIPMI 版本。已知较旧版本的 OpenIPMI 存在各种问题。

## ICMP 检查

- **Q:** 我的所有 ICMP 检查都失败了。
- **A:** 以下是几个可能的原因：

  检查 `fping` 是否可由运行 Zabbix Server 的用户以 root 身份通过 `setuid` 运行。

  确保 SELinux 不会阻止 Zabbix 运行 `fping`。`grep fping /var/log/audit/audit.log` 命令可能会提供更多信息。

## 简单检查的问题

- **Q:** 我的服务正常工作，但 `net.tcp.service` 或 `net.udp.service` 监控项显示它不工作。
- **A:** 除了验证查询的是否是正确的服务器之外，还要检查服务是否按 Zabbix 预期的方式响应——简单检查的详细信息参见 `https://www.zabbix.com/documentation/3.0/manual/appendix/items/service_check_details`。

## zabbix_sender 和 trapper 监控项的问题

- **Q:** 我发送带时间戳的值，但服务器数据库中输入了不同的时间戳。
- **A:** Zabbix sender 会在整个请求的 `clock` 属性中携带主机上的当前时间，Zabbix 服务器会据此调整所有值的时间戳。无法告知服务器不要这样做，也无法告知 sender 不要发送该时间戳。要么修复发送系统上的时间，要么在不发送请求时间戳的情况下实现基本协议。

## 常见问题

- **Q:** 我正在监控网络流量，但数字大得不切实际。
- **A:** 由于数据很可能以计数器（counter）的形式提供，请确保在 Zabbix 中将结果存储为差值（每秒速度）。
- **Q:** 我正在以字节/秒为单位监控 10G 接口速率，当接口处于负载状态时，我会丢失值。
- **A:** 确保**信息类型**设置为**数字（无符号）**。这样，你会损失几分之一位的精度，但会保留所有值。
- **Q:** Zabbix 不接受我的计算监控项的公式。

- A: 请确保使用正确的引号，尤其是当引用的监控项键带有引号时。例如，如果引用的监控项键是 `key["parameter",param]`，在计算监控项公式中，可以像这样使用：`last("key[\"parameter\",param]")`。请注意用反斜杠转义内部的双引号。
- **Q:** 我试图使用像 `proc.num['apache']` 这样的监控项键，但它不工作。
- **A:** Zabbix 只支持双引号；不要使用单引号来引用。
- **Q:** 我试图使用像 `{host:item.LAST()=13}` 这样的触发器表达式，但它不工作。
- **A:** 区分大小写——在 Zabbix 中几乎一切内容都区分大小写：监控项键、它们的参数、主机名、触发器函数等等。如果你来自 Windows 环境，请不断提醒自己大小写很重要。

## 触发器

- **Q:** 我的触发器不工作，或者 Zabbix 拒绝添加我的触发器。
- **A:** 检查触发器的语法，特别注意括号——是否使用了正确的类型？它们都正确闭合了吗？引号也一样，并记住要区分大小写。尝试拆分复杂的表达式以定位错误。

## 动作

- **Q:** 我的动作不工作。
- **A:** 如果通知没有出现在**报告 | 动作日志**中，请确保你想要发送通知的用户至少对参与生成事件的主机之一具有读取权限。另外，还要检查动作条件、主机维护设置和动作操作。确保你的动作没有被禁用——如果动作条件或操作中引用的资源被删除，Zabbix 可能会静默地自动禁用这些动作。还要检查用户媒体设置，例如严重性和时间过滤器，以及所配置的媒体类型是否已启用。如果消息确实出现在动作日志中且存在错误消息，希望错误信息能有所帮助。如果消息出现在动作日志中并显示为成功发送，请检查你的 MTA 或其他接收系统的日志。
- **Q:** 我的电子邮件通知没有发送，我可以在电子邮件服务器日志文件中看到类似 `[127.0.0.1] did not issue MAIL/EXPN/VRFY/ETRN during connection to MTA in e-mail server log files.` 的错误消息。





## 故障排除

- **Q:** 这些消息很可能是由 Zabbix 监控 SMTP 服务引起，而非通知尝试。检查上一问中提到的权限，并检查 **Reports | Action log** 中的操作日志以了解通知失败的原因。
- **Q:** 发生了什么事，我的 Zabbix 服务器正在发送大量消息。我能快速停止吗？
- **A:** 存在一种强硬的方法来停止失控或过度的升级——你可以删除所有当前活动的升级。请注意，即使删除活动升级，Zabbix 也会创建新的升级——一个好的解决方法是让操作条件仅在触发器未被确认时才发送消息，并确认有问题的触发器。注意：这也会移除正确的升级。在正确的数据库中执行以下操作：

```
mysql> delete from escalations;
```

## 发现与自动注册

- **Q:** 我将一台主机从某个主机组中移除，但它后来神秘地被重新添加。
- **A:** 检查网络发现和主动代理自动注册操作——很可能它们重新添加了该主机。
- **Q:** 我将一台主机移动到由特定代理或 Zabbix 服务器实例监控，但它后来又改回另一个代理或 Zabbix 服务器实例。
- **A:** 检查主动代理自动注册操作以及代理上的 `ServerActive` 参数。创建的主机将被分配给最后接收到自动注册请求的代理或服务器。
- **Q:** 我禁用了一个 LLD 原型，但下游的项目或触发器未被禁用。
- **A:** 不幸的是，这是设计使然，无法更改。你可以在配置列表中禁用单个项目和触发器。对于更改大量下游项目或触发器的状态，尝试使用 Zabbix API。

## 故障排除 Zabbix

之前的所有问答涵盖了一些新用户可能遇到的最常见问题。可能还会遇到许多其他问题，并且随着 Zabbix 新版本的发布，会出现新的问题。虽然快速解决常见问题是好的，但让我们看看在调试 Zabbix 问题时可能有帮助的一些细节。

## Zabbix 日志文件格式

当出现无法解释的问题时，我们首先应该检查的地方之一是日志文件。这不仅仅是 Zabbix 特有的事情；日志文件非常有用。有时。其他时候，它们没有帮助，但我们将讨论日志文件没有提供答案时的其他选项。为了能够找到答案，了解日志文件格式的一些基本知识是有帮助的。Zabbix 日志格式如下：

```
PPPPPP:YYYYMMDD:HHMMSS.mmm
```

这里，PPPPPP 是进程 ID，用空格填充到 6 个字符，YYYYMMDD 是当前日期，HHMMSS 是当前时间，mmm 是时间戳的毫秒。冒号和点是字面符号。此前缀后跟一个空格，然后是实际的日志消息。以下是一个示例日志条目：

```
10372:20151223:134406.865 database is down: reconnecting in 10 seconds
```

如果日志文件中有一行没有此前缀，它很可能来自外部源，例如脚本，或来自 Net-SNMP 等库。

启动期间，会记录类似以下内容的输出：

```
12331:20151215:163629.968 Starting Zabbix Server. Zabbix 3.0.0
(revision {ZABBIX_REVISION}).
12331:20151215:163630.020 ****** Enabled features ******
12331:20151215:163630.020 SNMP monitoring:           YES
12331:20151215:163630.020 IPMI monitoring:           YES
12331:20151215:163630.020 Web monitoring:            YES
12331:20151215:163630.020 VMware monitoring:         YES
12331:20151215:163630.020 SMTP authentication:       YES
12331:20151215:163630.020 Jabber notifications:      NO
12331:20151215:163630.020 Ez Texting notifications:  YES
12331:20151215:163630.020 ODBC:                      YES
12331:20151215:163630.020 SSH2 support:              YES
12331:20151215:163630.020 IPv6 support:              NO
12331:20151215:163630.020 TLS support:               NO
12331:20151215:163630.020 *******************************
12331:20151215:163630.020 using configuration file: /usr/local/etc/zabbix_server.conf
12331:20151215:163630.067 current database version (mandatory/optional): 03000000/03000000
12331:20151215:163630.067 required mandatory version: 03000000
```

## 故障排除

第一行打印出守护进程类型和版本。根据编译方式，它还可能包含当前的 SVN 修订号。随后是编译时功能的列表——非常有用，可以知道是否应该期望 SNMP、IPMI 或 VMware 监控能够工作。然后，显示当前使用的配置文件路径——当我们想弄清楚我们更改的文件是否是正确的文件时很有帮助。在服务器和代理日志文件中，都存在当前和所需的数据库版本——我们在 *Chapter 22, Zabbix Maintenance* 中讨论过这些。

数据库版本之后，可以找到内部进程启动消息：

```
2583:20151231:155712.323 server #0 started [main process]
2592:20151231:155712.334 server #5 started [poller #3]
2591:20151231:155712.336 server #4 started [poller #2]
2590:20151231:155712.337 server #3 started [poller #1]
2593:20151231:155712.339 server #6 started [poller #4]
```

会有更多类似的行；这里的输出已被精简。这有助于验证是否已启动预期数量的某种类型的进程。查看日志文件内容时，并不总是明显哪个进程记录了特定行——而启动消息可以在这里提供帮助。如果我们看到如下行，我们可以找出哪个进程记录了它：

```
21974:20151231:184520.117 Zabbix agent item "vfs.fs.size[/,free]" on
host "A test host" failed: another network error, wait for 15 seconds
```

我们可以通过查找具有相同 PID 的启动消息来做到这一点：

```
# grep 21974 zabbix_server.log | grep started

21974:20151231:184352.921 server #8 started [unreachable poller #1]
```

![](img/f5313a804532fabacc8d07cc8607eeba_720_0.png)

> 如果返回多行，请运用常识找到启动消息。

这表明主机在第一次网络失败后会被延迟到不可达轮询器。

但如果日志文件已被轮转，原始启动消息丢失了呢？除了更高级的侦探工作，还有一种简单的方法，前提是守护进程仍在运行。我们将在本章稍后讨论这种方法。

## 重新加载配置缓存

我们在 Chapter 2, Getting Your First Notification 中遇到了配置缓存，并在 Chapter 22, Zabbix Maintenance 中讨论了监控它的方法。虽然它在性能方面帮助很大，但在我们尝试快速测试某些东西时可能会有点问题。可以强制 Zabbix 服务器重新加载配置缓存。运行以下命令以显示 Zabbix 服务器选项：

```
# zabbix_server --help
```

> 我们在 Chapter 19, Using Proxies to Monitor Remote Locations 中简要讨论了 Zabbix 代理配置缓存重新加载。

在输出中，查找运行时控制选项部分：

```
-R --runtime-control runtime-option  Perform administrative functions
Runtime control options:
    config_cache_reload  Reload configuration cache
```

因此，可以通过以下方式启动服务器配置缓存的重新加载：

```
# zabbix_server --runtime-control config_cache_reload
```

```
zabbix_server [2682]: command sent successfully
```

检查服务器日志文件将显示它已接收到信号：

```
forced reloading of the configuration cache
```

在后台，发送信号的过程如下：

1. 服务器二进制文件查找默认配置文件。
2. 然后它查找 PidFile 选项指定的文件。
3. 它向具有该 ID 的进程发送信号。

如 Chapter 19, Using Proxies to Monitor Remote Locations 中所述，这个功能的好处是它也支持主动 Zabbix 代理。更好的是，当主动代理被指示重新加载其配置缓存时，它会连接到 Zabbix 服务器，获取所有最新配置，然后重新加载本地配置缓存。如果向被动代理发送此类信号，它会忽略该信号。





## 故障排查

如果同一系统上运行着多个代理进程，你如何告诉二进制程序具体哪个实例应该重新加载配置缓存？回顾之前向进程发送信号的过程，所需要做的只是指定正确的配置文件。如果在同一系统上运行多个代理进程，每个进程必须已经有自己的配置文件，指定不同的 PID 文件、日志文件、监听端口等。让使用特定配置文件的代理重新加载配置缓存只需这样：

```
# zabbix_proxy -c /path/to/zabbix_proxy.conf --runtime-control config_cache_reload
```

> 必须提供配置文件的完整绝对路径；不支持相对路径。

> 同样的原则也适用于服务器和代理，但在同一系统上运行多个 Zabbix 服务器的情况更为少见。

如果我们有一个大型 Zabbix 服务器实例，并且显著增加了 CacheUpdateFrequency 参数，手动重新加载配置缓存会很有用。

## 控制运行中的守护进程

配置缓存重载只是运行时部分提供的功能之一。让我们看看其中其余选项：

```
housekeeper_execute                 Execute the housekeeper
log_level_increase=target            Increase log level, affects all processes
if target is not specified
log_level_decrease=target            Decrease log level, affects all processes
if target is not specified
Log level control targets: pid
Process identifier process-type All processes of specified type (for
example, poller)
process-type,N                    Process type and number (e.g., poller,3)
```

如*第22章：Zabbix维护*中所讨论的，内部 housekeeper 会在服务器或代理启动 30 分钟后首次运行。housekeeper_execute 运行时选项允许我们随时运行它：

```
# zabbix_server --runtime-control housekeeper_execute
```

更有趣的是能够更改运行中进程的日志级别。该功能首次出现在 Zabbix 2.4 中，让调试变得容易得多。Zabbix 守护进程通常启动后即可正常运行——直到我们需要修改某些内容。虽然我们无法让任何守护进程重新读取其配置文件，但还有几个选项允许我们控制运行中守护进程的某些方面。正如第22章：Zabbix维护中简要提到的，DebugLevel 参数允许我们在守护进程启动时设置日志级别，默认值为 3。日志级别 4 会添加所有 SQL 查询，日志级别 5 还会添加来自 Web 监控和 VMware 监控的接收内容。对于不熟悉的人来说，任何高于 3 的级别都可能令人惊讶甚至畏惧。即使是一个很小的 Zabbix 服务器，在日志级别 4 下也很容易在几分钟内记录数十兆字节的日志。由于某些问题可能不会立即出现，人们可能需要在日志级别 4 或 5 下运行数小时或数天。想象一下需要处理你并不熟悉的 GB 级日志。为运行中的进程设置日志级别的能力，使我们能够在出现问题期间提高日志级别，然后再降低，而无需重启守护进程。

更妙的是，在使用运行时日志级别功能时，我们可以选择哪些具体组件的日志级别需要更改。单个进程可以通过其系统 PID 或 Zabbix 内部的进程编号来标识。通过系统 PID 指定进程可以这样做：

```
# zabbix_server --runtime-control log_level_increase=1313
```

指定单个 Zabbix 进程的方法是选择进程类型，然后传入进程编号：

```
# zabbix_server --runtime-control log_level_increase=trapper,3
```

一个非常有用且常见的做法是更改某一类型所有进程的日志级别——例如，我们不知道哪个 trapper 会接收导致问题的连接，因此我们可以省略进程编号，轻松地提高所有 trapper 的日志级别：

```
# zabbix_server --runtime-control log_level_increase=trapper
```

如果此运行时选项不带任何参数，它将会影响所有 Zabbix 进程：

```
# zabbix_server --runtime-control log_level_increase
```

当进程被要求更改日志级别时，它们会记录一条相关日志，然后更改日志级别：

```
21975:20151231:190556.881 log level has been increased to 4 (debug)
```

注意，无法查询当前日志级别或设置特定级别。如果你不确定所有进程当前的日志级别，有两种方法可以解决：

- 重启守护进程
- 将日志级别降低或升高五次，确保其处于 0 或 5，然后设置所需级别

作为对我们刚才探讨的选项的简单测试，提高所有 poller 的日志级别：

```
# zabbix_server --runtime-control log_level_increase=poller
```

跟踪 Zabbix 服务器日志文件：

```
# tail -f /tmp/zabbix_server.log
```

注意一个很小的 Zabbix 服务器上仅 5 个 poller 进程就能产生多少数据。然后降低日志级别：

```
# zabbix_server --runtime-control log_level_decrease=poller
```

## 运行时进程状态

Zabbix 还有另一个有助于调试的小技巧。运行 top，看看哪种模式能给你更稳定、更长的 Zabbix 进程列表——按处理器使用率排序（按 Shift + P）或按内存使用率排序（按 Shift + M）可能都行。

> 或者，按 o 并键入 COMMAND=zabbix_server。

按 C，注意 Zabbix 进程如何更新其命令行，以显示它具体是哪个内部进程以及它在做什么：

```
zabbix_server: poller #1 [got 0 values in 0.000005 sec, idle 1 sec]
zabbix_server: poller #4 [got 1 values in 0.000089 sec, idle 1 sec]
zabbix_server: poller #5 [got 0 values in 0.000004 sec, idle 1 sec]
```

跟踪它们的状态，看看某些进程的任务和耗时如何变化。我们也可以将输出重定向或通过其他命令进行过滤：

```
# top -c -b | grep zabbix_server
```

## 进一步调试

可能出错的地方很多，可用来查明原因的工具也很多。如果你熟悉这些工具箱，包括 `tcpdump`、`strace`、`ltrace` 和 `pmap` 等工具，你应该能够解决大多数 Zabbix 问题。

> 有些人声称一切都是 DNS 问题。
> 他们通常是对的——如果其他方法都不奏效，请检查 DNS。
> 以防万一。

我们不会在这里讨论它们，因为这超出了范围——那是通用的 Linux 或 Unix 调试。当然，仍有许多 Zabbix 特有的问题可能出错。你可能想查看 wiki 上的 Zabbix 故障排查页面：http://zabbix.org/wiki/Troubleshooting。如果这没有帮助，请务必查看社区和商业支持选项，例如我们将在*附录 B：成为社区的一员*中讨论的 Zabbix IRC 频道。
