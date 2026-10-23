

输出可在以下屏幕截图中看到：

```
[adm-pac@IND-SecurityOnionv2 ~]$ sudo so-allow
[sudo] password for adm-pac:
Sorry, try again.
[sudo] password for adm-pac:
This program allows you to add a firewall rule to allow connections from a new IP address.
Choose the role for the IP or Range you would like to add

[a] - Analyst - ports 80/tcp and 443/tcp
[b] - Logstash Beat - port 5044/tcp
[e] - Elasticsearch REST API - port 9200/tcp
[f] - Strelka frontend - port 57314/tcp
[g] - Osquery endpoint - port 8090/tcp
[s] - Syslog device - 514/tcp/udp
[w] - Wazuh agent - port 1514/tcp/udp
[p] - Wazuh API - port 55000/tcp
[r] - Wazuh registration service - 1515/tcp

Please enter your selection:
s
Enter a single ip address or range to allow (example: 10.10.10.10 or 10.10.0.0/16):
172.25.100.240
Adding 172.25.100.240 to the syslog role. This can take a few seconds
local:
------------
        ID: create_sysconfig_iptables
      Function: file.touch
        Name: /etc/sysconfig/iptables
      Result: True
    Comment: unless condition is true
    Started: 22:52:29.178700
    Duration: 2458.279 ms
     Changes:
------------
        ID: iptables_fix_docker
      Function: iptables.chain_present
        Name: DOCKER-USER
      Result: True
```

图 9.24 – 练习 4：Security Onion – so-allow syslog

这就是允许 SilentDefense Command Center 访问 syslog 所需的所有配置。

+   2. 我们现在需要登录 SilentDefense Command Center 网页门户（https://172.25.100.240/login）来配置 SilentDefense 端的相关设置。
+   3. 从门户仪表盘页面，点击顶部菜单栏中的 Settings。

**Host name**—将其设置为 `SilentDefense-001`。

**Application name**—将其设置为 `AlertLogs`。

以下屏幕截图显示了应如何操作：

![](img/c8ffa9959721097701584fcc95101362_364_0.png)

e) 这就是警报日志所需的所有配置。点击屏幕顶部的 **Finish**。

+   5. 点击 **Back** 返回到 Command Center **Settings** 页面。

+   6. 点击 **Network logs forwarding** 并按照以下步骤将 Syslog 转发添加到网络日志中：

+   a) 从 **Network logs forwarding** 配置页面，点击 **Add Instance** 按钮（屏幕右侧的加号 (+) 图标）。

+   b) 这将打开一个空白的 syslog 转发配置屏幕。指定一个服务器名称（`SecurityOnion-Network`）并切换到 **Connectivity** 选项卡。

+   c) 在 **Connectivity** 选项卡上，将远程主机 IP 地址设置为 `172.25.100.250`，即 Security Onion 服务器的 IP 地址。

d) 切换到 **Header** 选项卡并设置以下内容：

**Severity**—将其设置为 **DEBUG**（在验证工作正常后降至 **WARNING**）。

**Host name**—将其设置为 `SilentDefense-001`。

**Application name**—将其设置为 `NetworkLogs`。

e) 这就是网络日志所需的所有配置。点击屏幕顶部的 **Finish**。

这就是我们需要完成的所有配置，以便将 syslog 消息从 SilentDefense Command Center 发送到 Security Onion 服务器。我们可以通过打开 Kibana 网页门户，导航到 **Discovery** 页面，并搜索 `message` : `NetworkLogs` 来验证是否正常工作，如以下屏幕截图所示：

![](img/c8ffa9959721097701584fcc95101362_365_0.png)

我们已经收到 syslog 数据，但它处于原始格式，意味着它现在尚未解析为可用数据。这是我们将使用 Elasticsearch 解析来执行的操作。诀窍是解析传入的 syslog 数据并在将其存储到数据库之前修复数据。请按照以下说明操作：

+   7. Elasticsearch 使用 ingest 解析器指令文件来接收来自发送者的日志并以用户指定的形状和形式存储数据。我们可以在 /opt/so/conf/elasticsearch/ingest/ 文件夹中看到现有的指令文件，如以下屏幕截图所示：

![](img/c8ffa9959721097701584fcc95101362_366_0.png)

图 9.29 – 练习 4：Security Onion – Elasticsearch ingest 解析器指令文件

我们可以通过在 /opt/so/saltstack/local/salt/elasticsearch/files/ingest/ 中为它们创建 ingest 解析指令文件来添加自己的自定义指令。Elasticsearch 会在（重新）启动时包含自定义文件夹中的指令。我们将在此处添加用于解析 SilentDefense Syslog 消息的配置文件。

+   8. 下载 syslog 和 silentdefense 配置文件，如下所示：

+   a) 第一个要下载的文件是 syslog Elasticsearch ingest 解析配置文件，来自 https://github.com/SackOfHacks/Industrial-Cybersecurity-2nd-Edition/blob/main/security-onion-logstash/syslog。这将覆盖 Security Onion 的 syslog 解析，以添加以下 grok 解析器指令：

```
"<111>1 %{TIMESTAMP_ISO8601:sd.timestamp} %{SYSLOGHOST:sd.hostname} %{WORD:sd.logname} - - - CEF:0\|%{DATA:sd.vendor}\\\\|%{DATA:sd.product}\\\\|%{DATA:sd.product_version}\\\\|%{DATA:sd.alert_name}\\\\|%{DATA:sd.alert_title}\\\\|%{DATA:sd.alert_number}\\\\|%{GREEDYDATA:sd.real_message}"
```

grok 是一个正则表达式解析命令，允许 Logstash 查找和重塑 syslog 消息等非结构化文本。我们的指令文件中的 grok 表达式告诉 Elasticsearch 执行以下操作：

+   - 查找 TIMESTAMP_ISO8601 格式的时间戳并以 sd.timestamp 的名称存储它。
+   - 将消息的下一部分视为 SYSLOGHOST 并以 sd.hostname 的名称存储它。
+   - 接下来，查找以下信息（DATA）：
+     - sd.vendor
+     - sd.product
+     - sd.product_version
+     - sd.alert_name
+     - ...
+   - 最后，将剩余的事件数据（IP地址和详细消息数据）存储在 sd.real_message 中，该数据将通过 silentdefense ingest 解析器进一步解析。

grok 模式定义与正则表达式相关，可以在此处找到：https://github.com/elastic/logstash/blob/v1.4.2/patterns/grok-patterns。

b) 我们需要下载的第二个文件是 silentdefense Elasticsearch ingest 解析配置文件，来自 https://github.com/SackOfHacks/Industrial-Cybersecurity-2nd-Edition/blob/main/security-onion-logstash/silentdefense。此 ingest 解析器将 SilentDefense Command Center 发送到 Security Onion 的 syslog 事件的 real_message 部分拆分为相关信息，如下所示：

```
"dissect": {
    "if": "ctx.sd.logname == 'NetworkLogs'",
    "field": "sd.real_message",
    "pattern": "rt=%{sd.aler_rt} msg=%{sd.alert_message} cat=%{sd.alert_category} deviceExternalId=%{sd.device_extId} smac=%{sd.source.mac} dmac=%{sd.destination.mac} src=%{sd.source.ip} dst=%{sd.destination.ip} dpt=%{sd.destination.port} %{sd.source.hostname} %{sd.destination.hostname} %{sd.filePath} %{sd.fileType} %{sd.fsize} %{sd.user_message}",
```

```
"on_failure" : [ {"set" : {"field" : "error.message", "value" : "{{ _ingest.on_failure_message }}"}}]
}
```

我们将其放在单独的解析器中是为了允许拆分不同的 syslog shipper。正如你在 SilentDefense 练习中回忆的那样，我们创建了两个单独的 syslog 转发器，NetworkLogs 和 AlertLogs。这两者在格式化 syslog 消息的方式上略有不同，因此这是处理该问题的地方。你下载的 silentdefense 文件显示了 AlertLogs 转发器的第二个 dissector。

+   9. 下载完成后，将 syslog 和 silentdefense 文件复制到 Security Onion VM 上的 /opt/so/saltstack/local/salt/elasticsearch/files/ingest/ 文件夹中。

+   10. 我们现在需要重启 Elasticsearch。运行 sudo so-elasticsearch-restart 命令即可。

+   11. Elasticsearch 重启后，来自 SilentDefense 的 syslog 消息现在被解析为以下形式：

| ... |  |
|---|---|
| event.dataset | ot-ids |
| event.module | silentdefense |
| sd.host.name | IND-securityonion |
| ingest.timestamp | 2020-12-11T23:46:11.475Z |
| log.source.address | 172.25.100.240:53545 |
| message | <111>1 2020-12-11T16:46:11.472-07:00 SilentDefense-001 AlertLogs ... |
| sd.aler_rt | Feb 15 2021 16:46:11 |
| sd.alert_category | NetworkAttack |
| sd.alert_message | NMAP Scan detecte |
| sd.alert_name | nmap_scan |
| sd.alert_number | 11 |
| sd.alert_title with NMAP | A host on the network is scanning |
| sd.device_extId | SD-ProductionLine1 |
| ... |  |

