

![](img/34ff9b5b5a734dae3450ab701e223c73_83_0.png)

Figure 2.5 – 使用用户池访问后端资源

另一种选择是通过 API Gateway 或 Application Load Balancer 访问托管在 AWS 上的应用程序和 API。具体来说，你将使用你的令牌来保护 API 并实现对其访问的授权。这可以通过使用 Cognito authorizer（类型为 COGNITO_USER_POOLS）来完成，以通过 API Gateway 或 Application Load Balancer 授予对应用程序的访问权限。

API 部署后，已通过身份验证的用户可以检索身份或访问令牌，并使用它在授权头中附加令牌的情况下调用 API 方法。

下图说明了此工作流程：

![](img/34ff9b5b5a734dae3450ab701e223c73_83_1.png)

Figure 2.6 – 使用用户池通过 API Gateway 和 Lambda 访问资源

Amazon Cognito 用户池旨在用于限制 Web 和移动应用程序的 API 访问。

然而，用户池不能用于直接访问其他 AWS 服务，例如 Amazon S3 或 Kinesis。为此要求，你需要创建 Amazon Cognito **身份池**。让我们接下来讨论这个问题。

## Amazon Cognito Identity Pools

Amazon Cognito 身份池旨在用于在你想要授权经过身份验证或甚至匿名用户直接访问你的资源时使用。使用身份池时，你要么使用来自 Cognito 用户池的身份，要么使用第三方 IdP 的身份，并将用户池令牌（或第三方 IdP 的令牌）交换为临时的 AWS 凭证。然后你使用这些凭证访问 AWS API 和 AWS CLI。

当你的应用程序将凭证会话分配给经过身份验证的用户时，你可以授予对后端 AWS 资源（如 Amazon S3、Kinesis 和 DynamoDB）的访问权限。

Cognito 身份池的另一个功能是能够授予访客用户（匿名访问，如前所述）的访问权限。这涉及在用户未通过 IdP 进行身份验证的情况下向用户授予唯一标识符和 AWS 凭证。要启用访客访问，你需要在身份池的用户选项卡中激活它。

你还可以结合使用 Amazon Cognito 用户池和身份池，如下图所示，以促进你的用户注册和登录、管理他们的个人资料、访问你的应用程序，甚至访问后端 AWS 服务和资源。如果你不需要直接访问后端资源，则不需要这些临时的 AWS 凭证，在这种情况下，Amazon Cognito 用户池足以满足你的使用场景。

![](img/34ff9b5b5a734dae3450ab701e223c73_84_0.png)

Figure 2.7 – 使用用户池和身份池访问 AWS 服务

最终，你可能会根据需求使用用户池、身份池，甚至两者兼用。总结来说，用户池旨在限制对 Web 和移动应用程序的 API 访问，仅限授权用户——你可以创建和管理自己的池来管理用户的身份（或与第三方提供商联合）。你还可以创建组并将用户关联起来。用户通过身份验证后，Amazon Cognito 会颁发访问令牌，然后用于通过 API Gateway 或 Application Load Balancer 授予对 API 的访问权限。

如果需要直接提供对后端服务（如 Amazon S3 或 DynamoDB）的访问，则需要使用身份池，因为你需要将访问令牌交换为 AWS 临时凭证。这些 AWS 临时凭证由 Amazon STS 创建和管理，我们接下来讨论它。

## How AWS STS Generates Temporary Tokens

STS 服务将自动生成凭证，这些凭证根据附加到身份的策略文档中定义的权限授予对 AWS 资源的有限访问。IAM 用户和联合用户都可以分配临时凭证。

临时安全凭证与 IAM 用户的永久访问密钥之间存在一些差异。虽然它们功能相似，但临时安全凭证具有一些独特功能：

- 临时凭证是为指定持续时间创建的，并在特定时间后过期
- 你可以配置凭证过期时间，从几分钟到数小时不等
- AWS 在过期后不再识别这些凭证，过期后进行的任何 API 请求都将被拒绝，除非继续具有续期凭证的权限

默认情况下，对 AWS STS 的调用会发送到其全局端点 https://sts.amazonaws.com，该端点会将请求重定向到 N. Virginia（us-east-1）区域。然而，你应考虑使用区域端点，其中大多数默认启用，以确保低延迟、增加冗余并提高会话令牌有效性。

以下是你可以使用 AWS STS 服务执行的常见操作：

- **AssumeRole**: 这会返回可用于访问你 AWS 账户中的资源或执行跨账户访问的临时凭证。凭证将包括访问密钥 ID、秘密访问密钥和安全令牌。在前面讨论的 IAM 角色部分中，你了解到如果 Todo List 前端网站托管在 EC2 实例上，并且网站需要访问托管在 S3 存储桶中的媒体内容，则需要将适当的角色分配给 EC2 实例作为实例配置文件。在这里，IAM 角色将由两个策略组成：一个信任策略，定义谁或什么可以承担该角色，在本例中为 EC2 服务。另一个策略包括承担角色的实体可以对资源执行的一组权限。因此，在本例中，它将是使用 GET 请求检索托管在 S3 中的媒体内容的能力。下图说明了此 AssumeRole 信任策略：

![](img/34ff9b5b5a734dae3450ab701e223c73_86_0.png)

Figure 2.8 – AWS STS AssumeRole 信任策略

- **AssumeRoleWithSAML**: 这会返回通过 SAML 身份验证流程分配给经过身份验证用户的临时凭证。通常用于企业身份联合以实现 SSO 功能，组织可以通过其默认 IdP 联合身份验证。这允许用户使用现有的 IdP 身份验证凭证登录，并启用基于角色的 AWS 访问。使用 AssumeRoleWithSAML 生成的临时凭证默认持续 1 小时，但可配置为 1 小时到 12 小时之间的值。

- **AssumeRoleWithWebIdentity**: 这会为通过 Web IdP（如 OAuth 2.0 提供商，例如 Amazon、Google 或 Facebook）在 Web 或移动应用程序中进行身份验证的用户返回临时凭证。这使你能够构建和部署应用程序以请求临时凭证，而无需在应用程序中存储长期凭证。这是因为用户使用来自 Web IdP 的令牌进行身份验证。

- GetSessionToken: 这会为 IAM 用户返回临时凭证，通常用于当你希望通过 MFA 对某些 AWS API 操作（如 EC2 StopInstances）提供额外保护时。IAM 用户将使用 GetSessionToken 函数并提供其 MFA 设备上的 MFA 代码，从而能够执行实例停止请求。

在本节中，你了解了 AWS STS 以及它如何促进临时安全凭证的创建和管理，这些凭证被 IAM 角色等功能使用。

接下来，让我们看一下 Todo List 应用程序设计架构的高级概述，并特别强调 Amazon Cognito 服务以及它如何集成到设计中。

## Amazon Cognito and the Todo List Application

我们的客户 TodoPlus Limited 将利用 Amazon Cognito 服务来促进其 Todo List 应用程序的终端用户注册和登录流程。对于此应用程序的目的，你还将使用自定义 UI 而不是托管 UI。Amazon Cognito 将与多个其他 AWS 服务集成以提供安全功能。这些包括防止分布式拒绝服务（DDoS）攻击以及与 API Gateway 集成以控制谁调用这些 API 操作。

在下图中，你将看到 Todo List 应用程序的整体应用程序架构。此时架构工作流程可能看起来有点不熟悉也没关系。随着你逐步学习本学习指南，你将了解用于在 AWS 上设计、构建和部署此应用程序的核心基础技术。对于现在，我们在高层次上突出一些我们将使用的关键 AWS 服务。然后在后续章节中深入研究每个服务。

![](img/34ff9b5b5a734dae3450ab701e223c73_87_0.png)

Figure 2.9 – Todo List 应用程序的高级架构设计

