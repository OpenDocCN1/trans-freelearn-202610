
6. 修改身份验证部分以使用表单身份验证：

```
<authentication mode="Forms">
  <forms loginUrl="Login.aspx" />
</authentication>
```

## 使用登录控件创建登录页面

现在让我们继续实现一个登录页面，允许用户登录，该页面将使用新的 ASP.NET 2.0 登录控件。

- 1. 右键单击 **TropicalGreen** 网站，然后单击 **添加新项**。
- 2. 选择 **Web Form** 图标，在 **Name** 文本框中输入 **Login.aspx** 并单击 **添加**。确保未选中 **选择母版页** 复选框。

![](img/18a0aa8d36427a4c069b30410f998b8e_185_0.png)

> 将 `Login` 控件添加到母版页是可行的。但是如果母版页包含与 PAPI 交互的代码（例如导航控件或发布属性），则您的 MCMS 安装需要启用访客访问。此类实现仅适用于使用表单身份验证的混合创作和只读托管场景。

- 3. 切换到 **设计** 视图。
- 4. 从工具箱 **Login** 选项卡拖动 **Login** 控件到设计表面。

## 身份验证控件和成员资格提供程序

![](img/18a0aa8d36427a4c069b30410f998b8e_186_0.png)

- 5. 如有需要，使用登录控件的自动格式任务选择格式方案。

![](img/18a0aa8d36427a4c069b30410f998b8e_186_1.png)

- 6. 单击登录控件（Login1），然后在属性窗口中单击事件图标（闪电符号）。

![](img/18a0aa8d36427a4c069b30410f998b8e_187_0.png)

- 7. 双击 LoggedIn 事件以添加 Login1_LoggedIn 事件处理程序并在代码视图中跳转到它。
- 8. 添加以下 using 语句：

```
using Microsoft.ContentManagement.Web.Security;
```

- 9. 在 Page_Load 方法正下方实现以下内容：

```
protected void Login1_LoggedIn(object sender, EventArgs e)
{
    Login myLogin = sender as Login;

    if (HttpContext.Current.Items.Contains("MCMSTicket"))
    {
        CmsAuthenticationTicket Ticket =
            HttpContext.Current.Items["MCMSTicket"]
            as CmsAuthenticationTicket;
        CmsFormsAuthentication.SetAuthCookie(Ticket,
            false, Login1.RememberMeSet);
    }
}
```

- 10. 从“生成”菜单中选择“生成网站”。

现在我们已完成登录页面，可以继续在 Internet Explorer 中通过浏览 http://localhost/tropicalgreen 来测试功能。

![](img/18a0aa8d36427a4c069b30410f998b8e_188_0.png)

- 11. 输入用户名和密码并单击 **登录**。请记住以 **MachineOrDomainName\username** 的格式输入用户名。
- 12. 如果凭据正确，您将登录并被带到 Tropical Green 主页。否则，`Login` 控件中将显示一条消息。

## 使用 LoginStatus 和 LoginName 控件

现在我们有了登录页面，可以使用 `LoginStatus` 和 `LoginName` 控件为模板提供额外的成员资格相关功能。让我们继续将这些控件添加到我们的母版页。

- 1. 在 **设计** 视图中打开 `TropicalGreen.master`。
- 2. 从工具箱的 **Login** 选项卡拖动 **LoginStatus** 控件到右侧表格单元格中，位于现有菜单控件 (**MenuRight**) 之上。
- 3. 单击 `LoginStatus` 控件 (**LoginStatus1**) 和 **属性** 窗口中的事件图标。
- 4. 双击 **LoggedOut** 事件以添加 `Login1_LoggedOut` 事件处理程序并在 **代码** 视图中跳转到它。
- 5. 添加以下 using 语句：

```
using Microsoft.ContentManagement.Web.Security;
```

- 6. 在 `Page_Load` 方法正下方实现以下内容：

```
protected void LoginStatus1_LoggedOut(object sender, EventArgs e)
{
    CmsFormsAuthentication.SignOut();
}
```

- 7. 保存您的工作，然后从 **生成** 菜单中选择 **生成网站**。

我们现在可以通过登录我们的网站来测试 `LoginStatus` 控件：

![](img/18a0aa8d36427a4c069b30410f998b8e_189_0.png)

## 身份验证控件和成员资格提供程序

当我们单击注销链接时，我们将从 MCMS 注销并返回到登录页面。如果在我们的 MCMS 安装上启用了访客访问，则 LoginStatus 控件将显示登录链接。

现在让我们继续向母版页添加 LoginName 控件：

- 1. 在 Design 视图中打开 TropicalGreen.master。
- 2. 从 Login 选项卡的工具箱拖动 LoginName 控件到右侧表格单元格中，位于现有 LoginStatus 控件之上。
- 3. 切换到源视图。
- 4. 在 <asp:LoginName /> 元素之后添加一个不间断空格：
- 5. 保存您的工作，然后从生成菜单中选择生成网站。

```
<asp:LoginName ID="LoginName1" runat="server"/>
<br/>
```

现在当我们登录时，可以看到当前登录用户的名称：

![](img/18a0aa8d36427a4c069b30410f998b8e_190_0.png)

我们可以使用上一章中详细介绍的皮肤轻松增强 LoginStatus 和 LoginName 控件的外观和感觉。

## 支持自定义身份验证方案

到目前为止，我们已经了解了如何通过为 Windows 帐户创建 MCMS 成员资格提供程序并将其与表单身份验证一起使用来使用新的 ASP.NET 2.0 登录控件。也可以使用相同的方法为“帐户映射”场景提供更优雅的解决方案，在该场景中身份验证针对外部用户存储进行。

例如，假设我们需要使用 Active Directory 应用程序模式 (ADAM) 进行身份验证，我们可以通过重写 ValidateUser() 方法基于我们的 MCMSMembershipProviderBase 类实现 ADAM 成员资格提供程序：

```
using System.DirectoryServices;
... Code Continues ...
public override bool ValidateUser(string strName, string strPassword)
{
    // the LDAP server to connect to
    string strLdapServer = "LDAP://localhost:389/";
    // the DN of the OU containing the user
    string strLdapObj = "OU=MCMSUsers,O=TropicalGreen";
    // the DirectoryEntry property to validate
    string strDECheck = "MCMSUsers";
    // the DN of the user we are connecting as
    string strBindName = "CN=" + strName + "," + strLdapObj;
    // the Windows Username format for passing to ValidateUser
    string strWindowsUser = "WinNT://webdev/" + strName;

    try
    {
        // Connect to ADAM using LDAP AuthN...
        DirectoryEntry ent = new DirectoryEntry(strLdapServer +
            strLdapObj, strBindName,
            strPassword, AuthenticationTypes.None);

        // We must bind to perform authentication
        if ((string)ent.Properties["name"].Value == strDECheck)
        {
            // LDAP AuthN succeeded, so perform Windows
            // AuthN to setup a MCMS context...
            return base.ValidateUser(strWindowsUser, strPassword);
        }
        // LDAP AuthN failed, so return a false
        return false;
    }
    catch (Exception)
    {
        //LDAP AuthN failed, so return a false
        return false;
    }
}
```

Active Directory 应用程序模式 (ADAM) 是一个独立的 LDAP 3 目录服务。上面的示例假定 ADAM 安装在与 MCMS 相同的机器上，已配置为允许不安全操作，MCMS 用户存储在名为 MCMSUsers 的组织单位中，并且密码在 ADAM 和 Active Directory 或本地安全帐户 (SAM) 数据库中相同。

上面的示例可以扩展以创建 Windows 和 ADAM 用户帐户，并为最终用户提供附加功能，例如密码重置。但是，仍然没有受支持的机制来管理 MCMS 角色组成员资格。一个合适的折中方案是为每个 MCMS 角色组创建一个 Windows 组。然后使用站点管理器将 Windows 组添加到 MCMS 角色组，并且用户可以以编程方式添加到 Windows 组。

## 摘要

我们已经介绍了如何通过为 Windows 帐户创建 MCMS 成员资格提供程序并将其与表单身份验证一起使用来使用新的 ASP.NET 2.0 登录控件和成员资格提供程序模型。我们还了解了哪些控件对 MCMS 应用程序有用以及如何将它们与我们的 MCMS 应用程序集成。我们还看到了如何通过为外部身份验证系统开发成员资格提供程序来利用这些 ASP.NET 2.0 功能来增强“帐户映射”场景。
