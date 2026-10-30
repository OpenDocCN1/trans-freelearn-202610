
首先，您需要输入您组织的 Salesforce URL，这需要您拥有 Salesforce 订阅。如果您有的话，欢迎进一步尝试此连接器，但为了本练习的目的，我们将返回可用连接器的列表并继续探索。

- 7. 点击位于左侧边栏顶部的 **Back to Connectors** 链接以返回可用连接器的列表，如以下屏幕截图所示：

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_589_0.png)

图 16.3 – 连接器页面是集成管理员体验的主页

- 8. 将鼠标悬停在任何可用连接器上以显示相应的 **Getting Started** 链接，该链接打开一个页面，进一步解释所选连接器的功能。

花必要的时间对其他连接器重复上述步骤，以便更好地了解它们各自的功能。

在本节中，您已查看了随 Adobe Captivate Prime LMS 一起提供的开箱即用连接器。由于这些连接器允许您将 Captivate Prime 连接到其他应用程序，因此为了配置这些连接器，您除了订阅 Captivate Prime 本身外，还必须订阅所选的外部服务。

> **Note**
> 这些连接器由 Adobe Captivate Prime 开发团队开发。目前不支持向列表中添加自定义连接器。

可用的连接器涵盖了世界各地组织使用的许多最受欢迎的企业应用程序和服务。但在不断变化的技术环境中，不可能为每一种可以想象的外部应用程序提供专用连接器。很有可能您组织内使用的应用程序并未被这些连接器所覆盖。在这种情况下，您需要依赖 Captivate Prime API 以及一些自定义开发，将 Captivate Prime 与您组织中使用的解决方案集成起来。这就是我们在下一节中将讨论的内容。

## 使用 Captivate Prime API 开发应用程序

API 代表 **Application Programming Interface**。它是一种接口类型，通常由开发人员用来连接不同的应用程序。通过 Captivate Prime API，您帐户中包含的几乎任何数据以及 Captivate Prime LMS 提供的任何服务都可以以编程方式访问并集成到由您内部开发团队开发的任何类型的自定义应用程序中。

以下是涉及 Adobe Captivate Prime API 的一些最常见用例：

- 一些组织希望利用 Adobe Captivate Prime 的功能，而不使用默认的学习者体验。在这种情况下，他们可以使用自己选择的任何技术开发自己的个性化学习者体验。最常见的是，这种定制的用户体验以网站和/或移动应用程序的形式出现，完全由组织的内部开发人员设计和开发。在幕后，该网站/应用程序使用 Captivate Prime API 访问必要的数据（例如已登录用户的详细信息、可用课程列表和完成状态），并将学习者交互报告回 Captivate Prime。这就是所谓的将 Captivate Prime 用作 *headless* LMS。
- 您可以使用 Captivate Prime API 将 Captivate Prime 与平台中内置连接器未涵盖的企业应用程序集成。
- 您可以使用 Captivate Prime API 构建扩展 Captivate Prime 原生功能的自定义应用程序。

这些只是 Captivate Prime API 有用的众多实例中的几个。

如果这对您来说听起来有点技术性，请记住 Captivate Prime API 是为开发人员准备的。作为 Captivate Prime 管理员或集成管理员，您无需完全了解开发人员在构建这些自定义解决方案时使用的编程语言中如何实现 Captivate Prime API 的细节。

您唯一需要注意的是，Captivate Prime API 使用名为 **OAuth 2.0** 的安全框架来验证和授权外部应用程序。此安全框架要求您为每个需要通过 API 访问您的 Captivate Prime 帐户的外部应用程序生成安全令牌。

> **OAuth**
> 
> **OAuth** 代表 **Open Authorization**。它是一种开放的行业标准，用于授予网站和其他应用程序安全地相互访问。有关 OAuth 标准的更多信息可在官方 OAuth 网站 `https://oauth.net/` 上找到。我强烈建议您观看他们主页上的视频，以便快速但透彻地了解 OAuth 是什么。

为了尝试此工作流程，假设您想将 Captivate Prime 用作 headless LMS，因此您需要开发一个自定义网站/应用程序，为您的用户提供学习者体验。此自定义网站/应用程序由您托管，并通过 API 访问必要的 Captivate Prime 数据。因此，您需要使用以下步骤将此应用程序注册到您的 Prime 帐户并生成必要的安全令牌：

- 1. 以集成管理员身份登录您的 Prime 帐户。
- 2. 从集成管理员主页，单击左侧边栏中位于的 **Applications** 链接。
- 3. 单击页面右上角的 **Register** 按钮以打开 **Register a new application** 页面。
- 4. 在 **Application Name** 字段中输入应用程序名称。这不是技术性的，因此请随意选择您认为合适的任何名称。请注意，此名称之后无法更改。
- 5. 在 **URL** 字段中提供您的自定义应用程序将托管的 URL。
  为本练习的目的，只需输入您网站的 URL 或任何其他有效 URL。
- 6. 在 **Redirect Domains** 字段中输入 `https://*.<the_address_of_your_website>`。

这些是在用户通过 OAuth 服务器进行身份验证后安全重定向用户的域。请注意，您可以在此字段中添加多个重定向域。

- 7. 作为最佳实践，花必要的时间提供您正在注册的应用程序的简要描述。
- 8. 在页面的 Scopes 部分中，选择您的应用程序所需的复选框。

在此示例中，您正在构建自定义学习者体验，但您仍将使用原生的 Captivate Prime 管理员来构建课程并管理您的 LMS。因此，您只需要选择学习者角色读取访问权限和学习者角色写入访问权限。

使用此配置，您的自定义应用程序将被授权读取和写入与学习者角色相关的任何内容，但您将无法使用它远程管理您的 Captivate Prime 帐户。

- 9. 如果您希望此应用程序对您的 Prime 帐户的其他管理员可见，请在 For this account only? 字段中选择 No 单选按钮；否则，保留为 Yes。
- 10. 当 Register a new application 页面如下图所示时，单击页面右上角的 Save 按钮：

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_592_0.png)

图 16.4 – 注册新应用程序页面

- 11. 一旦您单击 **Save** 按钮，页面将刷新并显示应用程序 ID 和应用程序密钥。这些是您的开发人员需要在发送到 Captivate Prime API 的每个请求中包含的安全令牌。

### 我的自定义学习者体验

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_593_0.png)

图 16.5 – 点击保存时将生成应用程序 ID 和应用程序密钥

- 12. 单击左侧边栏顶部位于的 **Back to Applications** 链接以返回集成管理员体验的 **Applications** 页面。

返回 **Applications** 页面后，确认您新注册的应用程序出现在页面上。如有需要，您可以单击此应用程序编辑其详细信息或完全删除它。

> **Internal and external applications**
> 
> 您在本节中注册的应用程序被视为 *internal* 应用程序。外部应用程序由 Captivate Prime 提供或由 Prime 合作伙伴开发。要注册 Prime 合作伙伴提供的应用程序，请使用 **Applications** 页面右上角的 **Approve** 按钮并输入该合作伙伴提供的应用程序 ID。要使用 Captivate Prime 提供的任何应用程序，请使用 **Applications** 页面的 **Featured Apps** 选项卡。

