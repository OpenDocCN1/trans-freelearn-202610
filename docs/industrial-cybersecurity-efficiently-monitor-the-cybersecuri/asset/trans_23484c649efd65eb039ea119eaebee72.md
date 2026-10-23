

```
```
urls:
sids:
  enabled:
  disabled:
    - 2013505
modify:
```
```

保存文件，然后运行 `sudo salt ind-securityonyonv2_eval state.apply idstools` Salt 更新命令。包含 SID 2013505 的 Suricata 告警将不再被记录。

顺便提一下，如果你从 https://snort.org/ 获得了 Snort oinkcode，从而可以从他们的网站下载社区规则，那么 idstools 部分就是你要输入该代码的地方，这样 Security Onion 就会连同 Emerging Threats（ET）规则一起拉取 Snort 规则。

调整 IDS 规则是构建一个可用的安全监控环境的关键部分。分析人员为了找到真正的威胁而必须筛选的良性或虚假告警越多，他们就越有可能漏掉真正的告警。任何基于签名的工具部署，其大部分工作都在于规则的微调。

我们将向入侵检测仪表板添加一个 NIDS（Suricata）告警数据表可视化。请按照以下说明操作：

+   1. 在 **Editing New Dashboard** 页面上点击 **Create new object to this dashboard** 按钮。
+   2. 在弹出的 **New Visualization selection** 屏幕中选择 **Data Table**。
+   3. 在接下来的 **New Data Table / Choose a source** 屏幕中搜索 Alerts，并选择 **Security Onion – Alerts** 数据源，如下面的截图所示：

![](img/c8ffa9959721097701584fcc95101362_381_0.png)

图 9.45 – 练习 6：添加 Snort 告警数据表

4. 我们会进入可视化编辑页面。在这里，我们定义数据显示（小部件）的内容以及显示方式。数据按桶（bucket）显示；我们需要添加一个数据桶来查看 NIDS 告警消息。点击配置屏幕右侧 Data 面板下的 ADD BUCKET 按钮，然后选择 Split rows，如下面的截图所示：

![](img/c8ffa9959721097701584fcc95101362_382_0.png)

图 9.46 – 练习 6：Snort 告警数据表 – 添加数据桶

5. 在 Split rows 面板下，从 Aggregation 下拉菜单中选择 Terms，对于 Field（我们希望从中获取数据的数据库字段），搜索并选择 rule.name.keyword。最后，将 Size 设置改为 50，以显示最常出现的 50 条告警。

下面的截图显示了具体操作方式：

![](img/c8ffa9959721097701584fcc95101362_383_0.png)

图 9.47 – 练习 6：配置 Snort 告警数据表数据桶

6. 当你点击 **Update** 时，数据表会在屏幕左侧填充，显示我们刚刚配置的数据的结果，如下面的截图所示：

![](img/c8ffa9959721097701584fcc95101362_384_0.png)

图 9.48 – 练习 6：Snort 告警小部件

如果 Snort 引擎调整得当，这个视图应该只显示很少的输出。我的视图显示了一个很大的列表，是因为我展示的是过去 7 天的数据，而且我做了一些网络扫描和探测，以便让这些数据显示出来作为一张漂亮的图片。

我们的目标是创建一个仪表板，显示能够指示正在发生的攻击的数据——换句话说，就是能够简洁而准确地告诉我们正遭受攻击的数据。如果这个页面上的任何小部件出现一大堆误报，就无法给我们提供这种明确的告警总览。

7. 为了削减一些嘈杂的告警，当鼠标悬停在你要抑制的告警上时，点击 rule.name 旁边的减号（-）符号（过滤掉）。你可以稍后返回（Edit visualization）重新添加任何烦人的嘈杂告警。

8. 此外，我们可以按事件严重性进行过滤。为此，点击编辑器屏幕顶部的 Add Filter 按钮，将 Field 设置为 event.severity。然后，将 Operator 设置为 is between，并在底部和顶部值处分别输入 3 和 8，如下面的截图所示：

![](img/c8ffa9959721097701584fcc95101362_385_0.png)

图 9.49 – 按严重性过滤

点击 Save 以应用该过滤器。

9. 至此，我们刚刚创建了一个数据表，用于显示我们在指定时间范围（7 天）内看到的 NIDS 告警摘要。点击 Save，将此可视化命名为 Breach Detection - NIDS Alerts Summary，然后点击 Save and return。这会保存该可视化并将其添加到我们的新仪表板中。

10. 在仪表板的 Edit Mode 下，我们可以根据自己的喜好调整可视化的大小和位置。

11. 使用编辑器屏幕左上角的 Save 按钮保存仪表板，并将我们的新作品命名为 Breach Detection Portal。

我们刚刚创建了最初的入侵检测仪表板，目前它只显示 NIDS 告警，但我们很快会在这里添加更多数据，接下来是 Zeek notices。

## Zeek notices

Zeek notices 日志是 Zeek 针对有趣且紧迫的发现向我们发出告警的方式。一个示例通知是发现与已定义威胁情报相关联的日志。这是一个重要发现，需要发出告警。

我们可以按照以下说明添加 notices：

+   1. 导航到我们的 **Breach Detection** 仪表板（**Kibana | Dashboards | Breach Detection**），然后点击仪表板屏幕左上角的 **Edit**。
+   2. 点击 **Create New** 按钮以添加一个新的小部件（可视化）。
+   3. 选择 `Data Table` 可视化类型，并搜索并添加 `*:so-*` 数据源（这是所有日志的数据源）。
+   4. 输入 `event.dataset:notice AND event.module: zeek` 搜索词以仅过滤出 notices 日志。点击 **Refresh**。
+   5. 添加一个 `Split Rows` 数据桶，并将 **Sub aggregation** 设置为 **Terms**，将 `notice.message.keyword` 作为 **Field** 选择。
+   6. 将 **Size** 选项设置为 50，然后点击 **Update**。
+   7. 我们将添加源 IP 地址以与 notice 消息一起显示。为此，添加一个额外的 `Split Rows` 数据桶，并将 **Sub aggregation** 设置为 **Terms**，将 `source.ip` 作为 **Field** 选择。
+   8. 将 **Size** 选项设置为 50，然后点击 **Update**。

下面的截图显示了应如何操作：

![](img/c8ffa9959721097701584fcc95101362_387_0.png)

图 9.50 – 练习 6：Zeek notices – 创建可视化

默认情况下，Zeek 将自签名证书标记为无效并为其创建一条 notice。尽管这对 IT 环境来说是一个重要发现，但在 OT 方面，我们不太关心这一发现。典型工业环境的屏蔽架构使得公钥基础设施（PKI）的管理难以维护，因此证书错误并不罕见。在我们的 Breach Detection 仪表板视图中忽略这些证书错误是可以的。我们不会将它们从数据库中删除，只是将它们从视图中隐藏。

9. 要从视图中排除 SSL 证书验证失败（无法获取本地颁发者证书）的 notices，请将鼠标悬停在 notice 计数（14）左侧的区域上，并点击弹出的 –（减号）符号 Filter out value。点击此按钮。或者，你可以点击屏幕顶部的 Add filter 按钮。在 Add filter 弹出窗口中，在 Field 中键入 notice.message.keyword，将运算符设置为 is not，并在 Value 下拉列表中选择 SSL certificate validation failed with (unable to get local issuer certificate)。点击 Save。下面的截图说明了这一过程：

![](img/c8ffa9959721097701584fcc95101362_388_0.png)

图 9.51 – 练习 6：Zeek notices – 添加数据过滤器

10. 以同样的方式，过滤掉任何其他安全套接字层（SSL）证书通知或使视图混乱的通知。

11. 结果是一个 Zeek notice 日志视图，多余的告警极少，简洁明了。点击 Save，将小部件保存为 Breach Detection - Zeek Notices Summary。

接下来，我们将向 Breach Detection 仪表板添加 Zeek intel 日志。

## Zeek Intel 日志

当 Zeek 将发现的工件与 Intel 文件中的相应条目相关联时，就会创建 Zeek Intel 日志。在*第 8 章，工业威胁情报*中，我们将 AlienVault 威胁信息源添加到了 Security Onion。下面看一下该信息源文件（`intel.dat`）中的一些条目：

![](img/c8ffa9959721097701584fcc95101362_389_0.png)

我们可以看到Intel文件中有一个条目将 `Intel::DOMAIN) w0x.host` 域名与 `Luhansk Ukraine Gov. Phishing Campaig`n 相关联。现在，当 Zeek 在监控网络流量时遇到 `w0x.host` 域名，它就会创建一条 Intel 日志条目，如下所示：

![](img/c8ffa9959721097701584fcc95101362_389_1.png)

