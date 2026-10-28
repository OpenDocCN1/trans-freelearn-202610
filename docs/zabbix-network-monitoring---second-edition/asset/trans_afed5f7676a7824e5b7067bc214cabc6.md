
> 要将数值形式转换为文本形式，需要 MIB。在某些情况下，标准 MIB 就够了，但许多设备在厂商特定的扩展中包含有用的信息。一些厂商为其设备提供了高质量的 MIB，另一些则帮助不大。请联系厂商获取所需的任何 MIB。我们将在本章稍后讨论基本的 MIB 管理。

那是一个相当长的名称，展示了树状结构。它从一个无名的根对象开始，一直向下延伸，所有值都附着在这棵树的某个位置。嗯，我们提到了数值形式，我们也可以用 -On 标志让 snmpget 输出数值形式的名称：

```bash
$ snmpget -v 2c -c public -On 10.1.1.100 DISMAN-EVENT-MIB::sysUpTimeInstance
.1.3.6.1.2.1.1.3.0 = Timeticks: (9048942) 1 day, 1:08:09.42
```

因此，每个 OID 可以用三种表示法之一来引用：短名称、长名称或数字名称。在本例中，DISMAN-EVENT-MIB::sysUpTimeInstance、.iso.org.dod.internet.mgmt.mib-2.system.sysUpTime.sysUpTimeInstance 和 .1.3.6.1.2.1.1.3.0 都指向同一个值。

> 有关其他受支持的输出格式选项，请参阅 snmpcmd 手册页。

但这如何适用于 Zabbix SNMP 监控项呢？在 Zabbix 中创建 SNMP 监控项时，你必须输入一个 OID。你怎么知道该用哪个 OID？通常，你会有以下几种选择：

- 直接知道
- 询问别人
- 自己找出来

通常情况下，前两个选项行不通，所以自己找出来将是唯一的方法。正如我们已经了解的，Net-SNMP 工具非常擅长支持这种发现过程。

## 将 SNMPv3 与 Net-SNMP 结合使用

SNMP 的最新版本，第 3 版，目前还不太常见，而且比以前的版本更复杂一些。设备实现的质量也参差不齐，因此针对已知解决方案（Net-SNMP 守护进程）测试你的 Zabbix 配置可能会很有用。让我们向其中添加一个 SNMPv3 用户并获取一个值。确保 Net-SNMP 已安装，并且 snmpd 能成功启动。

要配置 SNMPv3，首先停止 snmpd，然后以 root 身份运行以下命令：

```
# net-snmp-create-v3-user -ro zabbix
```

此工具会提示输入密码。请输入至少八个字符的密码——虽然此处会接受较短的密码，但稍后会因默认长度要求而失败。再次启动 snmpd，并使用第 3 版测试值检索：

```
$ snmpget -u zabbix -A zabbixzabbix -v 3 -l authNoPriv localhost SNMPv2-MIB::sysDescr.0
```

这应该会成功返回数据，如下所示：

```
SNMPv2-MIB::sysDescr.0 = STRING: Linux another 3.11.10-29-default #1 SMP Thu Mar 5 16:24:00 UTC 2015 (338c513) x86_64
```

我们不需要分别配置版本 1 或 2c，所以现在我们有了一个通用的 SNMP 代理，提供所有常见版本用于测试或探索。

## 引擎 ID

尝试使用 SNMPv3 时，有一个非常常见的错误配置。根据 RFC 3414 (https://tools.ietf.org/html/rfc3414)，每个设备都必须有一个唯一标识符。每个 SNMP 引擎维护一个值 snmpEngineID，用于唯一标识该 SNMP 引擎。

有时，用户会为多个设备设置相同的 ID。结果，Zabbix 无法成功监控这些设备。更糟糕的是，每个设备对 snmpget 或 snmpwalk 等命令都响应正常。这些命令一次只与一个设备通信，因此它们不太关心 snmpEngineID。

在 Zabbix 中，这可能表现为一个设备正常工作，但当另一个设备加入监控时，它便停止工作。

如果 Zabbix 监控 SNMPv3 设备时出现神秘问题，而这些问题在使用命令行工具时不会出现，那么务必仔细检查 snmpEngineID。

> 获取 SNMP MIB 并不总是那么容易。某大型打印机厂商代表声称，他们不提供 SNMP MIB，所有人都应使用其专有的打印机管理应用程序。但大多数厂商确实提供 MIB，而且在某些情况下，可自由访问的 MIB 收集网站可能比官方厂商网站更有帮助。

下载新的 MIB 后，你必须将其放在 Net-SNMP 搜索 MIB 文件的位置，并同时进行配置。Net-SNMP 在两个位置搜索 MIB：用户主目录中的 .snmp/mibs 和 /usr/share/snmp/mibs；使用哪个由你决定。如果只需要当前用户使用，或者没有 /usr 目录的访问权限，可以使用 .snmp/mibs；否则，使用 /usr/share/snmp/mibs。无论选择哪个，这还不够——你还必须指示工具包含此 MIB。

> 虽然 Zabbix 服务器使用同一目录查找 MIB，但指定要使用的 MIB 仅对 Net-SNMP 工具是必需的——Zabbix 服务器会加载找到的所有 MIB。

第一种方法是直接将 MIB 名称传递给被调用的命令。但是，我们还不知道 MIB 名称。要找出某个文件中的特定名称，请用文本编辑器打开该文件，并在文件开头附近查找 MIB DEFINITIONS ::= BEGIN。该文本之前的字符串就是我们要找的 MIB 名称。示例如下：

```
PowerNet-MIB DEFINITIONS ::= BEGIN
```

因此，APC 选择将其 MIB 命名为 PowerNet-MIB。有了这个信息，我们可以指示任何命令包含此文件：

```
$ snmpget -m +PowerNet-MIB -v 1 -c public <IP address> SNMPv2-SMI::enterprises.318.1.1.1.1.2.3.0
PowerNet-MIB::upsAdvIdentSerialNumber.0 = STRING: "QS0547120198"
```

很好；snmpget 包含了正确的 MIB 并获得了完整的文本字符串，这证实了我们的猜测：这可能是一个序列号。现在，你可以对 snmpwalk 使用同样的标志，并获得一个具有更好值名称的文件。很多时候，你可以在这样的文件中搜索诸如序列号等感兴趣的字符串，并找到正确的 OID。

> 加号表示在已配置的 MIB 之外，还包含指定的 MIB。如果省略加号，MIB 列表将被替换为你指定的列表。

请随意查看 /usr/share/snmp/mibs 目录中的 MIB 文件。如你所见，这里的大多数文件名与其 MIB 名称相同，只是没有扩展名，不过这并非必需。实际上，文件名与 MIB 名称毫无关系；因此，有时你可能需要借助 grep 等工具来找出哪个文件包含哪个 MIB。

虽然在命令行中传递单个 MIB 名称很适合快速的一次性查询，但当你要更频繁地执行这些操作且 MIB 列表增长时，这会变得非常繁琐。还有另一种方法，更持久一些——MIB 的环境变量。在这种情况下，可以这样设置变量：

```
$ export MIBS=+PowerNet-MIB
```

在当前 shell 中，单个命令不再需要传递 MIB 名称。变量中指定的所有 MIB 都将在每次调用时被包含。

当然，这也不是永久的。虽然你可以在 profile 脚本中指定此变量，但为机器上的所有用户管理起来会很繁琐。这时就轮到第三种方法了：配置文件。

同样，你可以使用位于用户主目录中的 .snmp/snmp.conf 下的每用户配置文件，也可以使用全局的 /etc/snmp/snmp.conf 文件。

> 如果你从源码编译了 Net-SNMP，全局配置文件和 MIB 目录的位置可能不同。它们可能位于 /usr/local。

添加 MIB 的语法与环境变量中使用的语法类似——只需在每行前面加上 mibs，如下所示：

```
mibs +PowerNet-MIB
```

如果你想在上述任一位置指定多个 MIB 名称，必须用冒号分隔它们。假设你还需要一个通用的 UPS MIB；在这种情况下，MIB 名称字符串将如下所示：

```
+PowerNet-MIB:UPS-MIB
```

> 在某些 Net-SNMP 版本中，配置文件中的行可能会在 1024 个字符（包括换行符）处被静默截断。你可以指定多个 mibs 行来绕过此限制。

