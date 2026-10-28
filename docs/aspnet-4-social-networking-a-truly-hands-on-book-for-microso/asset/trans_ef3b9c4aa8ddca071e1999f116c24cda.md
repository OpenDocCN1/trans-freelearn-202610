

> 密码学是一门内容广泛的学科，超出了本书的范畴。不过，如果你想把 `System.Security.Cryptography.Rijndael` 理解得更透彻一些，本项目中所包含的 Cryptography 类带有详尽的注释！你可以在 **Common** 项目中找到这个类。

首先，让我们来看看我们是如何能够从一个 **string** 变量调用 `Encrypt()` 的。要实现这一点其实非常简单。尽管 **string** 类是密封的，意味着我们无法按常规方式对其进行扩展，但我们可以使用 .NET 中名为“扩展方法”的特性。

```csharp
//Fisharoo/Common/Extensions.cs
namespace Fisharoo.Common
{
    public static class Extensions
    {
        public static string Encrypt(this string s, string key)
        {
            return Cryptography.Encrypt(s, key);
        }
        public static string Decrypt(this string s, string key)
        {
            return Cryptography.Decrypt(s, key);
        }
    }
}
```

扩展方法允许我们在不影响类正常工作方式的前提下扩展一个类。实现方式是在一个静态类中定义一个静态方法。需要注意的一点是，该方法的第一个参数以目标类型开头，在这里就是 **string**。因此，我们实际上为 **string** 类定义了一个 `Encrypt()` 和 `Decrypt()` 方法。请注意，这个方法与你平时编写的方法之间唯一的区别，就是第一个参数前面的 **this** 引用。就是这么简单！

## 实现领域层

由于我们使用的是 Entity Framework，它拥有一些现在已经属于 .NET 框架一部分的功能；我们的业务层因此被大大简化了。我们可以回想一下，在之前的应用程序中，我们使用了各种不同的数据访问层，这要求我们花费大量时间在数据库中编写 SQL、连接逻辑、提供程序，以及对象的实例化和持久化逻辑。除此之外，我们还需要定义业务对象。在这些对象中，有 95% 的逻辑仅仅是为了以更易于管理的方式传输数据。

有了 Entity Framework，这些繁琐的工作大多都消失了！我们现在有了完全生成的类来负责传输数据。但如果我们还需要自定义逻辑该怎么办呢？虽然我们可以简单地将逻辑添加到生成的类中，但这并不是最佳途径。下次我们做修改时，就必须重新生成类。这会导致所有自定义功能丢失。

幸运的是，生成的类是分部类。这意味着我们可以在同一个命名空间中创建一个同名的新分部类文件，从而扩展我们生成的类。下面是我们自定义的 Account 对象，它扩展了生成的 Account 对象。

```csharp
//Fisharoo/DataAccess/Account.cs
namespace Fisharoo.DataAccess
{
    public partial class Account
    {
        private List<Permission> _permissions = new List<Permission>();
        public List<Permission> Permissions
        {
            get { return _permissions; }
        }

        public void AddPermission(Permission permission)
        {
            _permissions.Add(permission);
        }

        public bool HasPermission(string Name)
        {
            foreach (Permission p in _permissions)
            {
                if (p.Name == Name)
                    return true;
            }
            return false;
        }
    }
}
```

借助这个新的分部类，我们现在就可以扩展现有的已生成 Account 类。我们为 Account 类添加了一些重要功能。现在我们有了添加和检查权限的方法。我们还有一个返回权限列表的属性。

这可以轻松地应用于我们项目中的任何分部类！

视图通过定义视图的接口将自身的引用传递给 presenter。使用接口为我们提供了一种解耦的结构。这就允许我们在需要时轻松地替换 UI。只要 UI 恰当地实现了该接口，它就可以使用 presenter。

在这个场景中，presenter 扮演着控制器的角色。一旦它被启动并拥有了对 code-behind 的引用，它就能主动决定如何处理前端的事件。presenter 也是我们前端中唯一能够与领域逻辑（即模型）交互的部分。

在接下来的小节中，我们将讨论登录过程。这更多是为了说明 MVP 模式的运作方式，而不是关于 ASP.NET 本身，因为代码本身非常简单。

> 我们在创建 ASP.NET 4 应用程序（第 2 章）时创建了一个 **Account** 文件夹。它添加了与登录、用户注册、更改密码等相关的基文件，以及使用 ASP.NET 成员资格服务的代码。但是，由于我们希望为身份验证和授权编写自定义逻辑，我们删除了这个文件夹中的所有文件，并按照后面小节中讨论的方式编写了自定义逻辑。

## 视图

我们将从 ASP.NET 前端代码开始。它基本上定义了一个用户名和密码文本框，以及一个用于登录的按钮。它还有两个用于简单导航任务的链接按钮——一个用于转到密码恢复页面，另一个用于带你到注册页面。

```aspx
//Fisharoo/Web/Account/Login.aspx
<%@ Page Language="C#" MasterPageFile="~/Site.Master"
AutoEventWireup="true" CodeBehind="Login.aspx.cs"
Inherits="Fisharoo.Web.Account.Login" %>

<asp:Content ContentPlaceHolderID="Content" runat="server">
    <div class="divContainer">
        <div class="divContainerBox">
            <fieldset class="login">
                <legend>Enter your Username and Password</legend>
                <asp:Table CssClass="tableLogin" runat="server">
                    <asp:TableRow>
                        <asp:TableCell HorizontalAlign="Left">
                            <asp:Panel ID="Panel1"
DefaultButton="btnLogin" runat="server">
                                <p>
                                    <asp:Label ID="UserNameLabel"
```

## 用户账户

你会在上面注意到，在 Page_Load() 中我们初始化了 presenter。一旦我们启动了 presenter，我们立即通过 _presenter.Init(this) 方法调用将 code-behind 的引用传递给 presenter。你还应该注意到，code-behind 捕获了一个按钮点击事件。但这个方法所做的只是通知 presenter 需要执行 Login() 方法，并传递原始的 username 和 password 值。最后，请注意 code-behind 确实通过其 DisplayMessage() 方法实现了该接口。由于 presenter 可以访问 code-behind 类，它就能根据需要调用任何公共方法。

另一个需要注意的方面是，当你开始编写代码时，Visual Studio 会在文件顶部添加许多 using 语句。Visual Studio 2010 的重构支持提供了删除未使用 using 语句的功能。你可能会想尝试一下，以保持代码的组织性。

![](img/15a62e134135dde036b0ac3cbb01dab6_106_0.png)

我们这样做了，最终只剩下了三条 using 语句，而之前有十四条。

## Presenter

以下是 presenter 代码：

```csharp
//Fisharoo/Web/Account/Presenters/LoginPresenter.cs
namespace Fisharoo.Web.Account.Presenters
{
    public class LoginPresenter
    {
        private ILogin _view;
        [Import]
        private IAccountService _accountService;
        [Import]
        private IRedirector _redirector;
        public void Init(ILogin view)
        {
            _view = view;
            MEFManager.Compose(this);
        }
        public void Login(string username, string password)
        {
            string message = _accountService.Login(username,
                                                    password);
            _view.DisplayMessage(message);
        }
        public void GoToRegister()
        {
            _redirector.GoToAccountRegisterPage();
        }
        public void GoToRecoverPassword()
        {
            _redirector.GoToAccountRecoverPasswordPage();
        }
    }
}
```

请注意，在 **Init()** 方法中，presenter 使用 MEF 框架来设置它完成工作所需的对象。如果页面需要在初始加载时显示数据，就是在这里实现的。除了 presenter 的初始化之外，你还会注意到 presenter 还有其他三个方法：Login()、GoToRegister() 和 GoToRecoverPassword()。

Login() 方法处理 **View** 传递给它的按钮点击事件。请注意，即使在这里，我们也没有太多需要管理的逻辑。Presenter 会迅速将用户登录的职责转交给前面讨论过的 AccountService 对象。它只是期望从 AccountService 得到一个友好的消息，来描述登录过程进行得如何。我们知道，如果它收到了消息，那就意味着登录失败；否则 AccountService 会适当地重定向用户。一旦登录完成，presenter 就会使用视图的 DisplayMessage() 方法将状态告知用户。

GoToRegister() 和 GoToRecoverPassword() 方法只是利用 Redirector 对象将用户发送到站点上相应的页面。虽然这看起来可能有些极端，但请记住，它遵循了良好的设计原则。如果你在整个站点中遵循这一点，你将获得以下三个好处：

1.  你可以轻松地替换 UI，并以最小的努力获得相同的结果。
2.  由于你的重定向代码集中在一个地方，当多个链接使用相同的方法重定向到某个位置时，你可以在这个地方更改重定向，从而影响整个站点的所有链接。
3.  这有助于提高你站点的可测试性！

