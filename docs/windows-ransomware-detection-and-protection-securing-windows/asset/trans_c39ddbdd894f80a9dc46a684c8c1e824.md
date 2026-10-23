
然而，即使凭据受 Credential Guard 保护，Mimikatz 仍然可以在用户登录时拦截其输入的凭据。这是因为当用户登录 Windows 机器时，凭据会临时提供给该机器以对用户进行身份验证。在此期间，Mimikatz 可以在凭据传递给 LSA 进程并受到 Credential Guard 保护之前将其拦截。

还有其他机制可用于阻止 Mimikatz 读取**本地安全机构服务器服务 (LSASS)** 的内存，例如使用内置的**攻击面减少 (ASR)** 规则：https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference?view=o365-worldwide#block-credential-stealing-from-the-windows-local-security-authority-subsystem （该规则可同时应用于 Windows Server 和 Windows 客户端）。

## 远程桌面管理

大多数 Windows 环境使用诸如 RDP 之类的工具进行远程管理。在使用 RDP 等功能时，建议之一就是启用**网络级身份验证 (NLA)**。NLA 在建立连接之前提供了额外的预身份验证层，并确保在用户成功验证之前不会暴露系统信息。我们可以通过打开**系统属性**并转到**远程**选项卡来验证机器上是否启用了 NLA，如下面的截图所示：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_257_0.png)

图 10.2 – RDP 的 NLA 设置

注意：我不建议使用 RDP 执行远程管理；你应该使用第三方服务或使用云服务，例如 Azure Bastion，或支持多重身份验证 (MFA) 的工具，例如 Windows Admin Center。还有其他新的替代方案，例如 Cloudflare Zero Trust 或 Hashicorp Boundary；这些选项也支持与 Azure AD 集成，以提供基于零信任的访问方法。

可以通过组策略为整个基础架构强制启用 NLA，导航到：

**计算机配置 | 策略 | 管理模板 | Windows 组件 | 远程桌面服务 | 远程桌面会话主机 | 安全 | 要求使用网络级身份验证对远程连接进行用户身份验证**

重要的是要意识到 NLA 使用**凭据安全支持提供程序 (CredSSP)** 从发起系统传输身份验证请求。这可能会导致安全问题，因为 CredSSP 可能会将凭据存储在发起系统的 LSA 内存中，即使用户已注销也是如此。

在内部使用 RDP 管理基础设施时，这可能会带来问题，因为凭据可能会在每个被访问的服务器上暴露。这是因为远程交互式登录会导致凭据与 Kerberos 票证和其他信息一起存储在 LSASS 内存中。

为了解决此问题，微软在 Windows 8.1 和 Windows Server 2012 R2 中引入了一项名为**受限管理员模式**的功能。此功能通过限制存储凭据的使用，有助于防范横向移动和传递哈希攻击。

启用此功能后，建立 RDP 会话无需提供密码；而是使用用户的哈希或 Kerberos 票证作为 RDP 连接的一部分进行身份验证。

讽刺的是，该功能也引入了一个新的攻击向量，我们可以利用它，即使用用户的哈希，通过 Mimikatz 之类的工具发起传递哈希攻击来对受限系统进行身份验证。例如，如果我有一个启用了受限管理员模式的系统，我可以使用 Mimikatz 的以下参数执行传递哈希攻击：

```
sekurlsa::pth /user:<user name> /domain:<domain name> /
ntlm:<the user's ntlm hash> /run:"mstsc.exe /restrictedadmin /v:<IP of the system>"
```

随后在 Windows 10 中，微软引入了一项名为**远程凭据保护**的新功能，这是 Windows 操作系统中的一项安全功能，旨在防范凭据盗窃攻击。它使用基于虚拟化的安全性来隔离机密（例如凭据），并为其存储和使用提供安全环境，同时也有助于防止传递哈希攻击。与受限管理员模式不同，此功能仅支持 Kerberos 协议。

对于帮助台支持场景，即人员需要管理访问权限以通过远程桌面会话向计算机用户提供远程协助时，微软建议在这种情况下不应使用 Windows Defender 远程凭据保护。这是因为如果向攻击者已经控制的受感染客户端发起 RDP 会话，攻击者可以利用该开放通道代表用户创建会话（无需破坏凭据），从而在会话断开后的一段时间（几小时）内访问用户的任何资源。

从 Windows 11 企业版 22H2 开始，与 Windows Defender 凭据保护兼容的系统将默认启用该功能。

更好的替代方案是确保我们制定了适当的特权访问策略，以保证特权帐户不会暴露或丢失。微软在这里为我们应如何规划特权帐户策略编写了一份很好的总结：https://learn.microsoft.com/en-us/security/compass/privileged-access-strategy。

但简而言之，诸如企业管理员或域管理员之类的特权帐户绝不应在非特权帐户使用的常规设备上使用。确保这些特权帐户仅用于**特权访问工作站 (PAW)**，这些工作站是以最高安全配置配置的机器。

你还应对这些特权帐户添加额外的监控，以确保它们只能由 PAW 运行。一个示例是使用**安全信息和事件管理 (SIEM)** 工具来监控那些特权帐户的使用情况，如微软的这篇文章中所述：https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/security-operations-privileged-accounts。这里还有一篇文章展示了如何使用 Microsoft Sentinel 监控紧急访问帐户的使用情况，其中许多相同的原则也可用于监控特权帐户：https://jeffreyappel.nl/monitor-azure-ad-break-glass-accounts-with-azure-sentinel/。

微软提供了一些非常好的示例，强调并展示了我们如何使用 Azure **Active Directory (AD)** 和 Intune 设计和部署 PAW 解决方案：https://learn.microsoft.com/en-us/security/compass/privileged-access-deployment。

## 管理共享

另一个经常被忽视的攻击向量是利用管理共享。某些勒索软件变体会尝试使用管理共享或隐藏的网络共享。例如，许多勒索软件攻击者使用 Invoke-ShareFinder 并配合特权帐户的管理共享来试图查找敏感信息，正如这份**数字取证与事件响应 (DFIR)** 报告中提到的由 Emotet 实施的案例：https://thedfirreport.com/2022/11/28/emotet-strikes-again-lnk-file-leads-to-domain-wide-ransomware/。

注意：禁用服务器上的管理共享，尤其是**域控制器 (DC)**，可能会对基于域的环境中系统的功能和操作产生重大影响。因此，如果你计划禁用此功能，务必谨慎行事。此外，如果你的环境中使用了 PsExec，禁用管理 (ADMIN$) 共享可能会限制该软件的功能。

禁用该功能的方法要么是添加注册表项，要么是禁用 `LanmanServer` 服务。你可以通过添加以下注册表值来禁用管理共享：

```
HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters
DWORD Name = "AutoShareServer" Value = "0"
```

然后从管理员命令提示符（例如 CMD 或 PowerShell）运行以下命令：

```
net stop server
net start server
```

请注意，此更改不适用于 IPC$ 共享或你手动创建的共享。

第二种方法是禁用 `LanmanServer` 服务，这将影响与网络上的其他设备共享文件和打印机的功能。微软的 Windows Server 安全指南中也提到不应禁用该服务，因此不建议这样做：https://learn.microsoft.com/en-us/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server。

另外，我越来越多地使用的一种技术是金丝雀令牌 (canary token)。金丝雀令牌是计算机安全中使用的一种蜜罐或诱饵系统。它是一种特殊类型的 URL 或文件，用于模拟攻击者可能感兴趣的目标，例如登录页面或敏感文档。当被访问时，金丝雀令牌会发送警报，使我们能够跟踪攻击来源并做出相应响应。金丝雀令牌背后的理念是提供攻击的早期预警，以便目标可以采取行动防止或减轻损害。

虽然该工具和**软件即服务 (SaaS)** 有很多选择，但这里有一个将某种金丝雀产品与 Microsoft Sentinel 结合使用的示例：https://learnsentinel.blog/2022/03/24/deception-in-microsoft-sentinel-with-thinkst-canaries/。

这些类型的工具在勒索软件攻击者试图在网络共享上查找敏感信息的初始阶段很有用，因此，为名为 secret 或 sensitive information 的文件设置一个金丝雀令牌可能会触发警报。
