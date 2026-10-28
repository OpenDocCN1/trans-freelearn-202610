

它依赖于 `/sys/devices/system/cpu/` 为每个 CPU 保留一个目录，命名为 `cpu`，后跟 CPU 编号。在这些目录中的每一个里，我们查找 online 文件——如果该文件存在，我们检查其内容。如果内容为 1，则认为 CPU 为在线状态；如果为其他内容——则为离线。在某些情况下，更改 CPU0 的在线状态可能不被允许——此时该文件会缺失，我们会将其解释为 CPU 处于在线状态。然后我们附加 `{#CPU.NUMBER}` 和 `{#CPU.STATUS}` 宏并赋予适当的值，最终将其全部打印出来，包装在 LLD 数据数组中。现在让我们将其用作用户参数。

> 我们在第 11 章《高级项监控》中探讨了用户参数。

我们将全部在一行中拼接，因为此命令不需要包装脚本。在 A test host 上的 Zabbix agent daemon 配置文件中，添加以下内容：

```
UserParameter=reimplementing.cpu.discovery,for cpu in $(ls -d /sys/devices/system/cpu/cpu[0-9]*/); do cpui=${cpu#/sys/devices/system/cpu/cpu}; [[ $(cat ${cpu}/online 2>/dev/null) == 1 || ! -f ${cpu}/online ]] && status=online || status=offline; cpulist=$cpulist,'{"{#CPU.NUMBER}":"'${cpui%/*}'","{#CPU.STATUS}":"'$status'"}' ; done; echo '{"data":['${cpulist#,}']}'
```

> 对于更复杂的情况或生产环境实现，请考虑使用适当的 JSON 实现，例如 JSON::XS Perl 模块。

重启 agent daemon，并在同一系统上运行以下命令：

```
$ zabbix_get -s 127.0.0.1 -k reimplementing.cpu.discovery
```

在四核系统上，它会返回类似如下内容：

```
{"data":[{"{#CPU.NUMBER}":"0","{#CPU.STATUS}":"online"},{"{#CPU.NUMBER}":"1","{#CPU.STATUS}":"online"},{"{#CPU.NUMBER}":"2","{#CPU.STATUS}":"offline"},{"{#CPU.NUMBER}":"3","{#CPU.STATUS}":"online"}]}
```

> 你可以使用 Perl 或 Python 重新格式化 JSON 以提高可读性——我们在本章前面已经做过。

我们现在可以像使用内置项一样以相同的方式将此项键用于 LLD 规则。项原型将完全以相同的方式工作，我们甚至不需要使用不同的 LLD 宏。

完成后，点击底部的 Add 按钮。对于此项，我们使用了 active agent，因为这对于用户参数是建议的，并且我们还将更新间隔设置为 5 分钟——通常数据库大小不会变化那么快，我们更感兴趣的是长期趋势。我们现在有了该项，它将是一个 UserParameter 变量，而该项又将由 Zabbix sender 填充的 LLD 规则创建。让我们现在设置 UserParameter 变量。在 A test host 的 Zabbix agent daemon 配置文件中，添加以下内容：

```
UserParameter=mysql.db.size[*],HOME=/home/zabbix mysql -Ne select sum(data_length+index_length) from information_schema.tables where table_schema='$1';
```

此 UserParameter 变量将查询总数据库大小，包括实际数据和所有索引。注意我们再次设置了 HOME 变量。保存文件后不要忘记重启 agent daemon。最好立即进行测试：

```
$ zabbix_get -s 127.0.0.1 -k mysql.db.size[zabbix]
```

这很可能会返回某个数字：

```
147865600
```

如果失败，请重新检查我们在第 11 章《高级项监控》中使用的 MySQL 参数配置。

![](img/f5313a804532fabacc8d07cc8607eeba_486_0.png)

> 注意返回此值需要一些时间。对于大型数据库，使用 Zabbix Sender 来处理此类项可能是一个更好的主意。

在 LLD 规则和项原型就位后，让我们开始发送用于发现的 JSON。以下内容应发现当前用户可访问的所有数据库并为 Zabbix 生成 LLD JSON：

```
for db in $(mysql -u zabbix -Ne show databases); do
  dblist=$dblist,'{"{#MYSQL.DBNAME}":"'$db'"}'
done
echo '{"data":['${dblist#,}']}'
```

![](img/f5313a804532fabacc8d07cc8607eeba_486_1.png)

> 我们正在删除 JSON 数据库列表中的尾随逗号——JSON 不允许尾随逗号，包含它会导致发现失败。Zabbix 会抱怨传入的数据不是有效的 JSON。

这里的原理与之前的 CPU 发现重新实现类似：我们找到所有数据库并在正确的宏名称后将它们列在 JSON 中。它应返回类似以下的行：

```
{"data":[{"{#MYSQL.DBNAME}":"information_schema"},{"{#MYSQL.DBNAME}":"zabbix"}]}
```

现在让我们将其实际发送到我们的 LLD 规则——我们将为此使用 Zabbix Sender。

如果你测试过此操作从而修改了 dblist 变量，请在运行以下命令前执行 `unset dblist`：

```
$ zabbix_sender -z 127.0.0.1 -s "A test host" -k mysql.db.discovery -o $(for db in $(mysql -u zabbix -Ne show databases); do dblist=$dblist,'{"{#MYSQL.DBNAME}":"'$db'"}'; done; echo '{"data":['${dblist#,}']}')
```

> 此命令应以 Zabbix agent daemon 运行的用户身份运行；否则，它可能会包含 Zabbix 用户无权限的数据库，并且此类项将变得不受支持。

在配置中访问 A test host 的项列表应显示为每个数据库创建了一个项：

| Item | Key |
| --- | --- |
| MySQL database discovery: Database information_schema size | `mysql.db.size[information_schema]` |
| MySQL database discovery: Database zabbix size | `mysql.db.size[zabbix]` |

首次值出现在 Latest data 页面可能需要长达 3 分钟——首先，最长一分钟用于配置缓存刷新，然后，最长两分钟用于 active agent 从服务器更新其配置。

> 还请记住，规则仅在收到新数据时才会被评估。如果数据库被删除并计划删除，只要 trapper 项不再接收数据，它就永远不会被删除。

点击最底部的 Add 按钮。

使用 `10` 且 Character string not included 将排除包含 `10` 的任何内容，而不仅仅是确切字符串 `10`。

但一旦在此处配置了类似的东西，我们如何在 LLD 规则过滤器中使用它？全局正则表达式可以通过在其名称前加上 at (@) 符号来代替普通正则表达式使用。为此，请转到 Configuration | Templates，点击 C_Template_Linux 旁边的 Discovery，然后点击 NAME 列中的 Interface discovery。切换到 Filters 选项卡，并将 REGULAR EXPRESSION 列中唯一的值替换为 `@Exclude loopback`。

此处不应使用引号——只需 at 符号然后是全局正则表达式名称，与在管理部分中配置的完全一致。

完成后，点击 Update。新的配置应完全相同工作，但似乎更容易理解。

当全局正则表达式的名称被更改时不会进行任何检查——这样一来，配置在其他地方可能会被破坏，因此应谨慎进行，如果可能的话根本不要进行。

全局正则表达式派上用场的另一个地方是日志监控。与 LLD 规则过滤器类似，我们只需使用 @-前缀的表达式名称，而不是直接输入 regexp。例如，我们可以定义如下正则表达式：

```
(ERROR|WARNING) 13[0-9]{3}
```

它会捕获任何错误和警告，其错误代码在 13,000 范围内——因为这可能被定义为我们关心的问题。假设我们将全局命名为 "regexp errors and warnings 13k"，日志监控键将如下所示：

```
log[/path/to/the/file,@regexp errors and warnings 13k]
```

## 测试全局 regexp

让我们返回到 **Administration | General**，在下拉菜单中选择 **Regular expressions**，并点击 **New regular expression**。按如下方式在此处添加三个表达式：

- 第一个表达式：
  - **EXPRESSION TYPE:** Character string included
  - **EXPRESSION:** `A`
  - **CASE SENSITIVE:** yes
- 第二个表达式：
  - **Expression type:** Result is TRUE
  - **Expression:** `^[0-9]`
- 第三个表达式：
  - **Expression type:** Result is FALSE
  - **Expression:** `[0-9]$`

这应匹配包含大写 A 的字符串，以数字开头，且不以数字结尾的字符串。现在，切换到 **Test** 选项卡并在 **Test string** 字段中输入 `1A2`；然后，点击 **Test expressions**。在以下结果区域的截图中，它显示以数字开头且包含大写 A 的字符串匹配，但随后字符串以数字结尾，而我们已将其否定。因此，最终测试失败。

![](img/f5313a804532fabacc8d07cc8607eeba_491_0.png)

