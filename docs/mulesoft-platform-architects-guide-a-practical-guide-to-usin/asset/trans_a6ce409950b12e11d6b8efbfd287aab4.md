

# 专用负载均衡器的映射规则

映射规则与已在专用负载均衡器上配置的域证书相关联，每个域证书可以关联多个映射规则。如果存在多个规则匹配，传入专用负载均衡器的请求将始终执行第一个匹配的规则。

映射规则会转换传入请求的 URI，以将请求路由到部署在 Anypoint VPC 内 CloudHub 1.0 中的目标应用程序。在配置映射规则之前，需要了解以下几个参数：

*   **`inputUri` (输入路径)**：这是客户端请求的 URI —— 例如 `/{app}/`。
*   **`appName` (目标应用)**：这是在 Anypoint VPC 内 CloudHub 1.0 中运行的 MuleSoft 应用程序。专用负载均衡器将根据传入请求的 URI 将请求路由至此。
*   **`appUri` (输出路径)**：这是传递给部署在 Anypoint VPC 内的目标应用程序的 URI 字符串。
*   **`Protocol` (协议)** —— 这是目标（下游）应用程序监听所用的协议。可以是 HTTP、HTTPS、WS 等。

映射规则输入路径中花括号 (`{}`) 内定义的值为变量。这些变量不能用于输出路径，但可用于目标应用参数中的应用程序名称。

如果在专用负载均衡器上定义了多个映射规则，无论存在多少匹配规则，它始终会将第一个匹配的规则应用于传入请求。

让我们了解如何在专用负载均衡器上定义映射规则。

## 示例 1

应用程序名称可以包含在传入请求的 URL 中，并可用于将请求路由到部署在 CloudHub 1.0 中的应用程序。输入请求通过 [https://api-dev.domainname.com/invoice-app-dev/v1.0/invoices](https://api-dev.domainname.com/invoice-app-dev/v1.0/invoices) 发送以获取发票。以下是配置的映射规则：

| **输入路径** | **目标应用** | **输出路径** | **协议** |
| --- | --- | --- | --- |
| `/{app}/` | `{app}` | `/` | `https` |

表 9.8 - 专用负载均衡器映射规则

`{app}` 是变量，其值为 `invoice-app-dev`。HTTPS 是下游 API 的协议。因此，专用负载均衡器的映射规则会将 URL 转换为 [https://mule-worker-internal-invoice-app-dev.us-e1.cloudhub.io:8092/v1.0/invoices](https://mule-worker-internal-invoice-app-dev.us-e1.cloudhub.io:8092/v1.0/invoices)：

![图 9.21：带有映射规则的专用负载均衡器 URL 转换](image/B19922_09_21.jpg)

图 9.21：带有映射规则的专用负载均衡器 URL 转换

## 示例 2

在*示例 1*中，我们了解了如何通过专用负载均衡器使用应用程序名称路由请求。

在本示例中，我们将学习如何使用子域名路由请求。通常，每个环境都有不同的子域名。所有非生产环境将映射到非生产 Anypoint VPC，生产环境将映射到生产 Anypoint VPC：

| **域** | **子域名** | **Anypoint VPC** | **环境** |
| --- | --- | --- | --- |
| api-dev.subdomain.com | api-dev | VPC-1 | 开发 |
| api-uat.subdomain.com | api-uat | VPC-1 | UAT |
| api.subdomain.com | api | VPC-2 | 生产 |

表 9.9 - API 域

让我们考虑以下映射规则，它们已配置为将请求路由到部署在 CloudHub 1.0 中的应用程序。输入请求通过 [https://api-dev.domainname.com/invoice/v1.0/invoices](https://api-dev.domainname.com/invoice/v1.0/invoices) 发送以获取发票。这是配置的映射规则：

| **输入路径** | **目标应用** | **输出路径** | **协议** |
| --- | --- | --- | --- |
| `/{app}/` | `{app}-{subdomain}` | `/` | `https` |

表 9.10 - 专用负载均衡器映射规则

`{app}` 是变量，值为 `invoice`，`{subdomain}` 为 `api-dev`。HTTPS 是下游 API 的协议。因此，专用负载均衡器映射规则会将 URL 转换为 [https://mule-worker-internal-invoice-api-dev.us-e1.cloudhub.io:8092/v1.0/invoices](https://mule-worker-internal-invoice-api-dev.us-e1.cloudhub.io:8092/v1.0/invoices)：

![图 9.22 - 带有映射规则的专用负载均衡器 URL 转换](image/B19922_09_22.jpg)

图 9.22 - 带有映射规则的专用负载均衡器 URL 转换

## HTTP 入站模式

专用负载均衡器上的一项关键配置决定了如何路由传入的 HTTP 请求。它可以接受 HTTP 或 HTTPS 请求，静默丢弃 HTTP 请求，或将请求重定向到下游 API 的 HTTPS。

可以在专用负载均衡器上配置以下选项，以确保接受传入流量并将其发送到下游 API：

*   **`Off` (关闭)**：专用负载均衡器将静默丢弃所有传入的 HTTP 流量，仅接受 HTTPS 请求。
*   **`On` (开启)**：专用负载均衡器将在默认 SSL 端点上接受传入的 HTTP 请求。
*   **`Redirect` (重定向)**：专用负载均衡器将始终通过 HTTPS 将流量路由到下游 API。

## 建议

以下是在配置专用负载均衡器时可遵循的一些建议：

*   与 Anypoint VPC 关联的所有环境都将映射到专用负载均衡器。这意味着单个专用负载均衡器可以将请求路由到多个环境。
*   多个 CNAME 可以映射到单个专用负载均衡器或单个 A 记录，例如开发环境的 `api-dev.domainname.com`、QA 环境的 `api-uat.domainname.com` 和生产环境的 `api.domainname.com`。
*   始终为生产环境和沙箱环境创建单独的负载均衡器。

让我们看看专用负载均衡器和共享负载均衡器的区别：

| **功能** | **共享负载均衡器** | **专用负载均衡器** |
| --- | --- | --- |
| 自定义域 | 不支持 | 支持 |
| 自定义证书 | 不支持 | 支持 |
| 双向认证 | 不支持 | 支持 |
| 白名单 CIDR | 不支持 | 支持 |
| 速率限制 | 适用 | 不适用 |

表 9.11 - 共享负载均衡器与专用负载均衡器对比

## 用于公共和私有流量的专用负载均衡器

部署到 Runtime Manager 的 API 可能会接收来自外部和内部消费者的流量。出于安全原因，可能需要保护某些 API 免受外部消费者访问，或不为这些 API 提供外部访问。建议为私有和公共流量分别设置独立的专用负载均衡器。我们可能需要某些 API 仅内部可访问，或者某些 API 面向公众。在这里，我们将看到一个 API 主导连接的示例，其中体验 API 必须是公开 API，而流程 API 和系统 API 必须是私有 API。

有多种解决方案可实现这一点：

*   单个专用负载均衡器用于公共和私有 API
*   单独的专用负载均衡器分别用于公共和私有 API

### 方法 1 – 单个专用负载均衡器用于公共和私有 API

在此方法中，我们将设置一个专用负载均衡器，它将提供两个 **A** 记录：`<lb-name>.lb.anypointdns.net`（接受外部流量）和 `internal-<lb-name>.lb.anypointdns.net`（接受私有本地网络流量）。您可以在专用负载均衡器上上传多个证书，并创建指向这些 **A** 记录的 CNAME 记录。

以下是 DNS 注册表中 CNAME 和 A 记录映射的示例：

*   `api.domainname.com` => `<lb-name>.lb.anypointdns.net` (A 记录)

