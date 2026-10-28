

## SAML

SAML 可用于在 Splunk 平台上启用 SSO 身份验证方案，以便用户能够通过 SSO 身份识别登录。Splunk 平台支持 SAML 2.0 协议版本，并联系 IdP 以检索用户的 SAML 组信息。与 LDAP 类似，SAML 组由 Splunk 管理员预先映射到 Splunk 角色。

Splunk 提供了非常好的文档来配置基于 SAML 身份验证的 SSO。您可以参考此处的文档：https://tinyurl.com/294zrc8u。

## MFA

MFA 由 Splunk 平台通过 Duo 和 RSA 安全机制支持，如图 3.7 所示。此方法将允许用户使用多个服务对 Splunk 平台进行身份验证以增强安全性。

配置通过 Splunk Web 设置 | 身份验证方法 | 多因素身份验证菜单完成，选择其中一个单选按钮（Duo 或 RSA），然后单击“配置 Duo 安全”超链接，填写表单，然后单击“保存”按钮。

如果您想深入了解详细步骤，请按照 Splunk 文档链接：https://tinyurl.com/sv8rnwhb。

## 脚本化身份验证

Splunk 平台为诸如远程身份验证拨入用户服务（RADIUS）或可插拔身份验证模块（PAM）之类的外部系统提供基于脚本的身份验证方法。Splunk Web 中没有直接选项来配置它；但是，示例脚本可在 SPLUNK_HOME/shared/authScriptSamples 下获得，您可以在其中按照 README 文件中的说明进行操作。

## 总结

总之，角色、用户和身份验证方法共同保护 Splunk Enterprise 实例。我们首先了解了用户。他们是执行 Splunk 平台上所需操作和执行的首要实体。用户可以是管理员（admins）或由管理员创建的普通用户。我们通过 Splunk Web 界面和 Splunk CLI 了解了创建新用户。之后，我们研究了角色为用户提供了什么。

Splunk 遵循 RBAC 方法。角色由系统管理员创建和管理。Splunk 提供默认角色，并且可以通过继承它们通过新角色创建过程进行扩展。

将用户分配给特定角色是 Splunk 中常用的解决方案，用于限制他们对某些操作的访问、限制他们对资源的消耗以及控制他们对索引的访问。角色中

- 8. *选项 B* 是正确答案。Splunk 中没有 `roles.conf` 或 `capabilities.conf` 这样的文件。`authentication.conf` 包含与身份验证相关的设置。
- 9. *选项 C* 是正确答案。当在 Splunk 上本地添加用户时，他们不会针对 LDAP 进行身份验证，并且他们的凭据不会与 LDAP 同步。要启用 LDAP 身份验证和同步，用户需要分配给映射到 Splunk 角色的适当 LDAP 组。这确保了他们的身份验证是在 LDAP 目录中执行的，并且对他们的帐户的任何更改或更新都与 LDAP 服务器同步。
- 10. *选项 C* 是正确答案。例如 – `https://splunkhostname:8000/en-US/account/login?loginType=splunk.`

## Splunk Forwarder 管理

*forwarder* 名称在此时应该很熟悉，因为您已经阅读过两种类型的 forwarder：**通用 forwarder（UF）** 和 **重型 forwarder（HF）**。它们是为特定用例构建的两种不同的软件二进制文件。在本章中，您将了解更多关于 UFs 的信息，它们在大规模和复杂的 Splunk Enterprise 环境中通过 **部署服务器（DS）** 集中管理。

我们将首先了解 UFs 在整体 Splunk 部署架构中的目的，然后配置 DS，以及 UF 的安装和配置。配置包括将 UF 连接到外部索引器以转发数据，并使用 `deploymentclient.conf` 配置来从 DS 下载应用程序。最后，您将通过监控控制台应用程序了解监控转发器。

本章不仅对系统管理员很重要，对数据管理员也很重要。它涉及两种类型管理员之间的共同责任，包括安装 UF、DS 和索引器连接配置，这些都是数据管理员创建转发器上必要的数据输入以将数据摄取到索引器的先决条件。

在本章中，我们将涵盖以下主题：

- 介绍通用 forwarder
- 配置部署服务器
- 安装通用 forwarder
- 在通用 forwarder 上配置转发
- 配置 `deploymentclient`
- Forwarder 监控

![](img/5319cbd6f078c6cda7fac6347f77a07d_79_0.png)

图 4.1：源主机中的 UF 存在

在前面的图中，UF 安装在运行 Web 服务器的主机上。UF 旨在读取 Web 服务器日志并将其转发到索引器。

让我们通过 DS 的配置，然后安装和配置 UF。

## 配置部署服务器

DS 顾名思义，集中管理对转发器的部署。它可以部署到 UFs 和 HFs，并且在应用程序部署后可以选择性地重新启动它们。在本节中，您将了解 DS 的功能以及管理转发器所需的配置。

让我们回顾一下 DS 设置要求：

- DS 是 Splunk Enterprise 软件的实例，需要 Splunk Enterprise 许可证。
- 转发器，无论是 UFs 还是 HFs，都需要配置 deploymentclient.conf 文件以与 Splunk DS 建立连接。此配置文件使转发器能够“回拨”并与 DS 通信。通过手动编辑 deploymentclient.conf 文件或使用 Splunk CLI，转发器可以与 DS 建立所需的连接。
- 用于管理服务器类的 serverclass.conf 配置文件可以通过导航到设置 | 转发器管理菜单的 Splunk Web 界面进行设置。Splunk DS 上 serverclass.conf 文件的典型位置是 $SPLUNK_HOME/etc/system/local。有关 Splunk Web 界面的更多详细信息，请参阅图 4.2 和 4.3。
- Splunk 应用程序/插件可以按照 Splunk 应用程序结构自定义构建，也可以从 splunkbase.com 下载，在那里有数百个预构建的应用程序/插件。
- 需要部署到转发器的应用程序/插件放置在 `$SPLUNK_HOME/etc/deployment-apps` 目录中。
- 通过 DS 部署的应用程序存储在转发器客户端的 `$SPLUNK_HOME/etc/apps` 目录中。

让我们在下一节中查看 `serverclass` 的配置。

## 配置 serverclass

要通过 Splunk Web 转发器管理方法配置 DS，至少必须将一个应用程序复制到 `$SPLUNK_HOME/etc/deployment-apps` 目录；您可以从 splunkbase.com 下载任何示例应用程序。使用您的 Splunk 帐户登录 splunkbase.com 以下载应用程序。

复制应用程序到 `deployment-apps` 目录后，请按照以下步骤操作：

1. 复制应用程序后，以管理员身份登录 Splunk Web 并导航到 **设置 | 转发器管理**：

![](img/5319cbd6f078c6cda7fac6347f77a07d_80_0.png)

图 4.2：Splunk Web – 转发器管理

2. *图 4.2* 显示 **转发器管理** 和三个选项卡：**应用程序**、**服务器类** 和 **客户端**。第一次访问此区域时，不会存在服务器类（正如选项卡名称后的零所示），因此单击 **create one** 链接。
3. 该链接将带您进入一个页面，您可以在其中输入类的名称并选择要映射应用程序以进行部署的应用程序和客户端。*图 4.3* 显示了两个服务器类，**AppsForLinux** 和 **AppsForWindows**，并且 **应用程序** 计数为每个显示 **1**：

