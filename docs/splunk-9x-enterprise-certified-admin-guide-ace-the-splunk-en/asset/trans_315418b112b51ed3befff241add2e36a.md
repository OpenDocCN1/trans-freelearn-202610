

安装完成后，以管理员身份登录到 *第 1 章* 的 *独立* 部分中安装的 Splunk Enterprise 实例。登录后，导航到 **设置 | 转发和接收 | 配置接收 | 新接收端口**，然后在监听端口字段中输入 9997，并单击 **保存**。

然后，配置 UF 以连接到接收索引器（在本例中为 Splunk Enterprise），可以通过直接编辑 `outputs.conf` 文件或通过命令行进行配置，就像我们在 *第 4 章 Splunk Forwarder 管理* 的 *配置转发* 部分中所看到的那样。有关更多详细信息，您可以参考 Splunk 文档：https://tinyurl.com/2p8h9nuc。

下图详细说明了我们试图创建的 UF 和索引器（Splunk Enterprise 实例）设置。

![](img/5319cbd6f078c6cda7fac6347f77a07d_158_0.png)

图 9.1：UF 将日志发送到接收索引器

太好了！您现在有一个 UF 代理，可以为您配置输入。让我们从文件和目录监控数据输入开始。

## 文件和目录监控

顾名思义，此输入类型监控数据文件及其存储的目录。这是引入数据最有效的方式，也是 Splunk 推荐的处理文件最佳方法之一。监控设置在 `inputs.conf` 文件中配置。要启用此输入，需要在源机器上安装 UF 代理，相同的设置也适用于 **重型转发器 (HFs)** 和 Splunk Enterprise。让我们来看一下此输入类型的显著特点：

- 适用于所有基于文本的文件，包括结构化格式（XML、CSV、JSON 等）和 .gzip 压缩文件。
- 通过在 `$SPLUNK_HOME/var/lib/splunk/` 下的 `fishbucket` 目录中维护的检查点来跟踪正在监控的文件。
- 在转发器重启的情况下，从上次位置恢复文件和目录监控。
- 递归发现目录中的所有文件，包括新创建的文件。

在网络问题或索引器不可用的情况下，我们可以通过设置 `queueSize` 和 `persistentQueueSize` 来防止数据丢失。`queueSize` 设置指的是在转发器上的输出队列空闲之前保存数据的内存队列。`persistentQueueSize` 设置可在内存队列已满时启用将数据存储到磁盘。`persistentQueueSize` 必须大于 `queueSize`。数据将写入转发器上的 `$SPLUNK_HOME/var/run/splunk/` 目录。

UDP 输入规范与 TCP 相同，只是它监听 UDP 端口——其余设置和语法保持不变。UDP 的 `inputs.conf` 配置示例如下：

```
# inputs.conf listening on UDP port 514
[udp://514]
connection_host = ip
acceptFrom = 192.0.2.10, !external.example.host.com
index = main
sourcetype = access_common
queueSize = 1MB
persistentQueueSize = 100MB
```

网络输入在 *nix 和 Windows 主机上可能存在限制，因为它们需要访问某些端口以监听 TCP/UDP。您可以自行尝试在 UF/Splunk Enterprise 实例上配置这些输入并检查端口的监听状态（在 Linux 和 Windows 主机上使用 `netstat` 命令）。

我们已到达网络输入部分的末尾。在下一节中，我们将介绍脚本化输入的配置。

## 讨论脚本化输入

脚本化输入对于索引无法通过文件/目录监控或使用网络输入监控的瞬态/临时数据非常有用。脚本化输入从瞬态源收集数据，然后将收集到的数据写入文件或直接转发到索引器。让我们了解一下有关此输入类型的一些事实：

- 脚本化输入需要 UF 代理或 Splunk Enterprise 实例（HF）来执行脚本。
- 可以从瞬态源收集数据，例如操作系统命令。`top`、`vmstat`、`netstat` 和 `iostat` 命令都利用这种类型的输入，该输入在可从 http://splunkbase.splunk.com 下载的 Splunk Unix 和 Linux 插件中配置。Windows **技术插件 (TA)** 依赖于此输入类型来收集 Windows **活动目录 (AD)** 日志、注册表日志、WinEventLogs 等。也可以使用脚本从远程 API 拉取日志。
- 支持流行的脚本类型，包括 `.py`（Python）、`.sh`（shell）、`.ps1`（PowerShell）和 `.bat`（Windows 批处理）。
- 脚本化输入必须安排在特定间隔运行。脚本运行的频率通过指定以秒为单位的间隔数或设置 cron 表达式来设置。
- Splunk Enterprise 和 UF 将脚本执行消息记录在本地 splunkd.log 文件中。您可以在 _internal 索引中访问这些日志。
- 脚本化输入可通过 Splunk Web 中的 设置 | 数据输入 | 脚本 菜单进行配置。另一种选择是在 inputs.conf 文件中创建它们，可选择将其打包到应用程序中，并使用部署服务器将其部署到转发器。参考第 4 章，Splunk Forwarder 管理。

考虑以下脚本化输入规范：

```
# inputs.conf scripted input settings
[script://<script path or cmd>]
source = <source to set>
sourcetype = <sourcetype to set>
index = <index name to forward logs>
interval = <in seconds. default 60 sec or cron expression. Set -1 to run once, 0 to continuously>
passAuth = <username to run the script as the OS user>
queueSize = <in-memory queue size>
persistentQueueSize = <queue to store on disk when in-memory is full>
```

前述规范的节名称以 script:// 为前缀，后跟通常的通用元数据字段 source、sourcetype 和 index，以及队列设置。特定于脚本化输入的设置是 interval 和身份验证信息。

interval 设置安排脚本每 x 秒运行一次。如果此处未指定任何内容，脚本将默认安排每分钟运行一次。或者，我们可以指定 cron 表达式。

queueSize 和 persistentQueueSize 的工作原理与 处理网络数据输入 部分中解释的类似——在索引器不可用的情况下，这两个设置都会将脚本生成的数据保存在内存和磁盘中。

passAuth 设置可确保脚本由分配的用户名运行，通过为该用户创建身份验证令牌。在 Splunk 中设置脚本化输入之前，您需要创建脚本并确保它位于 Splunk 可以访问的文件系统路径中。然后 [script://<script path>] 节将能够从该指定位置执行脚本。出于安全原因，脚本必须放在以下位置之一：

- $SPLUNK_HOME/etc/apps/<app name>/bin
- $SPLUNK_HOME/etc/system/bin
- $SPLUNK_HOME/bin/scripts

但是，对于具有 .path 扩展名的脚本存在一个例外，它可以引用主机上任何位置的脚本（.py、.sh、.ps1 和 .bat）。.path 脚本文件本身必须位于上述位置之一，但它引用的脚本可以位于这些目录之外。

让我们看一下以下调用 *nix top 命令以获取进程信息的脚本化输入。在这里，脚本路径相对于 inputs.conf 文件。脚本具有 .sh 扩展名，并安排每 60 秒运行一次。sourcetype 和 source 都设置为 top。此脚本生成的日志将被转发到名为 os（代表操作系统）的自定义索引：

```
[script://./bin/top.sh]
interval = 60
sourcetype = top
source = top
index = os
disabled = 0
```

如果您想参考更多此类示例，请从 https://splunkbase.splunk.com/app/833 下载 Splunk_TA_nix 插件并检查 /default/inputs.conf 和 /bin 目录。

> **提示**
> 最佳做法是将脚本放在靠近 inputs.conf 文件的位置。例如，如果文件位于 $SPLUNK_HOME/etc/system/local/ 目录中，则将脚本放在 $SPLUNK_HOME/etc/system/bin/ 目录中。同样，如果 inputs.conf 位于 $SPLUNK_HOME/etc/apps/<app name>/local 目录中，则将脚本放在 $SPLUNK_HOME/etc/apps/<app name>/bin 目录中。

就目前而言，关于脚本化输入的内容就这些——尝试从 Splunkbase 安装一个使用脚本化输入的插件来测试此功能。在下一节中，我们将介绍 HEC 输入。

## 了解 HEC 输入

HEC 是一种无代理输入类型，不需要在源机器上安装转发器。此输入类型适用于能够通过 HTTP(S) 发送事件的源，例如通过 JavaScript 库的 Web 应用、移动应用和自动化脚本。HEC 在 Splunk Enterprise 实例上公开 RESTful API 端点以接受用于索引的数据。该实例可以在分布式部署中为 HF/索引器。让我们来看一下 HEC 输入的关键事实：

- HEC 默认在 Splunk 实例上处于禁用状态，用户必须手动启用它才能开始使用。它可以通过在多个 Splunk 实例上配置它并选择性地在前面使用负载均衡器来进行扩展。

