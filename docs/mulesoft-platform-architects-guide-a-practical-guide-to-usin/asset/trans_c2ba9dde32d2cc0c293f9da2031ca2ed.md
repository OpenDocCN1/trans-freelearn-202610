
在确定了控制平面的托管要求并完成了带有权利和许可的预配后，我们接下来需要了解如何保护对平台的访问。我们将在下一节中探讨这一点。

## 保护 Anypoint 控制平面

Anypoint 安全性在 Management Center（管理中心）的 Access Management（访问管理）中进行管理。在这里，管理员可以管理用户、业务组和审核日志。管理 Anypoint Platform 用户主要有两种方式：

*   Anypoint 托管用户（GovCloud 实现不可用）
*   外部 **身份提供商** 和 **单点登录 (SSO)**

在 Access Management 中，可以邀请用户加入 Anypoint Platform。使用此方法时，Anypoint 管理用户安全性。请注意，虽然用户可以使用注册其他 Anypoint Platform 实例时使用的相同电子邮件，但用户名对于每个帐户必须唯一。接受帐户邀请后，最佳做法是让用户启用 **多因素认证 (MFA)**，事实上，从 2023 年 8 月 1 日起，这已成为强制要求。

**注意**

MFA 在用户的个人资料页面中配置，而不是在 Access Management 的 **User（用户）** 选项卡中。

每个用户对平台所有资产、权利、功能和能力的访问均通过团队和权限进行控制，这些取代了现已弃用的角色功能。

我们可以创建团队以与特定团队成员负责的活动保持一致。然后，我们可以向每个团队授予权限，以访问平台的不同功能和能力，以及他们被允许执行这些活动的环境。我们将在下一节中探讨业务组和环境。

图 2.1 展示了一个名为 **API Developers（API 开发人员）** 的示例团队，它是 API 设计人员团队的子团队，并拥有 JJ Music Back Office 中某些活动和资源的权限。

![Figure 2.1 – Team permissions in Anypoint](image/B19922_2_01.jpg)

*图 2.1 – Anypoint 中的团队权限*

您也可以在前图中看到，权限是从团队的父级继承的。

**注意**

团队权限是在确定要应用权限的业务组之前选择的。

用户也可以使用组织的 IdP 进行管理。将组织的 IdP 作为可验证应用程序连接到 Anypoint Platform 的设置位于 Access Management 的 **Identity Providers（身份提供商）** 菜单选项下。Anypoint 支持 OpenID Connect 和 SAML 2.0。每个提供商所需的信息会略有不同。使用 Okta 或 Microsoft **Active Directory (AD)** 等外部提供商会将用户管理的负担转移到单独的安全平台上，该平台负责验证用户身份并确认其对资产的访问权限。

下图摘自 MuleSoft Identity Providers 文档，展示了 SAML 流程：要求用户向 IdP 验证其凭据，IdP 随后向 Anypoint Platform 提供断言，告知平台用户是否被授权访问请求的资源。

![Figure 2.2 – SAML flow with Anypoint Platform](image/B19922_2_02.jpg)

*图 2.2 – Anypoint Platform 的 SAML 流程*

设置 Anypoint 的 SSO 需要与组织的安全团队协调。获取设置双方所需的某些信息可能类似于“鸡生蛋、蛋生鸡”的两难境地。通常，IdP 需要先完成其连接的设置，并将所需的公钥、元数据、登录和注销 URL 以及颁发者 ID 提供给 Anypoint 管理员。

IdP 还可用于将用户映射到 Anypoint 内的团队。团队仍必须在 Anypoint 中设置并配置适当的权限。但是，向 Anypoint 添加新用户的业务流程现在包括确定某人应被分配到哪个团队，从而决定其拥有何种访问权限。

**注意**

Access Management 中有两个外观相似的选项：**Identity Providers（身份提供商）** 和 **Client Providers（客户端提供商）**。Client Providers 允许架构师使用 OpenAM、PingFederate 或 OpenID Connect 设置 IdP，以授权访问 API 的客户端应用程序。IdPs 使用 OpenAM、PingFederate 或 OpenID 来授权对 Anypoint Platform 的访问。

现在正确的人员已获得 Anypoint 的访问权限，让我们看看用于组织和管理 Anypoint 控制平面的选项。

## 组织 Anypoint 控制平面

组织 Anypoint 有两种主要机制：业务组和环境。首先，让我们考虑业务组。

### 业务组

业务组是 MuleSoft 资源的逻辑划分，允许将 vCore 和 VPC 等资源分配到部门业务单元级别。此外，使用上一节中提到的团队，业务组允许我们创建设计人员、开发人员、运维人员和管理员组，以管理特定部门内的资源。

然而，Anypoint 权利（通过许可证提供的组件和功能）是在根业务组级别管理的。假设一个组织购买了 10 个沙箱 vCore 和 5 个生产 vCore。这些 vCore 会分配给首次预配 Anypoint Platform 时创建的根组织。我们可以在图 2.3 中看到，当 JJ Music Store 的所有者将 MuleSoft 预配为其集成平台时，创建了根业务组。

![Figure 2.3 – Management Center control plane – Access Management](image/B19922_2_03.jpg)

*图 2.3 – Management Center 控制平面 – Access Management*

**注意**

只有拥有 Organizational Administrator（组织管理员）权限（以前称为角色）的用户才能打开 Business Units（业务单元）页面。

我们还可以在图 2.3 中看到业务组 ID、客户端 ID 和客户端密钥。组织的客户端 ID 和客户端密钥仅为支持向后兼容性而包含。应改用每个创建的环境的客户端 ID 和密钥，供非组织管理员的用户访问组织的资产。向下滚动，您还将看到任何 CloudHub 部署的默认区域。

**注意**

在试用帐户中，此默认值始终为 US-East（俄亥俄州）。

从组织角度来看，建立至少一个子业务组并从根业务组向该组分配资源和权利是一种良好的做法。在图 2.4 中，我们可以看到新组的详细信息已填写完毕，在本例中，JJ Music Store 组的所有 1 个 vCore 和 1 个设计 vCore 都已分配给子业务组。

![Figure 2.4 – Creating a child business group](image/B19922_2_04.jpg)

*图 2.4 – 创建子业务组*

此时，JJ Music Store 业务组将有 0 个可用 vCore，因此无法部署任何应用程序。同样，这通常是作为最佳实践的首选。部署应与负责该部署的子业务组关联。如果需要并采购更多核心，vCore 将最初由 MuleSoft 分配给 JJ Music Store 业务组。

现在让我们看看环境在 Anypoint Platform 中的作用。

### 环境

环境是用于分离部署和应用程序的逻辑分组，是在业务组的上下文中创建的。正如前面 *Business groups（业务组）* 一节中提到的，每个环境都有自己的客户端 ID 和客户端密钥。CI/CD 流水线可以使用这些来指导部署，开发人员也可以在 Anypoint Studio 中使用它们来标识 API Manager 环境。

最佳做法是定义环境以与组织的开发和测试方法保持一致。
