

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

## 成为社区的一员

Zabbix 的一个重要方面是其开源特性。Zabbix 是一个真正的开源解决方案——它不是“开放核心”，也没有企业版或某些专有插件。这类方法可被贴上“假开源”的标签。Zabbix 的所有组件都是完全开源的；没有封闭或隐藏的组件。其开发者已承诺让 Zabbix 保持这种状态——欢迎查看 Zabbix 2013 年会议视频，了解这一点：参见 Zabbix 创建者 Alexei Vladishev 的开场演讲，大约在 27 分 30 秒处。

除了开源之外，Zabbix 的很多开发也是公开进行的。这使得人们可以轻松地密切关注开发进展并获得社区支持。但每个开源项目的运作方式和指导方针各不相同，所以让我们看看在 Zabbix 的这一方面，人们可以期待什么：

- 社区支持是解决问题的一个很好的途径，可以通过在 IRC 频道聊天、查看 wiki、在官方论坛讨论，或使用开放的错误跟踪器来实现
- 通过获取最新源代码来更紧密地跟进开发，可以让人们尽快尝试问题的修复方案，提供早期反馈，并更加熟悉 Zabbix 的内部机制
- 对于产品开发、支持合同或其他服务，商业支持可能会很方便

Zabbix 的开发是公开进行的，但通常不接受外部贡献，只有一个领域例外：翻译。欢迎为 Zabbix 的所有翻译作出贡献，我们还将了解如何参与其中。

