

> Zabbix 前端使用 PCRE，但 Zabbix 守护进程使用 POSIX EXTENDED。不要使用 PCRE 字符类、环视或其他 PCRE 支持但 POSIX ERE 不支持的任何特性——它们在前端测试时看似能工作，但在由 Zabbix 守护进程解释时就会失败。

## 默认模板中的用法

既然我们创建了自己的全局正则表达式，你可能已经注意到其中已有一些现成的。让我们导航到 Administration | General 并再次在下拉菜单中选择 Regular expressions。除了我们为回环接口过滤创建的那个之外，还有三个现有的表达式：

| File systems for discovery | 1 » ^(btrfs|ext2|ext3|ext4|jfs|reiserfs|xfs|jffs|ufs|jffs2|qfs|hfs|nfs|fat32|zfs)$ | [Result is TRUE] |
| Network interfaces for discovery | 1 » ^lo$ | [Result is FALSE] |
|  | 2 » ^Software Loopback Interface | [Result is FALSE] |
| Storage devices for SNMP discovery | 1 » ^(Physical memory|Virtual memory|Memory buffers|Cached memory|Swap space)$ | [Result is FALSE] |

其中一个，Network interfaces for discovery，实际上做的事情几乎与我们做的相同，只是它还排除了名称以 Software Loopback Interface 开头的接口——这是针对 MS Windows 监控的。File systems for discovery 可以用来限制要监控的文件系统类型——除了我们过滤的 ext3 之外，它还包含一大堆其他文件系统类型。Storage devices for SNMP discovery 用于在通过 SNMP 监控时将内存统计信息从存储设备中排除。虽然文件系统类型正则表达式可以直接输入，但其他两个几乎不可能——POSIX EXTENDED 实际上并没有以合理的方式支持否定多个字符串。

## 网络发现

LLD 关注的是在单个主机上发现实体。Zabbix 还支持一种扫描网络地址范围并根据发现的内容执行某些操作的方式——这称为网络发现。

## 查看结果

等待几分钟后，检查 **Monitoring | Discovery** 部分：

| DISCOVERED DEVICE | MONITORED HOST | UPTIME/DOWNTIME | ICMP PING |
|---|---|---|---|
| **A test discovery** (2 devices) |  |  |  |
| 192.168.56.10 (testhost) |  | 00:23:22 | 🟩 |
| 192.168.56.11 | Another host | 00:23:20 | 🟩 |

所有在配置范围内响应 ping 的设备都将在这里列出。如果某个设备已作为 Zabbix 中的主机被监控，它将在 **MONITORED HOST** 列中列出。我们还将看到该主机已知处于活动状态的时间长短，以及 **ICMP PING** 列将为所有主机以绿色列出此特定服务。但为什么这里只有一台主机被列为已监控？主机在这里是通过其 IP 地址识别的，而对于 A test host，我们使用了 `127.0.0.1`。发现时使用的地址不同，因此它并不被视为同一主机或设备。

> 主机目前在此处不可点击——可能最简单的方法是通过全局搜索字段复制并粘贴主机名来访问主机属性。

现在，导航回 **Configuration | Discovery** 并在 **NAME** 列中点击 **A test discovery**。在 **Checks** 块中点击 **New** 并选择一个在这些主机上可访问且易于控制的服务——也许再次选择 **SMTP**。在 **Checks** 部分点击 **Add**，然后再次点击 **New**。这次，选择一个在配置范围内任何主机上都不存在的服务——**FTP** 可能是不错的选择。然后，再次在该块中点击 **Add**：

![](img/f5313a804532fabacc8d07cc8607eeba_494_0.png)

最后，点击 **Update**。等待几分钟后，访问 **Monitoring | Discovery**：

| DISCOVERED DEVICE ▲ | MONITORED HOST | UPTIME/DOWNTIME | ICMP PING | SMTP |
|---|---|---|---|---|
| A test discovery (2 devices) |  |  |  |  |
| 192.168.56.10 (testhost) |  | 02:07:13 |  |  |
| 192.168.56.11 | Another host | 02:07:11 |  |  |

**SMTP** 已出现，这很棒。但为什么没有 **FTP** 列？这个视图是否限制为两个服务？它并不限制服务数量，但如果某个服务在任何主机上都未被发现，则目前根本不会显示该服务。如果某个服务最初在某些系统上被发现而在其他系统上未被发现，则会显示该列，未发现该服务的系统将得到灰色单元格。

如果我们将光标移到绿色单元格上，我们将能够看到该服务已处于活动状态（或被发现）的时间长短：

![](img/f5313a804532fabacc8d07cc8607eeba_495_0.png)

让我们破坏一下——在其中一台主机上关闭 SMTP 服务，并等待几分钟。该主机对应的 **SMTP** 单元格应变为红色，并且弹出窗口应开始跟踪该服务的停机时间。如果主机上的所有服务都宕机，则该主机本身将被视为宕机，这将在 **UPTIME/DOWNTIME** 列中反映出来。

## 对发现结果做出反应

发现监控页面起初很有趣，但长期来看并不太有用。幸运的是，我们可以让 Zabbix 根据发现结果执行操作，其配置方式与我们对触发器触发做出反应时的方式有些相似。要查看此配置方式，请导航到 Configuration | Actions，并在右上角的 Event source 下拉菜单中切换到 Discovery。然后，点击 Create action。立刻就能注意到的一点是，此操作仍填入了默认的主题和消息，但内容不同：这里使用的宏特定于网络发现。填写 Network discovery test 的名称，然后让我们切换到 Conditions 选项卡并展开 New condition 部分中的第一个下拉菜单：

![](img/f5313a804532fabacc8d07cc8607eeba_496_0.png)

可用的条件与触发器操作中可用的条件完全不同。让我们回顾一下它们：

- **Discovery check**：这里必须选择特定发现规则中的特定检查。
- **Discovery object**：这里可以选择设备或服务。在我们的示例中，被发现的主机是设备对象，而 SMTP 是服务对象。
- **Discovery rule**：必须选择特定的网络发现规则。
- **Discovery status**：此条件可能的取值为 **Up**、**Down**、**Discovered** 和 **Lost**。对于设备，如果其上的至少一个服务可以到达，则被认为已发现或处于活动状态。以下是这些值的含义：
  - **Discovered**：此设备或服务首次被看到或在检测到宕机后再次被检测到
  - **Lost**：此设备或服务以前被看到过，但刚刚消失
- **Up:** 设备或服务已被发现——无论发生过多少次
- **Down:** 设备或服务曾在某个时间点被发现，但现在无法到达——无论发生过多少次
- **Host IP:** 这里可以指定单个地址或范围。
- **Proxy:** 操作可以限制为特定的 Zabbix 代理。我们将在 *第 19 章，使用代理监控远程位置* 中讨论代理。
- **Received value:** 如果我们轮询的是 Zabbix agent 项目或 SNMP OID，我们可以对特定值做出反应——例如，如果通过 `system.uname` 项目键发现，我们可以将返回字符串中包含 `Linux` 的所有主机链接到 Linux 模板。
- **Service port:** 操作可以限制为发现时发生的特定端口或端口范围。
- **Service type:** 操作可以限制为服务类型。这与 **Discovery check** 条件类似，只是选择 SMTP 将匹配所有网络发现规则中的所有 SMTP 检查，而不仅仅是特定的一个。
- **Uptime/Downtime:** 这里可以输入秒数，以将操作仅限制在设备或服务已处于活动或宕机状态一段时间之后。

其中大多数都很容易理解，但让我们更仔细地看看其中两个。**Discovery status** 条件允许我们在初始检查或在停机后被发现与周期性检查之间进行区分。例如，如果我们匹配 **Up** 状态并将主机添加到 **Host group**，此添加将在每次主机可到达时被检查并执行。如果有人将该主机从该主机组中移除，它将在每次发现周期中被重新添加。如果我们匹配 **Discovered** 状态，它只会在主机首次被发现以及主机宕机后再次上线时发生。在这种情况下，自动重新添加到组中很可能发生在更晚的时候。

**Uptime/Downtime** 条件允许我们在有延迟的情况下做出反应，而不是立即反应。例如，我们可能希望在设备上运行几小时的监控，因为它可能是一台临时用于故障排除的笔记本电脑，已连接到网络。可能更重要的是，我们可能不希望在主机停机 5 分钟时删除该主机及其所有历史记录。检查一周的停机时间可能是合理的——如果一周内没人关心该主机，则可以安全删除。

