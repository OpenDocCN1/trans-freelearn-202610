
然后，Sysmon 将开始监视这些更改，并将它们报告到 Windows 事件查看器中的“应用程序和服务 | Microsoft | Windows | Sysmon | 操作”下。

然后，可以使用集中式日志服务（如 Azure Log Analytics 配合 Microsoft Sentinel，或 ELK 或 Graylog 等第三方工具）收集这些日志。这使我们能够监视这些二进制文件的使用情况。我倾向于使用这种方法从端点收集日志，并结合 Sentinel 中的分析规则来监视端点上这些二进制文件的使用情况。

第二种选择是使用 EDR 工具，例如 Microsoft Defender for Endpoint，它也会收集端点上所有进程的使用情况，然后你可以为此定义自定义监控规则。这需要 Microsoft E5 许可证才能使用。

此外，我们可以使用 Kusto 查询来查找这些 LOLBAS 可执行文件的使用情况，查询示例如下：

```
DeviceProcessEvents
| where Timestamp > ago(7d)
and InitiatingProcessFileName =~ 'mshta.exe'
```

上述查询查看 Microsoft Defender for Endpoint 中的 DeviceProcessEvents 表。DeviceProcessEvents 表存储了端点上运行过的所有进程的信息。这使我们能够列出所有 LOLBAS 可执行文件，甚至制定自定义检测规则。

## 默认应用程序

最容易进行的自定义操作之一是更改端点上的默认文件关联。例如，HTML 应用程序和 JavaScript 文件默认情况下会在 Web 浏览器中打开。这些文件可能被用来传递恶意软件，因此一个简单的修复方法是确保这些类型的文件默认不在浏览器中打开（因为浏览器会在系统上下文中运行代码），而是使用记事本打开。

我们应该更改文件类型关联的一些文件包括以下这些：

- .bat、.hta、.js、.jse、.jsc、.sct、.slk、.vb、.vbe、.vbs、.wsc、.wsf 和 .wsh

如果使用 Intune，没有直接更改此设置的直接方法。首先，你需要一台参考计算机，在其中定义这些文件类型应设置为使用记事本打开。然后，你需要使用以下命令导出该文件类型关联：

```
Dism /Online /Export-DefaultAppAssociations:"<pathname/filename.xml>"
```

然后，打开 XML 文件并复制其内容。接下来，访问 https://www.base64encode.org/ ，将内容粘贴进去，然后点击“Encode”按钮。

接下来，我们需要部署一个自定义 OMA-URI 配置。这是通过端点门户完成的，在 **设备 | 配置文件 | 创建配置文件** 下进行。在 **配置文件类型** 下选择 **模板**，然后从列表中选择 **自定义**，点击 **创建**。然后，如前面的示例一样，我们需要在 **配置设置** 下定义自定义设置。在这里，点击 **添加**，并在 **OMA-URI 设置** 下添加以下内容：

- **名称：** DefaultAssociationsConfiguration
- **OMA-URI：** ./Vendor/MSFT/Policy/Config/ApplicationDefaults/DefaultAssociationsConfiguration
- **值：** Base64 编码值

它应该类似于以下截图：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_140_0.png)

图 4.16 – Endpoint Manager 中的默认文件关联

端点策略更新后，应该会反映关联的文件类型。需要注意的是，这并不是一份详尽的安全设置列表，你可以在端点上配置和使用这些设置来提供更高级别的安全性。

我还想推荐一些其他文章和博客文章，你可以在其中阅读有关其他安全措施的更多内容，例如 https://adsecurity.org/?p=3299 。

## 保护用户身份

到目前为止，本章的重点一直是保护端点，端点通常是勒索软件攻击的起点。那么，如果端点仍然被攻破怎么办？我们也需要采取防护措施，确保我们的账户或凭据不会被攻击者用于在网络中进行横向移动。

另一个部分是确保我们使用的外部服务也受多重身份验证保护，以确保攻击者无法通过暴力攻击或简单的凭据注入来访问我们的服务。

最后，我们还将研究如何保护我们的 AD 域免受已知攻击。

微软之前曾表示，如果公司实施了**多重身份验证（MFA）**，99.9% 的基于身份的攻击都可以避免。因此，我们需要研究的第一件事是如何在我们的服务中实施通用的 MFA 服务。

需要注意的是，在以下示例中，我将使用 Azure MFA 和附加的 Azure AD 服务为服务提供 MFA，这要求你至少拥有 Azure AD P1 许可证才能使用该功能。

大多数公司都混合使用本地部署和基于云的服务，并且有不同的身份集成点，这可能使得为所有不同服务使用同一个 MFA 提供商有点困难。我看到的更常见的实施方案之一是公司拥有服务，如下面的截图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_141_0.png)

图 4.17 – 服务的 MFA 配置

例如，Office 365 直接与 Azure AD 集成，其他 SaaS 服务通常也支持 Azure AD 进行身份集成。

然后，我们还有本地部署的服务，例如 VPN 网关、Citrix 或基于 VMware 的 VDI 服务，或其他网络应用程序，我们需要以安全的方式通过 MFA 向最终用户提供这些服务。

使用 Azure AD 原生集成（如 SAML）的云服务意味着身份验证将在 Azure AD 中发生。因此，我们可以使用 MFA 服务——条件访问——来确保安全身份验证。

对于可能不直接支持 Azure AD 的本地资源，我们有几种选择。例如，如果我们有支持 RADIUS 的本地资源（大多数基于网络的服务都支持），我们可以使用一个名为 Windows NPS Extension for Azure MFA 的组件。这是一个软件组件，你将其安装在运行 NPS 的 Windows Server 上，然后它将充当 RADIUS 服务器，供与基于网络设备（如 Citrix NetScaler、Palo Alto VPN 等）集成的其他组件使用。

该服务也可以与使用 NPS 扩展的本地 RDS 网关部署一起使用。我不打算在这里介绍 NPS 扩展的安装，但你可以在 `https://docs.microsoft.com/en-us/azure/active-directory/authentication/how-to-mfa-nps-extension` 找到 Microsoft 关于如何设置它的文档。

请注意，使用 NPS 扩展通过 RADIUS 处理 MFA 提示并不支持条件访问提供的各种安全机制。为了确保与 Azure AD 及其安全机制的无缝集成，请确保所有实施的服务（无论是基于 Web、基于网络还是 VDI 服务）至少支持 SAML。

例如，以下是一个 HP Aruba 与 Azure AD 集成以实现 SAML SSO 的示例：`https://www.arubanetworks.com/techdocs/central/latest/content/nms/policy/ca-azure.htm`。

一旦你拥有与 Azure AD 集成的应用程序，就可以使用条件访问规则来定义在允许用户访问应用程序或服务之前需要满足的条件。

因此，让我们配置一个示例条件访问规则，要求所有用户使用 MFA 登录 Office 365。需要注意的是，条件访问要求你的组织启用 Azure AD P1 许可证，并且你拥有足够的权限来配置策略。

登录 Azure 门户，转到 **Azure Active Directory | 安全性** 选项卡，然后选择 **条件访问**。微软现在提供了预定义模板，我们可以使用这些模板更轻松地配置 MFA。但是，对于此示例，我们将使用默认向导。

点击 **新策略**，然后选择 **创建新策略**。

在每个条件访问中，我们有一组可以定义的分配和访问控制机制：
