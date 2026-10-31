
## API 和命令行权限

在 `XML API`/`REST API` 选项卡中，每个角色可以被授予或拒绝访问某些 API 调用的权限：

![](Images/B31589_02_33.png)

图 2.33：XML API/REST API

在 `Command Line` 选项卡中，每个角色可以被授予一定级别的访问权限或完全拒绝访问：

![](Images/B31589_02_34.png)

图 2.34：命令行权限

现在我们能够设置管理员账户，也应创建密码安全配置文件以防止密码管理不善。

## 密码安全

您需要通过前往 `Device` | `Password Profiles` 添加密码配置文件，以确保密码定期更改：

![](Images/B31589_02_35.png)

图 2.35：密码配置文件

`Password Profiles` 中的可配置设置如下：

* The change period indicates how long a password is valid
* 更改周期表示密码的有效时长
* The expiration warning pops up a warning when an admin logs on if their password is about to expire
* 到期警告在管理员登录时，如果密码即将过期则弹出警告
* The post-expiration login feature allows the admin to log on a certain number of times, even after their password has expired
* 过期后登录功能允许管理员在密码过期后仍可登录一定次数
* The post-expiration grace period indicates how long an admin will be able to log on after their account has expired before it is locked permanently and will require intervention from a different admin
* 过期后宽限期表示管理员在账户过期后仍可登录多长时间，之后账户将被永久锁定并需要其他管理员介入

此外，您还应通过 `Device` | `Setup` | `Management` | `Minimum Password Complexity` 为本地账户强制执行最小密码复杂度，以确保管理员不使用弱密码：

![](Images/B31589_02_36.png)

图 2.36：最小密码复杂度

NIST 提供了关于身份验证和生命周期管理的详尽指南，可在 [https://pages.nist.gov/800-63-3/sp800-63b.html](https://pages.nist.gov/800-63-3/sp800-63b.html) 找到。

现在让我们来看外部身份验证因素。

## 外部身份验证

最好使用 Kerberos、LDAP、RADIUS 或 SAML 等外部身份验证因素来在集中式系统中控制凭据，这使得管理员只需更改一次密码即可用于多个设备，或在他们离开组织时一次性被锁定在所有关键基础设施之外。

保留一个“应急”本地账户作为保障，以防管理界面完全失去对外部身份验证服务器的访问。此账户仅应在紧急情况下使用，并在账户用于登录时发送警报。有关如何设置在账户使用时发送警报的配置文件的说明，请参阅 *第 9 章*、*日志记录和报告*。

您首先需要从 `Device` | `Server Profiles` 菜单创建服务器配置文件。每种服务器类型都有自己的配置参数。以下配置文件可用：

* `TACACS+` (`Terminal Access Control Access Control System Plus`) is an authentication protocol developed by Cisco that is still used in many environments for terminal access authentication.
* `TACACS+` (`Terminal Access Control Access Control System Plus`) 是由 Cisco 开发的身份验证协议，至今仍在许多环境中用于终端访问身份验证。
* `LDAP` (`Lightweight Directory Access Protocol`) is probably one of the most commonly available protocols to authenticate against directory services and will work with Microsoft Active Directory, eDirectory, and custom LDAP servers.
* `LDAP` (`Lightweight Directory Access Protocol`) 可能是用于对目录服务进行身份验证的最常见协议之一，可与 Microsoft Active Directory、eDirectory 和自定义 LDAP 服务器配合使用。
* `RADIUS` (`Remote Authentication Dial-in User Service`) is an open standard authentication protocol that is widely used in remote access authentication.
* `RADIUS` (`Remote Authentication Dial-in User Service`) 是一种开放标准的身份验证协议，广泛用于远程访问身份验证。
* `Kerberos` is an authentication protocol that is mostly used for single sign-on and relies on a negotiation that does not require the exchange of passwords.
* `Kerberos` 是一种身份验证协议，主要用于单点登录，并依赖于无需交换密码的协商。
* `SAML` (`Security Assertion Markup Language`) uses an XML framework to exchange security information and is mostly used with cloud-based `identity providers` (`IdPs`). It integrates with `multi-factor authentication` (`MFA`) very easily.
* `SAML` (`Security Assertion Markup Language`) 使用 XML 框架交换安全信息，主要与基于云的 `identity providers` (`IdPs`) 配合使用。它能非常轻松地与 `multi-factor authentication` (`MFA`) 集成。
* Multi-factor profiles allow for several built-in MFA providers to be configured and added separately to an authentication profile.
* 多因素配置文件允许配置多个内置 MFA 提供程序并将其单独添加到身份验证配置文件中。

让我们来看一下这些配置文件的每个配置以及如何设置其基本配置。

### TACACS+ 服务器配置文件

TACACS+ requires you to choose between `Password Authentication Protocol` (`PAP`) and `Challenge-Handshake Authentication Protocol` (`CHAP`) and set the secret associated with connecting to the TACACS+ authentication server.

TACACS+ 要求您在 `Password Authentication Protocol` (`PAP`) 和 `Challenge-Handshake Authentication Protocol` (`CHAP`) 之间进行选择，并设置与连接 TACACS+ 身份验证服务器关联的密钥。

Optionally, you can set the profile so that it can only be used for administrator authentication, and set the profile to use a single session for all authentication events, rather than a new session per authentication event:

可选地，您可以设置配置文件使其仅用于管理员身份验证，并设置配置文件对所有身份验证事件使用单个会话，而不是每个身份验证事件使用新会话：

![](Images/B31589_02_37.png)

图 2.37：TACACS+ 服务器配置文件

While TACACS+ is somewhat rare, LDAP authentication is very common.

虽然 TACACS+ 有些罕见，但 LDAP 身份验证非常常见。

### LDAP 服务器配置文件

For an LDAP profile, you need to provide the type of the LDAP server, which can be `active-directory`, `E-directory`, `sun`, or `other`.

对于 LDAP 配置文件，您需要提供 LDAP 服务器的类型，可以是 `active-directory`、`E-directory`、`sun` 或 `other`。

You need to provide a `Base DN` value, which is the domain name (or the distinguished name) of the LDAP tree. The `Bind DN` field is for the user account that will be used to connect to the LDAP server and perform the request and its password. `Bind DN` can be fully qualified, as shown in the following screenshot, or be a `user principal name` (`UPN`) formatted as `user@domain`:

您需要提供 `Base DN` 值，即 LDAP 树的域名（或可分辨名称）。`Bind DN` 字段用于将用于连接到 LDAP 服务器并执行请求的用户账户及其密码。`Bind DN` 可以是完全限定的，如以下屏幕截图所示，或采用 `user principal name` (`UPN`) 格式为 `user@domain`：

![](Images/B31589_02_38.png)

图 2.38：LDAP 服务器配置文件

One thing to remember is that when you configure the server IPs and you have `Require SSL/TLS secured connection` enabled, the default port for LDAPS is `636`, rather than `389`.

需要注意的是，当您配置服务器 IP 且启用了 `Require SSL/TLS secured connection` 时，LDAPS 的默认端口为 `636`，而不是 `389`。

If your LDAP server has an externally signed certificate, enable `Verify Server Certificate for SSL sessions` to ensure the authenticity of your server. For the certificate check to work, the LDAP server root and intermediary certificates need to be in the device certificate store in `Device` | `Certificate Management` | `Certificates` | `Device Certificates`. The server name in the profile must match the `fully qualified domain name` (`FQDN`) certificate and `Subject AltName` attribute for this check to pass.

如果您的 LDAP 服务器具有外部签名的证书，请启用 `Verify Server Certificate for SSL sessions` 以确保服务器的真实性。要使证书检查生效，LDAP 服务器根证书和中间证书需要在 `Device` | `Certificate Management` | `Certificates` | `Device Certificates` 中的设备证书存储中。配置文件中的服务器名称必须与 `fully qualified domain name` (`FQDN`) 证书和 `Subject AltName` 属性匹配，才能通过此检查。

### RADIUS 服务器配置文件

RADIUS is one of the most popular authentication methods and supports the following authentication protocols:

RADIUS 是最受欢迎的身份验证方法之一，支持以下身份验证协议：

* PEAP-MSCHAPv2: `Protected Extensible Authentication Protocol` (`PEAP`) with Microsoft CHAP v2 provides improved security over PAP or CHAP by transmitting both the username and password in an encrypted tunnel.
* PEAP-MSCHAPv2：`Protected Extensible Authentication Protocol` (`PEAP`) 与 Microsoft CHAP v2 通过在加密隧道中传输用户名和密码，提供比 PAP 或 CHAP 更高的安全性。
* PEAP with `Generic Token Card` (`GTC`): PEAP with GTC enables the use of one-time tokens in an encrypted tunnel.
* PEAP with `Generic Token Card` (`GTC`)：PEAP with GTC 允许在加密隧道中使用一次性令牌。
* EAP-TTLS with PAP: `Extensible Authentication Protocol` (`EAP`) with `Tunneled Transport Layer Security` (`TTLS`) and PAP is used to transport plain text credentials for PAP in an encrypted tunnel. EAP-TTLS uses certificates to secure the connection and should be the preferred protocol as it is the most secure.
* EAP-TTLS with PAP：`Extensible Authentication Protocol` (`EAP`) 与 `Tunneled Transport Layer Security` (`TTLS`) 和 PAP 用于在加密隧道中传输 PAP 的明文凭据。EAP-TTLS 使用证书保护连接，应为首选协议，因为它是安全性最高的。
* CHAP: Used if the RADIUS server does not support EAP or PAP or is not configured for it.
* CHAP：如果 RADIUS 服务器不支持 EAP 或 PAP 或未为其配置，则使用。
* PAP: Used if the RADIUS server does not support EAP or CHAP or is not configured for it.
* PAP：如果 RADIUS 服务器不支持 EAP 或 CHAP 或未为其配置，则使用。

Palo Alto Networks uses vendor code `25461`.

Palo Alto Networks 使用厂商代码 `25461`。

Like the other profiles, RADIUS can be set so that it is only used for administrator authentication. The `Allow users to change passwords after expiry` option is limited to GlobalProtect users if the profile is also used to authenticate GlobalProtect inbound connections.

与其它配置文件类似，RADIUS 可以设置为仅用于管理员身份验证。如果配置文件还用于身份验证 GlobalProtect 入站连接，则 `Allow users to change passwords after expiry` 选项仅限于 GlobalProtect 用户。

The `Make Outer Identity Anonymous` option ensures the admin username is not visible for anyone sniffing the authentication sessions if PEAP-MSCHAPv2, PEAP with GTC, or EAP-TTLS are used and the server supports this:

`Make Outer Identity Anonymous` 选项可确保如果使用 PEAP-MSCHAPv2、PEAP with GTC 或 EAP-TTLS 且服务器支持此功能，则任何嗅探身份验证会话的人都无法看到管理员用户名：

![](Images/B31589_02_39.png)

图 2.39：RADIUS 服务器配置文件
