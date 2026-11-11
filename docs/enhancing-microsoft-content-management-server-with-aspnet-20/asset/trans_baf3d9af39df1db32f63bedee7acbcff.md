
using System.Web;
using System.Web.Security;
using Microsoft.ContentManagement.Web.Security;

12. 修改命名空间声明为：

TropicalGreen.MCMSMembershipProviders

13. 修改类声明为：

public abstract class MCMSMembershipProviderBase :
    System.Web.Security.MembershipProvider

14. 在 MCMSMembershipProviderBase 类中实现以下方法。（为避免输入以下代码，您可以从 Packt 网站下载。）

public override bool ValidateUser(string strUsername,
    string strPassword)
{
    bool ValidUser = false;
    CmsAuthenticationTicket Ticket;
    Ticket = CmsFormsAuthentication.AuthenticateAsUser(strUsername, strPassword);
    if (Ticket != null)
    {
        // pass the CmsAuthenticationTicket to the Login Control
        HttpContext.Current.Items.Add("MCMS Ticket", Ticket);
        ValidUser = true;
    }
    return ValidUser;
}

public override bool EnablePasswordReset
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override bool EnablePasswordRetrieval
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override bool RequiresQuestionAndAnswer
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override string ApplicationName
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
    set { throw new Exception(
        "The method or operation is not implemented."); }
}
public override int MaxInvalidPasswordAttempts
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override int PasswordAttemptWindow
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}

public override bool RequiresUniqueEmail
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override MembershipPasswordFormat PasswordFormat
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override int MinRequiredPasswordLength
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override int MinRequiredNonAlphanumericCharacters
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override string PasswordStrengthRegularExpression
{
    get { throw new Exception(
        "The method or operation is not implemented."); }
}
public override MembershipUser CreateUser(string username,
    string password, string email,
    string passwordQuestion, string passwordAnswer,
    bool isApproved, object providerUserKey,
    out MembershipCreateStatus status)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override bool ChangePassword(string username,
    string oldPassword, string
    newPassword)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override string GetPassword(string username, string answer)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override string ResetPassword(string username, string answer)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override void UpdateUser(MembershipUser user)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override MembershipUserCollection GetAllUsers(int pageIndex,
    int pageSize, out int totalRecords)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override bool ChangePasswordQuestionAndAnswer(string username,
    string password, string newPasswordQuestion,
    string newPasswordAnswer)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override bool UnlockUser(string userName)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override MembershipUser GetUser(object providerUserKey,
                                    bool userIsOnline)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override MembershipUser GetUser(string username,
                                    bool userIsOnline)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override string GetUserNameByEmail(string email)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override bool DeleteUser(string username,
                                bool deleteAllRelatedData)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override int GetNumberOfUsersOnline()
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override MembershipUserCollection FindUsersByName(
                                    string usernameToMatch,
                                    int pageIndex,
                                    int pageSize, out int totalRecords)
{
    throw new Exception(
        "The method or operation is not implemented.");
}
public override MembershipUserCollection FindUsersByEmail(
                                    string emailToMatch,
int pageIndex,
int pageSize,
out int totalRecords)
{
    throw new Exception(
        "The method or operation is not implemented.");
}

> 请注意，许多方法在被调用时只是抛出异常。这些方法与功能（例如“password reset”）相关，仅与支持此类功能的自定义身份验证方案相关。

15. 保存并构建 **MCMSMembershipProviders** 项目。

现在让我们基于抽象类实现一个简单的 MCMS 成员资格提供程序，用于 Windows 账户，它在调用基类中的 ValidateUser() 方法之前仅正确格式化用户名。

- 1. 右键单击 **MCMSMembershipProviders** 项目并点击 **添加**，然后点击 **新建项目...**。在 **添加新项目** 对话框中，选择 **类** 图标，并在 **名称** 文本框中输入 **MCMSWindowsMembershipProvider**。点击 **确定**。
- 2. 修改命名空间声明为：
  `TropicalGreen.MCMSMembershipProviders`
- 3. 修改类声明为：
  `public class McmsWindowsMembershipProvider : MCMSMembershipProviderBase`
- 4. 在 `McmsWindowsMembershipProvider` 类中实现以下内容：

public override bool ValidateUser
    (string strName, string strPassword)
{
    string strLogin = "WinNT:///" + strName.Replace("\\", "/");
    return base.ValidateUser(strLogin, strPassword);
}

> 为简洁起见，此示例代码不包含任何输入验证或异常处理，并假设 `strName` 的格式为 `MachineOrDomainName\username`。对于生产应用程序，我们将实现异常处理和输入验证。

5. 保存并构建 **MCMSMembershipProviders** 项目。

## 使用 MCMS 成员资格提供程序

现在我们已经创建了 MCMS 成员资格提供程序，需要将其注册到我们的 Tropical Green MCMS 应用程序中。

- 1. 右键单击 **TropicalGreen** 网站并点击 **添加引用...**
- 2. 在 **添加引用** 对话框中，点击 **项目** 选项卡。
- 3. 点击 **MCMSMembershipProviders** 并点击 **确定**。
- 4. 打开 `web.config` 文件。

5. 在现有的 `<authentication />` 标签正下方添加以下部分：

<membership defaultProvider="McmsWindowsMembershipProvider" userIsOnlineTimeWindow="30">
  <providers>
    <add name="McmsWindowsMembershipProvider"
         type="TropicalGreen.MCMSMembershipProviders.McmsWindowsMembershipProvider"
         applicationName="/" />
  </providers>
</membership>

> 请注意，`<add>` 元素的 `type` 属性需要在 一行中输入，且不能有空格或换行。上面的格式仅用于打印。
