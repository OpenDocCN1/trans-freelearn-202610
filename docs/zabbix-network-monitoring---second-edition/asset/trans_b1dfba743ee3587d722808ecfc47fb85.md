

让我们在这里再做一个更改——切换到 Messaging 选项卡：

![](img/f5313a804532fabacc8d07cc8607eeba_65_0.png)

它允许你配置前端消息。现在，只需将 Frontend messaging 选项标记为启用，并将 Message timeout (seconds) 改为 180。我们将在本章后面讨论这些选项的作用，当消息开始出现时。

> 验证 Trigger severity 部分中的所有复选框是否都已标记：如果之前保存过用户配置文件，它们可能具有不同的默认状态。

更改主题并启用前端消息后，单击 Update 按钮。

注意我们用来到达特定主机项目列表的方法——我们使用了主机元素的便捷链接，这是一种相当简单的方式到达那里，也是我们将经常使用 **Configuration | Hosts** 的原因。

回到我们想要的内容，我们可以看到一个相当长的已存在项目的列表。但等等，Zabbix 状态屏幕在第一张截图中声称只有一个主机且没有项目？这显然是错误的！返回到 **Reports | Status of Zabbix**（或 **Monitoring | Dashboard**，显示相同的数据）。它确实显示项目数为零。现在将鼠标光标移到写着 **Number of items (enabled/disabled/not supported)** 的文字上，并查看工具提示：

| Status of Zabbix |
| PARAMETER | VALUE | DETAILS |
| Zabbix server is running | No | localhost:10051 |
| Number of hosts (enabled/disabled/templates) | 39 | 0 / 1 / 38 |
| Number of items (enabled/disabled/not supported) | 0 | 0 / 0 / 0 |
| Number of triggers (enabled/disabled [problem] | 0 | 0 / 0 [0 / 0] |
| Number of users (online) | 2 | 1 |
| Required server performance, new values per second | 0 |  |

*Tooltip: Only items assigned to enabled hosts are counted*

啊哈！所以它只计算分配给已启用主机的项目。由于此示例主机 Zabbix server 已被禁用，因此现在很清楚为什么 Zabbix 状态报告显示项目数为零。记住这一点很有帮助，以后当你尝试评估更复杂的配置时。

## Creating a host

与其使用此预定义的主机配置，我们想了解项目的工作原理。但项目不能存在于空旷的空间中——每个项目都必须附加到一个主机。

> 在 Zabbix 中，主机是一个逻辑实体，用于对项目进行分组。主机定义的定义可以自由地适应特定的环境和情况。Zabbix 绝不限制这种选择；因此，主机可以是网络交换机、物理服务器、虚拟机或网站。

如果需要主机来附加项目，那么我们必须创建一个。前往 Configuration | Hosts 并单击右上角的 Create host 按钮。你将看到一个主机创建屏幕。这次，我们不会关心细节，所以让我们只输入一些相关信息：

- Name: 输入 A test host。
- Groups: 从右侧列表框中选择 Linux servers，名为 Other groups；按 ◄ 按钮添加此组。然后，从 In groups 列表框中选择 Zabbix servers 并按 ► 按钮将我们新主机从此预定义组中移除。

> 为什么我们必须为此主机选择一个组？所有权限都是分配给主机组，而不是单个主机，因此主机必须属于至少一个组。我们将在第 5 章 Managing Hosts, Users and Permissions 中更详细地介绍权限。

我们为主机更改的字段应如下所示：

![](img/f5313a804532fabacc8d07cc8607eeba_68_0.png)

准备就绪后，单击底部的 Add 按钮。

## Creating an item

所以，我们创建了我们自己的第一个主机。但由于项目是所有数据的基础，现在可能用处不大。为了让它更有实质内容，我们应该创建项目，因此从 Groups 下拉菜单中选择 Linux servers，然后单击我们刚刚创建的主机 A test host 旁边的 Items。此主机没有要列出的项目——单击右上角的 Create item 按钮。

有一个表单，模糊地类似于主机创建表单，所以让我们填写一些值：

- **Name:** 在此字段中输入 `CPU load`。这就是项目的名称——基本上，你将在大多数地方用来引用项目的名称。
- **Key:** 此字段中的值为 `system.cpu.load`。这是项目的“技术名称”，标识它收集的信息。
- **Type of information:** 选择 `Numeric (float)`。这定义了传入数据的格式和类型。

填写所有必填信息后，你将看到以下屏幕：

![](img/f5313a804532fabacc8d07cc8607eeba_69_0.png)

我们稍后会更详细地查看其他默认值，所以单击底部的 **Add** 按钮。

> 有关项目键的更多信息，请参阅 *Chapter 3, Monitoring with Zabbix Agents and Basic Protocols。*

你现在应该在列表中看到你的新项目。但我们对相关数据感兴趣，所以导航到 **Monitoring | Latest data**。注意到占据半页的过滤器吗？这次，我们希望立即使用它。

从 Zabbix 2.4 开始，出于性能原因，**Latest data** 页面默认不显示任何数据；因此，我们必须先设置过滤器：

![](img/f5313a804532fabacc8d07cc8607eeba_69_1.png)

在 **Filter** 中，在 **Hosts** 字段中输入 `test`。我们新主机应该会出现。单击它，然后单击 **Filter** 按钮。在过滤器下方，如果已折叠，请展开 - other - 部分。你可能需要等待保存项目后最多一分钟，然后你应该会看到此新创建的项目已经收集了一些数据：

| NAME ▲ | LAST CHECK | LAST VALUE |
|---|---|---|
| - other - (1 item) | | |
| CPU load | 2016-02-16 20:44:55 | 0.05 |

如果完全看不到任何条目怎么办？这通常意味着数据未被收集，这可能有多种原因。如果是这种情况，请检查以下常见原因：

- 你是否完全按照屏幕截图输入了项目配置？检查项目键和信息类型。
- 代理和服务器是否都在运行？你可以通过以下方式作为 root 执行来检查：
  ```
  # netstat -ntpl | grep zabbix
  ```
  输出应列出服务器和代理守护进程在正确的端口上运行：
  ```
  tcp 0 0 0.0.0.0:10050 0.0.0.0:* LISTEN 23569/zabbix_agentd
  tcp 0 0 0.0.0.0:10051 0.0.0.0:* LISTEN 23539/zabbix_server
  ```
  如果其中任何一个缺失，请确保启动它。
- 服务器能否连接到代理？你可以从 Zabbix 服务器执行以下操作来验证：
  ```
  $ telnet localhost 10050
  ```
  如果连接失败，可能意味着代理未运行或某些限制性防火墙设置阻止了连接。在某些情况下，SELinux 也可能阻止该连接。
  如果连接成功但立即关闭，则代理接收到的连接 IP 地址与 `zabbix_agentd.conf` 配置文件中 `Server` 指令指定的地址不匹配。在某些发行版中，这可能是由于默认使用 IPv6，因此你应该尝试向该指令的同一行添加另一个以逗号分隔的值，用于 IPv6 localhost 表示形式 `::1`。

Zabbix 服务器默认每分钟将要监控的所有项目信息读入缓存一次。这意味着添加新项目等配置更改可能在收集的数据中在一分钟后才会显示效果。此间隔可以在 `zabbix_server.conf` 中通过更改 `CacheUpdateFrequency` 参数进行调整。

一旦数据开始到达，你可能会在 **Change** 列中看到没有值。这意味着你很快就转到此显示，并且项目只设法收集了一个值；因此，尚无变化。如果是这种情况，等待片刻应该会导致页面自动刷新（查看页面标题——还记得我们在用户配置文件中未更改的 30 秒刷新吗？），并且 **Change** 列将被填充。所以，我们现在正在监控一个单一值：UNIX 系统负载。数据会自动检索并存储在数据库中。如果你不熟悉这个概念，阅读 `https://en.wikipedia.org/wiki/Load_(computing)` 上的概述可能是一个好主意。

## Introducing simple graphs

如果你去阅读系统负载，几分钟应该已经过去了。现在是查看 Zabbix 中的另一个功能 **Graphs** 的好时机。图形可自由用于任何监控的数值项目，无需额外配置。

你应该仍在 **Latest data** 屏幕上，并且可以看到 **CPU load** 项目，因此单击名为 **Graph** 的链接。你会得到类似这样的内容：

![](img/f5313a804532fabacc8d07cc8607eeba_71_0.png)

