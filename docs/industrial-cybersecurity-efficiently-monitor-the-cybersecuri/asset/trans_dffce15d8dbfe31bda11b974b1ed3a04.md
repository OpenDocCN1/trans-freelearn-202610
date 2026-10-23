

8. 我们将添加启动 PowerShell 进程的用户名以显示在 PowerShell 调用事件条目中。为此，添加一个额外的 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `user.name.keyword`。
9. 将 **Size** 选项设置为 50 并点击 **Update**。
10. 这会生成一份完整的命令行日志列表。我们只会筛选出有趣的条目。点击 **Add Filter** 按钮并将 **Field** 选项设置为 `process.command_line`，**Operator** 选项设置为 `is one of`。
11. 现在，一次添加一个以下值，并在完成后点击 **Save**：
- `-noprofile`
- `-nop`
- `-encoded`
- `-en`
- `-enc`
- `-enco`
- `-encod`
- `-encode`
- `-ExecutionPolicy bypass`
- `-ep bypass`
- `-exp bypass`
- `-exec bypass`
- `-bypass`
- `-windowStyle hidden`

下图显示了配置好的 widget：

![](img/c8ffa9959721097701584fcc95101362_396_0.png)

Figure 9.57 – Exercise 6: Suspicious PowerShell invocation

12. 点击 **Save**，将 widget 保存为 `Breach Detection – Suspicious PowerShell Invocation`。

我们现在已经围绕可疑 PowerShell 命令以及 PowerShell 的调用方式创建了可视化。接下来，我们将查看可疑网络连接。

## Suspicious egress connections

看到对外部地址（企业区或互联网）的重复连接可能是网络上存在 **bot** 活动的迹象，或是从内部网络（工业区）外泄数据的迹象。Bot 是一种将它们感染的主机转变为 **zombie** 机器的恶意软件，即受制于主人的无意识无人机，为该主人执行操作。Bot 需要定期与其 C&C 服务器建立连接以向 botmaster 汇报，请求指令或更新。我们可以通过监控内部 IP 地址与外部 IP 地址之间的重复连接来检测这些汇报。通常，这些连接的类型和大小相同，并且都指向同一个 IP 地址。我们接下来要做的是添加一个可视化，以便在汇总表中快速描绘这种典型的连接类型。要添加一个汇总出站已建立连接计数的表格，请按照以下说明操作：

1. 导航到我们的 **Breach Detection** 仪表板（**Kibana | Dashboards | Breach Detection**）并在仪表板屏幕左上角点击 **Edit**。
2. 点击 **Create New** 按钮以添加新的 widget（可视化）。
3. 选择 `Data Table` 可视化类型并搜索并添加 `Security Onion – Connections` 数据源。
4. 添加一个 `Split Rows` 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `source.ip`。将 **Size** 选项设置为 50。
5. 添加一个 `Split Rows` 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `destination.ip`。将 **Size** 选项设置为 50。
6. 添加一个 `Split Rows` 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `destination.port`。将 **Size** 选项设置为 50。
7. 添加一个 `Split Rows` 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `network.transport.keyword`。将 **Size** 选项设置为 10。
8. 添加一个 `Split Rows` 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `network.bytes`。将 **Size** 选项设置为 50。
9. 添加一个 `Split Rows` 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `connection.state_description.keyword`。将 **Size** 选项设置为 50。

## Suspicious ingress connections

另一个基于网络连接的良好指标是入站连接检测。尤其是在工业环境中，对 ICS 网络上资产的外部连接都是可疑的，如果它们源自互联网则更可疑。我们现在将创建一个可视化，用于显示入站网络连接汇总。按照以下说明创建汇总外部连接进入工业网络的表格：

1. 导航到我们的 **Breach Detection** 仪表板（**Kibana | Dashboards | Breach Detection**）并在仪表板屏幕左上角点击 **Edit**。
2. 点击 **Create New** 按钮以添加新的 widget（可视化）。
3. 选择 Data Table 可视化类型并搜索并添加 Security Onion - Connections 数据源。
4. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 @timestamp。 将 **Size** 选项设置为 50。
5. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 source.ip。 将 **Size** 选项设置为 50。
6. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 destination.ip。 将 **Size** 选项设置为 50。
7. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 destination.port。 将 **Size** 选项设置为 50。
8. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 network.transport.keyword。 将 **Size** 选项设置为 50。
9. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 event.duration。 将 **Size** 选项设置为 50。
10. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 network.bytes。 将 **Size** 选项设置为 50。
11. 添加一个 Split Rows 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 connection.state_description.keyword。 将 **Size** 选项设置为 10。
12. 这会生成所有连接日志的汇总视图。我们希望查看已建立的连接，但仅限出站。为此，添加以下过滤器：
- 输入以下搜索字符串以进行视图过滤：NOT source.ip: 172.25.100.0/24。这将过滤掉任何本地于工业网络的 IP 地址（根据你的子网进行调整）。
- 在 connection.local.responder 上对数据表进行过滤：true——这将过滤视图，仅显示连接到本地目标（工业区）的连接。

13. 鉴于我们处理的是工业网络，在我们刚刚创建的数据表视图中，我们应该看到很少或没有外部连接。事实上，在这里看到连接出现是极其可疑的。如果你有企业系统出于正当理由需要连接到工业资产，你可以通过过滤它们将其加入白名单。你可以在以下截图中看到示例输出：

![](img/c8ffa9959721097701584fcc95101362_400_0.png)

Figure 9.59 – Exercise 6: Suspicious ingress connections

14. 确保按 **Duration**（降序）排序并点击 **Save**，将 widget 保存为 **Breach Detection – Suspicious Ingress Connections**。

接下来，我们将查看一些用户账户活动。

## Failed user login attempts

失败的用户登录尝试时有发生。人们会输错密码、忘记密码，或因某种原因被锁定。这是对任何网络的正常活动；然而，失败登录尝试中的某些模式会脱颖而出。从或到某个 IP 地址（主机）的过度登录尝试几乎可以肯定是恶意的。由于许多网络管理员为过度登录尝试实施了账户锁定程序，攻击者已改变其方法，只在每台机器上尝试 X 段时间，然后转到下一台机器。对于这种情况，来自单台机器对多台其他机器的几次失败登录尝试也应被视为可疑。

为了捕获这两种类型的登录攻击（暴力攻击），我们将创建一个可视化，用于随时间推移展示失败登录尝试。按照以下说明创建图表 widget：

1. 导航到我们的 **Breach Detection** 仪表板（**Kibana | Dashboards | Breach Detection**）并在仪表板屏幕左上角点击 **Edit**。
2. 点击 **Create New** 按钮以添加新的 widget（可视化）。
3. 选择 Vertical Bar 可视化类型并搜索并添加 Security Onion - Alerts 数据源。
4. 添加一个 Split Series 数据桶并将 **Aggregation** 设置为 **Terms**，**Field** 选择为 `winlog.event_data.targetUserName.keyword`。将 **Size** 选项设置为 50。
5. 添加一个 X-axis 数据桶并将 **Aggregation** 设置为 **Data Histogram**，然后点击 **Update**。
6. 这将聚合设置为时间线显示，随时间查看所有警报的目标用户名日志。
7. 现在是关键时刻：点击 **Add Filter** 按钮，然后将 **Field** 选项设置为 `event.code`，**Operator** 选项设置为 `is one of`。将值设置为 4625、4771 和 539，这三个众所周知的 Windows 事件 ID 表示失败的登录尝试。以下截图显示了示例输出：

![](img/c8ffa9959721097701584fcc95101362_401_0.png)

Figure 9.60 – Exercise 6: Failed login attempts

