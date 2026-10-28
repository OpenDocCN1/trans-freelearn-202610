
- 对 HEC API 的身份验证是通过源发送的 HTTP 请求中提供的令牌来完成的。令牌配置由管理员设置，并与应用程序/源团队共享，以便他们发送事件。
- HEC 提供了两个重要的 RESTful 端点用于数据索引——一个用于访问仅限 JSON 格式的事件，另一个用于接受任何原始类型的数据。JSON RESTful 端点专为接受仅以 JSON 格式化的数据而设计。在向此端点发送数据时，应确保载荷（数据）以 JSON 结构正确格式化，并根据需要包含额外的元数据字段。封装在 JSON 载荷中的原始数据事件可以是原始事件。
- 为每个应用程序/源创建的每个令牌都必须是唯一标识符。最佳实践是使用 32 个字符的**全局唯一标识符（GUID）**作为令牌，例如 `CD1E753F-5BB2-4F5C-AE22-FE0381A0D916`。源应用程序在将事件作为载荷发送到收集器端点时，会在 HTTP 请求中提供此令牌。
- HEC 会自动解析接收到的 JSON 事件，因为其事件时间戳和其他元数据遵循预定义的结构。对于原始数据，如果包含时间戳，则应用断行策略；或者，可以在 `props.conf` 中配置 `sourcetype` 设置，以相应覆盖此行为。
- HEC 通过 `useAck` 索引器确认设置提供事件索引确认功能。索引器确认在节级别为单个令牌启用；一个单独的端点提供事件索引的状态。
- 要允许/限制某些地址通过 HEC 发送数据，可以在 `inputs.conf` 文件的 `[http]` 节级别配置 `acceptFrom` 设置。

## 配置 HEC

现在，我们将逐步完成配置 HEC 的过程。

**步骤 1：** 让我们为通过 HEC 发送数据做好准备。第一步是在 Splunk Web 中启用 HEC。导航至 **设置 | 数据输入 | HTTP 事件收集器**，然后点击右上角的**全局设置**按钮。这将打开一个包含**所有令牌**选项的表单。将此选项设置为**已启用**，然后点击**保存**。

在**全局设置**中启用 HEC 时，您应该已经注意到几件事，其中之一是**启用 SSL** 默认处于选中状态，另一件事是收集器端点监听接收事件的 HTTP 端口号是 8088。

HEC 全局设置也可以通过 `[http]` 节进行配置，该节默认位于 `$SPLUNK_HOME/etc/apps/splunk_httpinput/default/inputs.conf`。要使用此方法启用 HEC，请将以下设置添加到 `$SPLUNK_HOME/etc/apps/splunk_httpinput/`

- https://<splunk-hostname>:8088/services/collector/ack：用于查询事件索引状态。它适用于已启用索引器确认的令牌。
- 完整列表可在 https://tinyurl.com/j53xmynj 获取。

为了展示如何向这些 HEC 端点发送数据，我们将在 Linux 环境中使用 curl 命令。我们可以看到有两个标头值——一个带有承载令牌值的 Authorization，另一个带有特定于客户端的唯一 GUID 的 X-Splunk-Request-Channel。

这里我们以 JSON 格式的载荷发送数据——一个收集器/事件请求：

```
curl -k -H "Authorization: Splunk 09776ade-cf23-42c0-9138-89ad8388516a" -H "X-Splunk-Request-Channel: FE0ECFAD-13D5-401B-847D-77833BD77131" https://mysplunk.example.com:8088/services/collector/event -d '{"sourcetype": "signaling_data", "event": "stable signal!"}'
```

以下是收集器/事件响应：

```
{"text":"Success","code":0,"ackId":0}
```

这里我们以原始事件发送数据——一个收集器/原始请求：

```
curl -k -H "Authorization: Splunk 09776ade-cf23-42c0-9138-89ad8388516a" -H "X-Splunk-Request-Channel: FE0ECFAD-13D5-401B-847D-77833BD77131" https://mysplunk.example.com:8088/services/collector/raw -d 'stable signal!'
```

我们得到如下收集器/原始响应：

```
{"text":"Success","code":0,"ackId":1}
```

由于我们在令牌级别设置了 useACK = 1（true），客户端必须在发送给 HEC 的请求中携带唯一的通道标识符。作为回报，客户端会收到 HEC 以 JSON 格式返回的 ackID（请参阅前面收集器/原始事件示例中的响应载荷）。客户端进一步使用这些 ack 查询 HEC 的 /ack 端点，以了解事件索引状态，如下所示。

查找事件索引状态——/ack 端点请求——需要前面两个 JSON 和原始事件请求中返回给客户端的通道标识符和 ack：

```
curl -H "Authorization: Splunk 09776ade-cf23-42c0-9138-89ad8388516a" -H "X-Splunk-Request-Channel: FE0ECFAD-13D5-401B-847D-77833BD77131" https://mysplunk.example.com:8088/services/collector/ack -d '{"acks":[0,1]}'
```

上述 ack 端点请求的 JSON 响应载荷如下所示：
`{"acks": {"0": true, "1": false}}` — 其中，`true` 表示索引成功，`false` 表示该事件索引失败。一旦特定事件的 `acks` 返回 `true`，其索引状态将从内存中消失，如果客户端之后再次发送请求，它将返回 `false`。

我们已经讲完了 HEC 输入——它很有技术含量，不是吗？不要被这些技术术语所困扰。您需要记住，将 HEC 视为一个普通的 API，每个请求都需要提供令牌进行身份验证。索引器确认本质上与包裹投递跟踪功能相同——例如，您下单后（比如通过 `/raw` 或 `/events` 端点），您会得到一个跟踪号（`ackID`），然后在门户（`/ack` 端点）中输入跟踪号（`acks`），即可跟踪包裹的位置。最后，在送达后，您会从跟踪门户收到一个*成功送达*状态。

在下一节中，我们将介绍 Windows 输入。

## 探索 Windows 输入

到目前为止，我们介绍过的输入类型既不针对特定技术，也不针对特定操作系统。但正如其名称所示，Windows 输入仅适用于 Windows 主机。主机至少需要 UF 或 Splunk Enterprise 实例才能收集 Windows 特定的日志。Windows 原生日志以二进制格式存储，Windows 输入通过与操作系统 API 交互来获取这些日志。

在 Windows 主机上安装 UF 时，您可以选择启用 Windows 输入。对于 Splunk Enterprise 实例，可以通过 Splunk Web 中的**设置 | 数据输入**，然后选择**本地事件日志收集**和**远程事件日志收集**来配置 Windows 输入。远程事件日志收集需要 Windows 域帐户才能从远程主机收集日志。

在已配置 UF 的大规模 Windows 主机环境中，使用部署服务器集中管理输入。在应用程序中创建并配置 `inputs.conf`。将应用程序部署到转发器，以实现高效且集中的输入配置。让我们了解一些关于 Windows 输入的关键事实：

- Windows 输入能够收集 Windows 事件、AD、性能监控、注册表、打印机、主机和网络日志。
- **Windows 管理规范（WMI）** 用于从网络中的远程主机收集事件日志，通常在具有域帐户的 Windows 服务器上。
- 远程事件日志收集在 `wmi.conf` 文件中配置。完整规范可在 `https://tinyurl.com/37mdekpe` 获取。
- 启用 Windows 日志可能会导致数据量显著增加，可能消耗我们 Enterprise 许可证的很大一部分。Splunk 提供了两个设置，`whitelist` 和 `blacklist`，用于在不需要的事件日志被索引之前过滤掉它们。这有助于减少许可证使用量，并仅索引所需事件。

