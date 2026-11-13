

### 表 9.12 - Mule Maven 插件属性

| 属性 | 说明 | 条件 | 默认值 |
| --- | --- | --- | --- |
| `connectedAppClientId` | 指定连接应用 ClientID 值。 | 仅当使用连接应用凭据登录时 |  |
| `connectedAppClientSecret` | 指定连接应用客户端密钥。 | 仅当使用连接应用凭据登录时 |  |
| `connectedAppGrantType` | 指定唯一支持的连接类型：客户端凭据。 | 仅当使用连接应用凭据登录时 |  |
| `applyLatestRuntimePatch` | 启用后，应用程序将首先更新至最新版本的 Mule Runtime，然后再部署到 CloudHub。 | 否 | `false` |
| `disableCloudHubLogs` | 设置为 `true` 时，此功能将禁用 CloudHub 日志。 | 否 | `false` |

有关 Mule Maven 插件的更多详细信息，请参阅 MuleSoft 文档：<https://docs.mulesoft.com/mule-runtime/4.5/mmp-concept>。

可以使用以下命令来部署和取消部署应用程序：

```
mvn clean deploy –DmuleDeploy
mvn mule:undeploy
```

Mule Maven 插件可以使用以下身份验证机制之一将应用程序部署到 CloudHub 1.0：

*   Anypoint Platform 用户名和密码
*   服务器
*   Anypoint 连接应用
*   认证令牌

## Anypoint CLI

Anypoint CLI 可用于将 MuleSoft 应用程序部署、取消部署或重新部署到 CloudHub 1.0。Anypoint CLI 是 Anypoint Platform 的脚本编写和命令行工具。Anypoint CLI 可以管理 Anypoint Platform 的各种功能，例如 CloudHub 1.0、API 治理、Anypoint 专用负载均衡器、Anypoint VPC、Exchange、Design Center 和 API Manager。要使用 Anypoint CLI，您需要设置 Anypoint CLI 的 npm 包。

以下是 MuleSoft 提供的用于设置 Anypoint CLI 的文档：<https://docs.mulesoft.com/anypoint-cli/4.x/install>。

Anypoint CLI 可以使用以下身份验证机制之一来管理 Anypoint Platform：

*   Anypoint Platform 用户名和密码
*   客户端 ID 和客户端密钥
*   Bearer 令牌 – 仅限命令行

可在 MuleSoft 文档中找到用于管理 CloudHub 1.0 的命令列表：<https://docs.mulesoft.com/anypoint-cli/4.x/cloudhub-apps>。

可以使用以下命令将应用程序部署到 CloudHub 1.0：

```
runtime-mgr:cloudhub-application:deploy <name> <zipfile> [flags]
```

可以将以下标志与此命令一起传递：

### 表 9.13 - 用于将应用程序部署到 CloudHub 1.0 的 Anypoint CLI 属性

| 标志 | 说明 | 默认值 |
| --- | --- | --- |
| `--runtime` | 提供运行时环境的名称和版本。 |  |
| `--workers` | 工作节点数量。 | `1` |
| `--workerSize` | 工作节点的大小（以 vCore 为单位）。 | `0.1` |
| `--region` | 区域名称。 | `us-east-1` |
| `--property` | 设置属性（`name:value`）——例如，`--property "https.port:8082"`。 |  |
| `--propertiesFile` | 此文件中的所有属性都应被其对应的值覆盖。请确保在本地硬盘上指定属性文件的绝对路径。 |  |
| `--[no-]persistentQueues` | 启用或禁用持久队列。 | `disabled` |
| `--[no-]persistentQueuesEncrypted` | 启用或禁用持久队列加密。 |  |
| `--[no-]staticIPsEnabled` | 启用或禁用静态 IP。 | `disabled` |
| `--[no-]objectStoreV1` | 启用或禁用对象存储 V1。 |  |
| `--[no-]objectStoreV2` | 启用或禁用对象存储 V2。 |  |
| `--[no-]autoRestart` | 应用程序在无响应时将自动重启。 | `enabled` |
| `--output` | 指定响应所需的格式。可用选项为 `table` 和 `json`。 | `table` |
| `--help` | 输出用法信息。 |  |
| `--timeout` | 设置超时值（以毫秒为单位）。此值应在 `60000` 到 `300000` 之间。 |  |

有关用于将应用程序部署到 CloudHub 1.0 的 Anypoint CLI 的更多详细信息，请参阅 MuleSoft 文档：<https://docs.mulesoft.com/anypoint-cli/4.x/cloudhub-apps>。

## CloudHub 1.0 API

CloudHub 1.0 API 允许您部署和管理 MuleSoft 应用程序。您可以部署 MuleSoft 应用程序、管理计划和队列，并查看有关应用程序的日志。以下是 MuleSoft 提供的用于将应用程序部署到 CloudHub 1.0 的 REST API：

### 表 9.14 - 用于将应用程序部署到 CloudHub 1.0 的 Platform API 属性

| 属性 | 值 |
| --- | --- |
| **方法** | `POST` |
| **API 端点** | <https://anypoint.mulesoft.com/cloudhub/api/v2/applications> |
| **请求体** | 请参阅下方的 JSON 示例 |
| **认证** | Basic 或 OAuth 2.0（连接应用或 Anypoint 用户名和密码） |
| **请求头** | `X-ANYPNT-ENV-ID`（Anypoint Platform 的环境 ID），`Content-Type`（`application/json`） |

请求体示例：

```
{
  "domain": "api-dev",
  "muleVersion": {
    "version": "4.4.0"
  },
  "region": "us-east-1",
  "monitoringEnabled": true,
  "monitoringAutoRestart": true,
  "workers": {
    "amount": 2,
    "type": {
      "name": "Medium",
      "weight": 1,
      "cpu": "1 vCore",
      "memory": "1.5 GB memory"
    }
  },
  "loggingNgEnabled": true,
  "persistentQueues": true,
  "objectStoreV1": true
}
```

有关将应用程序部署到 CloudHub 1.0 的 API 请求的更多详细信息，请访问 Anypoint Platform 开发者门户：<https://anypoint.mulesoft.com/exchange/portals/anypoint-platform/f1e97bc6-315a-4490-82a7-23abe036327a.anypoint-platform/cloudhub-api/>。

# 摘要

在本章中，我们探讨了 CloudHub 1.0 提供的功能和特性，以及如何利用它们来运行和管理 MuleSoft 应用程序。我们研究了各种功能，例如高可用性、容错性、可扩展性和灾难恢复。此外，我们了解了 Anypoint VPC，它允许我们在 CloudHub 1.0 内创建隔离且独立的网络段。我们还理解了 Anypoint VPC 的重要性。此外，我们探讨了不同的网络连接选项，包括 VPC 对等连接、VPN IPsec 隧道、AWS Direct Connect 和传输网关附件，这些选项使我们能够连接防火墙后面或公有/私有云上的资源。此外，我们学习了如何利用 Mule Maven 插件、Anypoint CLI 和 Platform API 在 CloudHub 1.0 环境中部署 MuleSoft 应用程序。

在下一章中，我们将讨论 CloudHub 2.0，它允许我们在基于 Kubernetes 的容器化环境中运行 MuleSoft 应用程序。

# 延伸阅读

CloudHub 1.0 参考列表包含以下来源：

*   MuleSoft 文档：CloudHub 架构 | MuleSoft 文档
*   YouTube 系列：<https://youtube.com/playlist?list=PL5GwZHHgKcuDQ6vWarTLgVbrPEQipzbOM&si=xKjv6f7f10VRkyjg>
*   Medium 文章：Anypoint Virtual Private Cloud、Virtual Private Network 和 Dedicated Load Balancer 库及资源 | 作者：Jitendra Bafna | Medium
*   本章中的一些表格参考自 MuleSoft 官方文档：<https://docs.mulesoft.com/>

# 10

