

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

