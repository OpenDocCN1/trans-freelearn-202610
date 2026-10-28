
## User Accounts

The item we didn't cover above is the page that receives the click from the link in the email. This series of code is relatively simple. So to start, we are going to list all of the files in order of use (design, code behind, and presenter). Then we can discuss it.

```aspx
//Fisharoo/Web/Account/VerifyEmail.aspx
<%@ Page Language="C#" MasterPageFile="~/Site.Master"
AutoEventWireup="true" CodeBehind="VerifyEmail.aspx.cs"
Inherits="Fisharoo.Web.Account.VerifyEmail" %>
<asp:Content ContentPlaceHolderID="Content" runat="server">
    <asp:Label ID="lblMsg" runat="server" ForeColor="Red"></asp:Label>
</asp:Content>
```

```csharp
//Fisharoo/Web/Account/VerifyEmail.aspx.cs
namespace Fisharoo.Web.Account
{
    public partial class VerifyEmail : System.Web.UI.Page,
                                      IVerifyEmail
    {
        private VerifyEmailPresenter _presenter;
        protected void Page_Load(object sender, EventArgs e)
        {
            _presenter = new VerifyEmailPresenter();
            _presenter.Init(this);
        }
        public void ShowMessage(string Message)
        {
            lblMsg.Text = Message;
        }
    }
}
```

```csharp
//Fisharoo/Web/Account/Presenters/VerifyEmailPresenter.cs

using System.ComponentModel.Composition;
using Fisharoo.Common;
using Fisharoo.DataAccess.Interfaces;
using Fisharoo.Web.Account.Interfaces;

namespace Fisharoo.Web.Account.Presenters
{
    public class VerifyEmailPresenter
    {
        [Import]
        private IWebContext _webContext;
        [Import]
```

```csharp
_view = View;
}
public void RecoverPassword(string Email)
{
    Fisharoo.DataAccess.Account account =
    _accountRepository.GetAccountByEmail(Email);
    if(account != null)
    {
        _email.SendPasswordReminderEmail(account.Email,
            account.Password, account.Username);
        _view.ShowRecoverPasswordPanel(false);
        _view.ShowMessage("An email was sent to your account!");
    }
    else
    {
        _view.ShowRecoverPasswordPanel(true);
        _view.ShowMessage("We couldn't find the account you requested.");
    }
}
}
}
}
```

This page asks the user to provide their email address. It then looks up the account with that email address. If it finds the account it then uses the Email.SendPasswordReminderEmail() method to send the user's decrypted password to their email account.

The SendPasswordReminderEmail() method looks like this.

```csharp
//Fisharoo/Components/Email.cs
public void SendPasswordReminderEmail(string To,
    string EncryptedPassword, string Username)
{
    string Message = "Here is the password you requested: " +
        Cryptography.Decrypt(EncryptedPassword, Username);
    SendEmail(To, "", "Password Reminder", Message);
}
```

### Edit account

To save a bit of space we are going to forgo showing you the design, the code behind, and the interface portion of this code. It is just a basic form with text boxes and the like, and has the same validation requirements as the registration form did. It allows you to change the information and, on successfully saving, displays an appropriate message and prevents further editing (user can go to another page and come back here to edit again). There are a couple of aspects, however, in the UpdateAccount() method that we describe below.

```csharp
//Fisharoo/Web/Account/Presenters/EditAccountPresenter.cs

namespace Fisharoo.Web.Account.Presenters
{
    public class EditAccountPresenter
    {
        ...

        public void UpdateAccount(string OldPassword, string NewPassword, string UserName,
            string FirstName, string LastName, string Email,
            string ZipCode, DateTime BirthDate)
        {
            //verify that this user is the same as the logged in user
            if(OldPassword.Encrypt(OldPassword) == account.Password)
            {
                if (Email != _userSession.CurrentUser.Email)
                {
                    if (!_accountService.EmailInUse(Email))
                    {
                        account.Email = Email;
                        //TODO: for development disable sending emails, so setting the following to true
                        //in production this will be set to false
                        account.EmailVerified = true;
                        _email.SendEmailAddressVerificationEmail(account.Username, Email);
                    }
                    else
                    {
                        _view.ShowMessage(false, "The email you entered is already in our system!", account.UserName);
                        return;
                    }
                }

                if(!string.IsNullOrEmpty(NewPassword))
                    account.Password = NewPassword.Encrypt(NewPassword);

                account.FirstName = FirstName;
                account.LastName = LastName;

                account.Zip = ZipCode;
                account.BirthDate = BirthDate;

                _accountRepository.SaveAccount(account);
                _view.ShowMessage(true, "Your account has been updated!", account.UserName);
            }
            else
            {
                _view.ShowMessage(false, "The password you entered doesn't match your current password! Please try again.", account.UserName);
            }
        }
    }
}
```

One thing to notice with the presenter is that when it is first initialized, it loads the current user's account details and passes that data to the view for initial display. Then once the user edits their data, there are several validation steps that occur. The most important is that of the password and the email.

If the password is not changed, we want to make sure that we do not store an empty value to the system!

For the email, if a user changes it, we want to make sure that we resend the verification email again and flag the account as not having a validated email address.

Beyond that we are simply updating the account object via the AccountRepository.Save() method.

### Implementing security

Now that we have all of our plumbing in place, we are at a point that we can lock down our site. Up until now someone could go wherever they wanted to on the site and we would not be able to stop them at all!

### SiteMap

The primary .NET widget that we will use to lock down our site is the ASP.NET sitemap. This is a wonderful tool that can be used not only for security but also to display breadcrumb trails, your primary navigation, and many other useful page/file-oriented tasks.

A sitemap file is made up of several siteMapNodes. Each node contains things such as URL, title, description, and roles by default. You can also add your own custom attributes. In our site we will use attributes for identifying links that belong in the topnav, the footer nav, as well as allowing the siteMap to help us with each page's title. Our current siteMap looks like this:

```xml
//Fisharoo/Web/Web.Sitemap
<?xml version="1.0" encoding="utf-8" ?>
<siteMap xmlns="http://schemas.microsoft.com/AspNet/SiteMap-File-1.0" >
    <siteMapNode url="default.aspx" title="Home" description="Home page" pageTitle="Welcome to Fisharoo.com!"
                 roles="PUBLIC">

    <!-- TOP NAV NODES -->
        <siteMapNode url="/account/default.aspx" title="My Account"
                     description="" pageTitle="" roles="PUBLIC">
            <siteMapNode url="/account/EditAccount.aspx" title="Edit Account" description="" pageTitle="" roles="PUBLIC" />
            <siteMapNode url="/account/Login.aspx" title="Login"
                         description="" pageTitle="" roles="PUBLIC" />
            <siteMapNode url="/account/RecoverPassword.aspx"
                         title="Recover Password" description="Recover Your Password" pageTitle="Recover your password"
                         roles="PUBLIC" />
            <siteMapNode url="/account/Register.aspx" title="Register"
                         description="" pageTitle="" roles="PUBLIC" />
            <siteMapNode url="/account/VerifyEmail.aspx" title="Verify Email" description="Verify your email address"
                         pageTitle="Email Verification" roles="PUBLIC" />
            <siteMapNode url="/account/AccessDenied.aspx" title="Access Denied" description="Access Denied"
                         pageTitle="Access Denied" roles="PUBLIC" />
        </siteMapNode>
        <siteMapNode url="/profile/default.aspx" title="Profile"
                     description="" topnav="1" pageTitle=""
                     roles="PUBLIC"></siteMapNode>
        <siteMapNode url="/friends/default.aspx" title="Friends"
                     description="" topnav="1" pageTitle=""
                     roles="PUBLIC"></siteMapNode>
        <siteMapNode url="/mail/default.aspx" title="Mail"
```
