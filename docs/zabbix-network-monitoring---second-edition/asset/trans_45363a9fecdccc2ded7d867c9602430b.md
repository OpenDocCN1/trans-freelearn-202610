

所以这个项目将记录代理上次联系 Zabbix 服务器的时间。这很好，但不足以在日常工作中注意到问题——我们已经很清楚触发器绝对是必需的。这里，熟悉的 fuzzytime() 函数就派上用场了。导航到 Configuration | Hosts，点击你在其上创建了代理最后访问项目的那个主机旁边的 Triggers，然后点击 Create trigger 按钮。

假设我们有一个负载较重且关键的代理——我们希望知道代理在三分钟内没有回传时。 在这种情况下，可以使用如下的触发器表达式：

```
{host:zabbix[proxy,proxy name,lastaccess].fuzzytime(180)}=0
```

> 可以考虑对最后访问项目使用 delta Store value，当代理未通信时它会返回 0。 针对此类项目的触发器更晦涩，因此 fuzzytime() 是此用途最常见的触发器函数。

![](img/f5313a804532fabacc8d07cc8607eeba_611_0.png)

正如我们可能回忆的那样，代理在两种情况下连接到服务器——它要么同步配置，要么发送收集到的数据。 如果由于某种原因，这两种事件的所有发生都相隔超过三分钟怎么办？ 幸运的是，Zabbix 代理有一个心跳进程，它会定期向服务器报告。 更妙的是，这个时间是可以配置的。 再看一下 zabbix_proxy.conf，这次查找 HeartbeatFrequency 变量，默认如下：

```
# HeartbeatFrequency=60
```

以秒为单位，这个值意味着代理每分钟都会向服务器报告，即使没有新的值需要发送。 lastaccess 项目是判断代理很可能已宕机或至少无法访问的相当可靠的方法，即使它在较长时间内不会发送数据。

对于我们的触发器，填写以下值：

- Name: Proxy "First proxy" not connected for 3 minutes
- Expression: {Another host:zabbix[proxy,First proxy,lastaccess].fuzzytime(3m)}=0
- Severity: High

> 用代理最后访问项目创建的主机名替换代理名称。 如果最后访问项目使用了 {HOST.HOST} 宏，也在触发器名称和表达式中使用相同的宏。

![](img/f5313a804532fabacc8d07cc8607eeba_611_1.png)

我们本可以使用 `180` 代替 `3m`，但带时间后缀的版本更易读。 时间后缀在 *Chapter 6, Detecting Problems with Triggers* 中有讨论。 完成后，点击底部的 **Add** 按钮。

这种项目和触发器的组合将很好地在代理不可用时提醒我们。 现在我们只需要为该代理后面的所有可用性触发器设置触发器依赖关系，使其依赖于此代理最后访问触发器。

不幸的是，有一个常见的问题情况。 当代理-服务器通信中断时，代理最后访问触发器会触发并因依赖关系而掩盖所有其他触发器。 虽然代理在一段时间内无法连接到服务器，但它仍在收集值。 一旦通信恢复，代理就会将所有值发送到服务器，*先发送旧值*。 第一个值发送的瞬间，最后访问项目会被更新并且触发器恢复。 不幸的是，在这一点上，代理仍在发送收集了 5、30 或 60 分钟前的值。 任何检查较短周期的 `nodata()` 触发器都会触发。 这使得代理触发器依赖关系仅在代理恢复之前有效，并在其恢复时导致巨大的事件风暴。 我们如何解决它？ 我们可以尝试找出代理有多少未发送的值，如果太多，就忽略代理后面的所有触发器——本质上，将具有大量值缓冲区的代理视为与无法到达的代理相同。

## Method 2 – Internal proxy buffer item

我们可以转向 Zabbix 内部项目来确定代理缓冲区的大小——即它有多少值需要发送到 Zabbix 服务器。 让我们前往 **Configuration | Hosts**，点击 **Another host** 旁边的 **Items**，然后点击 **Create item**。 填写以下值：

- **Name:** `First proxy: buffer size`
- **Type:** **Zabbix internal**
- **Key:** `zabbix[proxy_history]`

> 此项目必须在通过要监控缓冲区大小的代理监控的主机上创建。 如果分配给主机并由 Zabbix 服务器监控，此项目将变为不支持。

完成后，点击底部的 **Add** 按钮。 在默认的代理配置更新间隔为一小时的情况下，在我们可以看到此项目的结果之前可能需要相当长的时间。 为加快配置更新，在代理主机上运行以下命令：

```bash
zabbix_proxy --runtime-control config_cache_reload
```

代理将从服务器请求项目配置并更新其自己的缓存。 过一会儿，我们应该能在最新数据页面中看到结果：

| NAME | LAST CHECK | LAST VALUE |
|---|---|---|
| **- other -** (1 Item) |  |  |
| First proxy: buffer size | 2015-11-26 14:44:59 | 0 |

那是什么值呢？ 它很简单，就是仍在代理缓冲区中且必须发送到服务器的值的数量。 这可能允许我们针对此项目创建触发器。 每当缓冲区大于一百、二百或一千个值时，我们就会认为代理数据不是最新的，并使所有主机触发器依赖于缓冲区大小。 除了仍然存在一个重要问题。 此项目的数值保存在它所监控的同一代理缓冲区中，并受相同的顺序发送影响，先发送旧值。 使用此项目，我们仍会遭受与之前相同的问题——代理不可用时，代理缓冲区项目会保持 0 或其他较小值。 随着值的开始流入，各个主机触发器会触发，只有在一段时间后我们才会看到缓冲区实际上非常大。 这对以后的调试可能有用，但无助于屏蔽代理后面的主机。 那么有解决方案吗？

### Method 3 – Custom proxy buffer item

一种解决方案可能是某种绕过缓冲区本身向我们发送代理缓冲区大小的方法。 Zabbix 没有提供这样的方法，因此我们必须自己实现。 在此之前，让我们弄清楚如何获取缓冲区大小的信息。 为此，我们将深入代理数据库。

![](img/f5313a804532fabacc8d07cc8607eeba_613_0.png)

## Using Proxies to Monitor Remote Locations

在代理主机上运行：

```bash
$ sqlite3 /tmp/zabbix_proxy.db
```

代理将所有收集的值保存在一个表 `proxy_history` 中。 让我们获取最后三个收集的值：

```text
sqlite> select * from proxy_history order by id desc limit 3;
1850|24659|1448547689|0|0|0|0|0|749846420|0|0|0|0
1849|23872|1448547664|0|0|0.000050|0|655990544|0|0|0|0
1848|24659|1448547659|0|0|0|0|0|744712272|0|0|0|0
```

我们将在 *Chapter 21, Working Closely with Data* 中更详细地讨论其他字段，但现在知道第一个字段是顺序 ID 就足够了。 尽管如此，代理如何知道哪些值已发送到服务器？ 让我们查看 IDs 表：

```text
sqlite> select * from ids where table_name='proxy_history';
proxy_history|history_lastid|1850
```

这里的 `history_lastid` 值是已同步到服务器的最后一个 ID。 在繁忙的代理上，你可能需要非常快速地运行这些语句才能看到真实情况，因为新值会不断添加并发送到服务器。 我们可以用以下方法获取当前缓冲区（未发送值）大小：

```sql
sqlite> select (select max(proxy_history.id) from proxy_history)-nextid
from ids where field_name='history_lastid';
```

它将计算最大 ID 与 `history_lastid` 值之间的差值。 在我们的代理上，这很可能一直返回 0。

> 尝试停止 Zabbix 服务器并查看此值如何增加。
> 不要忘记再次启动 Zabbix 服务器。

现在我们应该将其放入一个项目中。 最重要的是确保此项目直接由服务器处理，而不涉及 Zabbix 代理。 我们有几个选项：

- Passive agent item
- Active agent item
- Zabbix trapper item that is populated by `zabbix_sender`

