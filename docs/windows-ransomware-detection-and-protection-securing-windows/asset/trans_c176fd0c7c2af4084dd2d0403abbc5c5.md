

![](img/7b49a27a3c2adc52d097f2e77f3bf394_125_0.png)

在 Intune 中，有多个配置文件可用于启用 **Application Guard**。如图 *4.5* 所示，这些选项仅以独立模式为 Microsoft Edge 启用该功能。如果我们想为 Office 和托管模式启用 Application Guard，则需要使用 Intune 中 **Endpoint Security | Attack Surface Reduction | Create Profile** 下的另一个配置文件设置，并选择 **App and browser isolation**。

此外，需要注意的是，如果你希望其他浏览器也能获得同等程度的保护，Application Guard 可作为 Firefox 和 Google Chrome 等浏览器的扩展使用。你可以在此处找到该扩展：`https://www.microsoft.com/security/blog/2019/05/23/new-browser-extensions-for-integrating-microsofts-hardware-based-isolation/`。

当你部署此设置时，它将配置独立模式下的 Application Guard 功能，该模式会在 Microsoft Edge 底部添加一个附加按钮，用户可以在需要时点击该按钮，在 Application Guard 中启动浏览器会话，如以下截图所示：

如果你拥有 Microsoft 365 E5 许可证并为端点启用了 Microsoft Defender for Endpoint，你还可以从 Microsoft 365 Defender 门户查看任何已发现的漏洞。通过访问 `security.microsoft.com`，并使用有权访问该服务的 Azure AD 管理员账户登录，然后进入 **Vulnerability management and weaknesses**，它将列出你环境中的所有软件和漏洞。

虽然 Microsoft 的更新通道和 Autopatch 可以管理 Windows 原生应用程序和产品，但它们不处理环境中可能存在的第三方应用程序的更新。要添加此功能，你需要使用诸如 PatchMyPC 之类的第三方产品，该产品支持与 Intune 集成，以处理这些应用程序的补丁管理。

## 保护 Microsoft Office 应用

Microsoft Office 应用程序是任何企业的重要方面，许多企业每天都会收到网络钓鱼尝试或作为电子邮件附件附带的恶意内容。

如前所述，在我讨论一些不同的勒索软件攻击时，其中许多都是从一封带有文档的电子邮件开始的，当用户运行文档中的代码时，攻击者便获得了立足点。一个例子是一种名为 Locky 的勒索软件变种，它使用包含带有恶意内容的 Word 文档的网络钓鱼电子邮件。这些文档嵌入了宏，因此一旦用户单击启用内容，恶意软件可执行文件就会从 Web 服务器下载额外的载荷，并开始查找要加密的附加驱动器和网络共享。

因此，为我们的 Office 应用程序设置一些额外的安全机制也是降低勒索软件攻击风险的一个重要方面。

在管理 Microsoft Office 应用程序时，通常可以使用组策略设置来完成，这些设置可以在 https://www.microsoft.com/en-us/download/details.aspx?id=49030 找到。

组策略设置是在任何基于 AD 的部署中管理配置的最常见方式。我们还可以使用 Office 365 管理中心中新的基于云的配置服务，其优势在于，无论设备是否完全受管理，都可以使用它们来管理 Office 365。这意味着，如果用户使用其 Azure AD 账户登录，策略将自动应用于应用程序。

此服务的另一个优点是，当 Microsoft 创建了额外的策略时，它们会自动在服务中可用，因此你无需下载 ADMX 即可确保新策略得以应用。

需要注意的是，只有基于用户的策略可用。如果你想应用基于计算机的设置，可以通过 Intune 使用 ADMX 部署模板。

Office 365 配置服务可在 https://config.office.com/officeSettings/officePolicies 找到，我将以此为例，为我们的用户构建 Office 应用程序的安全基线。

在配置门户中，转到 **Customization | Policy Management**，然后单击 **Create** 启动向导，如下面的截图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_129_0.png)

图 4.9 – Microsoft Office 365 配置服务

为配置文件提供一个名称，单击 **Next**，然后选择一个范围。在这里，我们需要定义一个 Azure AD 安全组，该组包含一个或多个我们希望应用这些设置的用户。然后进入 **Policies**，在这里我们将看到所有可供我们为用户配置的策略列表，甚至在顶部还有一个显示 **Baseline** 的筛选器。在这里，基线筛选器将显示属于 Microsoft 安全基线一部分的策略，并将成为我们的起点。

这些设置中有许多适用于单个应用程序或整个 Office 套件，你可以在 *Application* 下的图标用法中看到这一点，如下面的截图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_129_1.png)

图 4.10 – 在 Microsoft Office 配置中定义受信任位置

这里，我们有一个适用于 Microsoft Excel 的策略，另一个适用于 Office 套件的策略，后者随后应用于所有 Office 应用程序。在门户顶部，我们还有一个基线筛选器，用于筛选出 Microsoft 作为其安全基线一部分的所有策略。

总体而言，禁用宏可能会影响使用该功能的最终用户，尤其是在 Microsoft Excel 等应用程序中。因此，你可能需要允许宏从特定的网络位置运行。

当我们将网络位置指定为 Office 中的受信任位置时，这也意味着 Application Guard 等功能将不会运行。

需要注意的是，Microsoft 已对所有从互联网下载的包含宏的文件进行了一些更改，这些文件默认将被禁用。你可以访问 https://docs.microsoft.com/en-gb/DeployOffice/security/internet-macros-blocked 阅读有关此近期更改的更多信息。

## 保护 Web 浏览器

用户最常用的应用程序是 Web 浏览器；虽然大多数现代 Web 浏览器都具有较高安全级别，但仍存在一些与漏洞相关的挑战。Google Chrome 和 Microsoft Edge 都基于 Chromium 内核构建，它们曾因漏洞数量众多而陷入困境，你可以在 https://www.cisecurity.org/advisory/multiple-vulnerabilities-in-google-chrome-could-allow-for-arbitrary-code-execution_2022-001 看到这一点。

虽然这两种浏览器都支持自动更新，但通常需要重启浏览器才能正确安装更新。我在众多项目中看到的一件事是，最终用户通常不太愿意重启浏览器；因此，你还需要实施一些功能来确保这些更新得以安装。

Web 浏览器另一个需要考虑的重要方面是扩展。虽然许多扩展可以提供更多安全功能，例如弹窗拦截器或广告拦截器，但也有许多已知的恶意扩展被用来在本地计算机上收集用户名和密码。因此，在托管端点上控制扩展非常重要。

对于 Google Chrome 和 Microsoft Edge，我们都可以通过集中管理工具（如 Intune 或组策略）来管理扩展和更新机制。Microsoft 和 Google 都有各自的 ADMX 文件，可用于通过 AD 和组策略来管理浏览器。在此，我将使用带有 ADMX 功能的 Microsoft Intune 来部署 Microsoft Edge 设置。

> 注意
> 你可以在 https://docs.microsoft.com/en-us/deployedge/microsoft-edge-update-policies 查看 Microsoft Edge 的最新策略设置列表。

在 Intune 中部署 Microsoft Edge 设置是通过端点门户完成的；转到 **Devices | Configuration profiles | Create a Profile**，然后在 **Profile type** 下选择 **Templates**。接着，选择 **Administrative Templates**，如下面的截图所示：

