

6. 在使用你的域名注册商设置好所需的 DNS 记录后，你可能需要等待 DNS 记录更新。然后，你可以在 Azure 门户中单击 **验证**：

通过电子邮件共享这些设置
在你按照上述说明在域名注册商处配置好域之前，验证将不会成功。
[验证]

### 图 4.19 – Azure 门户中的域验证

7. 你将收到一则通知，指示域验证成功：

![](img/cf35f46a8dd5f11e58349c1d81611b75_145_0.png)

### 图 4.20 – 域名验证成功

8. 成功验证域后，你应通过单击 **设为主域** 将其设为 Azure 租户的主域：

![](img/cf35f46a8dd5f11e58349c1d81611b75_145_1.png)

### 图 4.21 – 为租户设置主域

- 9. 你将收到一则通知，指示该域已成为租户的 *主域*：

![](img/cf35f46a8dd5f11e58349c1d81611b75_146_0.png)

### 图 4.22 – 租户主域名称更改成功

- 10. 在 **自定义域名** 边栏中，你现在可以看到该域显示为 *主域*：

![](img/cf35f46a8dd5f11e58349c1d81611b75_146_1.png)

### 图 4.23 – Azure 门户中已更新的主域

- 11. 现在可以关闭 **自定义域名** 边栏，准备执行下一个任务。

## 任务 3 – 下载并安装 Azure AD Connect

在此任务中，我们将下载并配置 Azure AD Connect 工具。在此练习中，我们将遵循 PHS 目录同步的一个简单用例：

- 1. 从用于下载和安装该工具的服务器上，使用为租户分配 **全局管理员** 角色的账户登录到 *Azure 门户*。

- 2. 导航到 Azure Active Directory 并选择 Azure AD Connect。然后，单击下载 Azure AD Connect：

![](img/cf35f46a8dd5f11e58349c1d81611b75_147_0.png)

### 图 4.24 – 预配 Azure AD Connect

- 3. 查看“详细信息 | 系统要求 | 安装说明”下的信息，然后单击下载：

![](img/cf35f46a8dd5f11e58349c1d81611b75_147_1.png)

### 图 4.25 – 下载 Azure AD Connect

4. 导航到下载位置，单击 AzureADConnect 安装程序以开始安装：

![](img/cf35f46a8dd5f11e58349c1d81611b75_148_0.png)

### 图 4.26 – AzureADConnect 安装程序

5. 查看“欢迎使用 Azure AD Connect”页面信息，确认选中“我同意许可条款和隐私声明”复选框，然后单击“继续”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_148_1.png)

### 图 4.27 – Azure AD Connect 欢迎屏幕

6. 为此练习，在“快速设置”屏幕上单击“使用快速设置”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_149_0.png)

### 图 4.28 – Azure AD Connect 快速设置

7. 查看“连接到 Azure AD”屏幕上的帮助信息并输入用户名和密码。然后，单击“下一步”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_149_1.png)

### 图 4.29 – 连接到 Azure AD

- 8. 查看“连接到 AD DS”屏幕上的帮助信息并输入用户名和密码。然后，单击“下一步”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_150_0.png)

### 图 4.30 – 连接到 AD DS

- 9. 查看“准备配置”屏幕上的信息，然后单击“安装”。配置将开始并显示进度：

![](img/cf35f46a8dd5f11e58349c1d81611b75_150_1.png)

### 图 4.31 – 配置进行中

10. 查看“配置完成”屏幕上的信息，然后单击“退出”：

![](img/cf35f46a8dd5f11e58349c1d81611b75_151_0.png)

### 图 4.32 – Azure AD Connect 配置完成

11. 登录 Azure 门户，导航到 Azure Active Directory，并选择 Azure AD Connect。你将看到“同步状态”和“密码哈希同步”选项现在显示为“已启用”，并提供了上次同步的时间。在 Azure AD 门户中同步 AD 对象最多可能需要 15 分钟才能显示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_151_1.png)

### 图 4.33 – Azure AD Connect 的预配状态

在下一个任务中，我们将查看如何验证同步。

## 任务 4 – 验证同步

此任务介绍如何使用同步服务管理器验证目录同步：

- 1. 从安装了 AD Connect 的服务器上，单击“开始”菜单，并从 Azure AD Connect 菜单项中选择同步服务管理器。

- 2. 在同步服务管理器中，从“操作”选项卡，查看连接器操作和同步统计信息的任务。如下图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_152_0.png)

### 图 4.34 – 同步服务管理器操作

- 3. 单击“连接器”选项卡，查看两种连接器类型以及可以采取的操作。一个连接器用于 AD DS 重点，另一个用于 Azure AD 重点。如下图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_153_0.png)

### 图 4.35 – 同步服务管理器连接器

- 4. 现在可以关闭 **同步服务管理器**。

- 5. 你还可以使用以下 PowerShell cmdlet 命令同步更改：

```
Start-ADSyncSyncCycle
```

在下一个任务中，我们将查看用于管理已目录同步的 Azure AD 用户的任务。

用户 | 所有用户 ... milesbettersolutions.onmicrosoft.com > Azure Active Directory

![](img/cf35f46a8dd5f11e58349c1d81611b75_153_1.png)

### 图 4.36 – 已目录同步的用户

7. 单击用户帐户，并注意该用户的 UPN 对本地保持不变，从而提供具有相同 SSO 功能的通用标识，以使用相同的 UPN 访问 AD 经过身份验证的资源和服务或 Azure AD 经过身份验证的资源和服务：

![](img/cf35f46a8dd5f11e58349c1d81611b75_154_0.png)

### 图 4.37 – 用户帐户 UPN

8. 请注意，你无法编辑此已目录同步用户的任何属性。所有更改都必须在 AD 中的对象上进行，并允许同步更新 Azure AD 中的属性：

![](img/cf35f46a8dd5f11e58349c1d81611b75_154_1.png)

### 图 4.38 – 编辑用户属性

9. 请注意，要使用 **重置密码** 选项，必须已启用 SSPR 写回：

![](img/cf35f46a8dd5f11e58349c1d81611b75_155_0.png)

### 图 4.39 – 重置密码

10. 一旦用户帐户同步到 Azure AD，你可以根据需要 *分配许可证*：

![](img/cf35f46a8dd5f11e58349c1d81611b75_155_1.png)

### 图 4.40 – 分配用户许可证

借此，我们已安装 Azure AD Connect 并将 AD 对象与 Azure AD 同步。此练习教授了实现 AD Connect 所需的技能。现在，让我们总结本章。

## 摘要

本章涵盖了考试 AZ-800 管理 Windows Server 混合核心基础设施：实现和管理混合标识。

从本章中需要记住的关键课程是理解混合标识的概念，如何实现和管理 Azure AD Connect，云同步之间的差异，不同的混合标识身份验证方法，以及根据需求何时在它们之间进行选择。

然后，我们通过一个模拟真实场景的实践练习结束了本章，以进一步发展我们的技能。

你将通过所提供的信息获得新技能，本章的目标是让你超越考试目标，为真实世界中以日常混合环境为重点的角色做好准备。

下一章将教授有关实现和管理本地网络基础设施的内容。

## 延伸阅读

本部分提供有关考试和学习参考的其他链接：

- 微软认证：Windows Server 混合管理员专家: https://docs.microsoft.com/en-us/learn/certifications/windows-server-hybrid-administrator/
- 考试 AZ-800：管理 Windows Server 混合核心基础设施: https://docs.microsoft.com/en-us/learn/certifications/exams/az-800
- 考试 AZ-800：学习指南: https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWKI0r
- Microsoft Learn: https://docs.microsoft.com/en-gb/learn/modules/implement-hybrid-identity-windows-server/

## 技能检查

用本章所学内容挑战自己：

- 1. 两个 Microsoft IDP 是什么？
- 2. 什么是混合标识？
- 3. 什么是 Azure AD Connect？
- 4. 什么是 Azure AD Connect 云同步？
- 5. 安装 Azure AD Connect 时有哪些操作？

