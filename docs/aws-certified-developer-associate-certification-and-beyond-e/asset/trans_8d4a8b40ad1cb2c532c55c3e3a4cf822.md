

图 9.4 中的流程确保只有拥有有效签名 cookie 的用户或应用程序才能访问私有存储桶中的内容，而该内容只能通过使用私钥的 RESTful 服务获取。这是在云存储服务中保护内容的典型模式，通过加密方法提供受控访问。

同时也必须理解，签名者必须创建签名 URL 或签名 cookie。这可以是创建于 CloudFront 的受信任密钥组，也可以是包含 CloudFront 密钥对的 AWS 账户。AWS 建议使用受信任密钥组配合签名 URL 和签名 cookie。做出此建议有几个原因。

使用受信任密钥组时，无需使用 AWS 账户根用户来管理公钥。AWS 建议你不要将根用户用于日常操作。此外，密钥组允许对密钥进行更灵活的管理，例如在不更改 CloudFront 分发的情况下轮换密钥。你还可以定义 IAM 权限以限制不同用户可以执行的操作；例如，允许用户上传公钥但不允许删除它们。此外，作为一项附加功能，可以将更多公钥与分发关联。

使用 CloudFront 密钥对时，它们与 AWS 账户根用户绑定，并且在管理方面灵活性较低。你按图 9.5 在根用户安全凭证环境中进行配置：

![](img/34ff9b5b5a734dae3450ab701e223c73_359_0.png)

图 9.5：不建议为根用户配置 CloudFront 密钥对

重要的是，你每个 AWS 账户只能拥有两个活动的 CloudFront 密钥对。

在受信任密钥组和 CloudFront 密钥对之间进行选择通常取决于你的运营规模和管理偏好。受信任密钥组为更大、更动态的环境提供更好的控制和管理。

当通过 CloudFront 分发 URL 分发内容时，绕过此 URL 直接连接到源是可能的，具体取决于源类型。例如，如果你在 Amazon S3 存储桶上创建静态网站并且对 S3 存储桶具有完全的公共访问权限，那么任何知道 URL 地址的人都可以通过网站端点 URL 连接到它。如果你想防止此类访问，下一个部分将介绍称为源访问身份（OAI）和源访问控制（OAC）的功能，它们有助于实现这一点。

## CloudFront OAI 和 OAC

Amazon CloudFront 提供两种选项来确保查看者只能通过 CloudFront 分发点访问你的内容。

CloudFront OAI 是一项功能，可让你强制仅使用你在 CloudFront 分发中创建的 OAI 身份来访问托管在源中的内容，例如 Amazon S3 存储桶。一旦配置此功能，你必须更新存储桶策略以仅允许来自 OAI 用户的内容访问。你通过将以下内容添加到存储桶策略来实现：

```json
"Principal": {
    "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <origin access identity ID>"
}
```

这确保 OAI 代表用户授予对存储桶中托管内容的访问权限。存储桶策略的此更新可以在完成 CloudFront 分发时自动创建，也可手动创建策略。如果用户尝试使用 Amazon S3 端点直接访问内容，将被拒绝访问。

然而，此功能存在一些缺点，包括以下内容：

- OAI 不支持所有 AWS 区域。
- OAI 不原生支持使用 AWS KMS 的 Amazon S3 服务器端加密（SSE-KMS）。它仅支持使用 Amazon S3 管理密钥的服务器端加密（SSE-S3）。
- 不支持动态请求，例如 PUT、POST 或 DELETE。

新的 OAC 解决了这些缺点，接下来将讨论。

使用 OAC 是 AWS 推荐的方法，用于授予用户访问托管在源中的内容的方法，例如 Amazon S3 存储桶。在 CloudFront 分发中配置 OAC 之前，你必须允许 CloudFront 服务主体（cloudfront.amazonaws.com）访问你的 Amazon S3 存储桶。具体来说，你可以定义一个存储桶策略，其中仅在请求通过 CloudFront 分发到达时才授予访问权限，使用 Condition 元素。以下示例策略显示了这一点，该策略允许用户同时读取和写入存储桶中的内容：

```json
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowCloudFrontServiceReadWritePermission",
        "Effect": "Allow",
        "Principal": {
            "Service": "cloudfront.amazonaws.com"
        },
        "Action": [
            "s3:GetObject",
            "s3:PutObject"
        ],
        "Resource": "arn:aws:s3:::<S3 bucket name>/*",
        "Condition": {
            "StringEquals": {
                "AWS:SourceArn": "arn:aws:cloudfront::<AWS account ID>:distribution/<CloudFront distribution ID>"
            }
        }
    }
}
```

虽然确保用户通过 CloudFront 分发访问你的内容可以增强安全性，但你也希望保护最终用户与内容之间的网络流量。下一节讨论传输加密，以保护使用 Amazon CloudFront 时内容的连接性。这涉及在最终用户与 CloudFront 分发之间配置 HTTPS，并强制 CloudFront 与源之间的 HTTPS 连接，无论是自定义源还是 Amazon S3 源。

## 在用户与 Amazon CloudFront 之间配置 HTTPS

为确保最终用户与通过 CloudFront 访问的内容之间的流量在传输过程中得到加密，你可以使用 HTTPS。你可以同时使用 HTTPS 进行最终用户与 CloudFront 分发点之间的通信以及 CloudFront 与源之间的通信。

图 9.6 描述了此通信的高级概述：

![](img/34ff9b5b5a734dae3450ab701e223c73_361_0.png)

图 9.6：通过 CloudFront 分发建立到你的内容的 HTTPS 连接

以下是在前述图中描绘的流量流程中的关键步骤：

1. 最终用户尝试与 CloudFront 建立 HTTPS 连接，这会导致 SSL/TLS 协商。
2. 建立后，最终用户提交加密请求。
3. 如果发生缓存命中，即请求的 content 存在于缓存中，则将其返回给最终用户，后者然后解密内容。
4. 如果发生缓存未命中，CloudFront 将连接到源并执行 SSL/TLS 协商。
5. CloudFront 然后将加密请求转发到源。
6. 源解密请求并处理消息。然后它加密响应并发送给 CloudFront。
7. CloudFront 解密响应并重新加密，确保将其副本存储在缓存中并转发给最终用户。
8. 最终用户然后解密响应。

在配置 HTTPS 连接时，你可以为最终用户与 CloudFront 分发之间的连接以及从 CloudFront 分发到源的连接配置 HTTPS。以下部分概述了这两个配置组件。

## 配置最终用户与 CloudFront 分发之间的 HTTPS 通信

这涉及配置缓存行为以要求最终用户/查看者与分发之间的通信使用 HTTPS。你也可以将此通信配置为同时允许 HTTP 和 HTTPS，从而允许你通过 HTTP 提供某些内容但强制其他内容仅通过 HTTPS。

在这里，你有两个配置选项：

- **使用默认域名：** 如果你使用提供的分发域名，例如 `x12121212ebcdef9.cloudfront.net`，你必须将查看器协议策略配置为 **Redirect HTTP to HTTPS** 或 **HTTPS Only** 设置。HTTP GET 和 HEAD 请求将使用前一个选项重定向到 HTTPS 请求。POST、PUT、DELETE、OPTIONS 或 PATCH 请求仅在你使用 HTTP 1.1 或更高版本时重定向到 HTTPS。
- **使用自定义域名：** 如果你使用自定义域名，你将需要使用 SSL/TLS 证书，你可以使用 **AWS 证书管理器（ACM）** 配置，或购买第三方证书并将其导入 ACM。

除了在最终用户与 CloudFront 分发之间配置 HTTPS 外，你还可以配置 CloudFront 分发与其源之间的 HTTPS。这可以是自定义源或 Amazon S3 源。

