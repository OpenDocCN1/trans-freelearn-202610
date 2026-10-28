
## 用户个人资料

### Gravatar

正如我们之前讨论过的，Gravatar 是一项允许你把头像集中存储的服务。它让你可以将一个头像与你的电子邮件地址关联起来，这样其他网站（比如我们的网站）之后就可以引用它。对于那些在互联网上到处都留有个人资料的用户来说，这非常棒！

为了让用户使用 Gravatar 服务，他们只需在上传头像界面的初始页面上勾选复选框即可。当他们点击**提交**时，我们会将他们的**个人资料**中的 UseGravatar 属性设置为 1。之后每当我们调用 **ProfileImage.aspx** 页面来显示头像时，都必须使用 Gravatar 而不是本地存储的头像。

为了调用 Gravatar 服务，我们必须创建一个 URL，其中包含以十六进制 MD5 哈希形式正确格式化的电子邮件地址。我们在 Cryptography 类中创建了一个方法，来为我们执行此转换。

```csharp
public static string CreateMD5Hash(string StringToHash)
{
    MD5 md5Hasher = MD5.Create();
    byte[] data =
        md5Hasher.ComputeHash(Encoding.Default.GetBytes(StringToHash));
    StringBuilder sBuilder = new StringBuilder();
    for (int i = 0; i < data.Length; i++)
    {
        sBuilder.Append(data[i].ToString("x2"));
    }
    return sBuilder.ToString();
}
```

接着，我们创建了一个简单的扩展方法，它以一种比直接调用 Cryptography 套件更简化的方式为我们处理转换。

```csharp
public static string ToMD5Hash(this string s)
{
    return Cryptography.CreateMD5Hash(s);
}
```

了解了这些背景后，让我们来看看这个方法。

```csharp
//Fisharoo/Web/Images/ProfileAvatar/ProfileImage.aspx.cs
public string GetGravatarURL()
{
    defaultAvatar = Server.UrlPathEncode(__webContext.RootUrl +
        "/images/ProfileAvatar/Male.jpg");
    gravatarURL = "http://www.gravatar.com/avatar.php?";
    gravatarURL += "gravatar_id=" + account.Email.ToMD5Hash();
    gravatarURL += "&rating=r";
    gravatarURL += "&size=80";
    gravatarURL += "&default=" + defaultAvatar;
    return gravatarURL;
}
```

我们讨论的这些属性相当容易理解。不过，你可以在这里获取有关如何使用 Gravatar 服务的完整说明：http://site.gravatar.com/site/implement#section_1_1。

### 公开个人资料

现在我们已经解决了隐私问题，为用户提供了管理其个人资料数据的方式，并允许他们上传和适当格式化头像，接下来我们需要为用户提供一个显示其所有数据的页面。

在很大程度上，这不过是另一个“管道页面”，也就是说我们只是在这里展示数据。一点也不复杂！因此，我们将跳过大部分基础工作，直接切入正题，也就是我们的隐私处理方式。

在 Profile.aspx 页面中，我们把所有内容都存放在类似这样的面板中：

```aspx
//Fisharoo/Web/Profiles/Profile.aspx
<asp:Panel ID="pnlPrivacyAccountInfo" runat="server">
    <fieldset>
        <div class="divContainerTitle"> Account Info</div>
        <table class="profileInfo">
            <tr> <td>Email: </td>
                <td><asp:Literal ID="litEmail" runat="server" /></td>
            </tr>
            <tr> <td>Zip:</td>
                <td><asp:Literal ID="litZip" runat="server" /></td>
            </tr>
            <tr> <td>Birthday: </td>
                <td><asp:Literal ID="litBirthDate" runat="server" />/</td>
            </tr>
            <tr> <td>Updated: </td>
                <td><asp:Literal ID="litLastUpdateDate" runat="server" />
                </td> </tr>
        </table> </fieldset>
</asp:Panel>
```

这让我们可以轻松地按需锁定某个数据区域（例如，为了隐藏数据）。在此页面的 presenter 中，我们有一个 TogglePrivacy() 方法。

```csharp
///Fisharoo/Web/Profiles/Presenters/ProfilePresenter.cs
private void TogglePrivacy()
{
    _view.pnlPrivacyIMVisible(_privacyService.ShouldShow((int)PrivacyFlagType.PrivacyFlagTypes.IM,_accountBeingViewed, _account, _privacyFlags));
    _view.pnlPrivacyAccountInfoVisible(_privacyService.ShouldShow((int)PrivacyFlagType.PrivacyFlagTypes.AccountInfo,_accountBeingViewed, _account, _privacyFlags));
    _view.pnlPrivacyTankInfoVisible(_privacyService.ShouldShow((int)PrivacyFlagType.PrivacyFlagTypes.TankInfo,_accountBeingViewed, _account, _privacyFlags));
}
```

这个方法会调用 PrivacyService.ShouldShow() 方法，该方法返回一个布尔值，指示该项是否应显示。这个布尔值会直接设置到视图中的 Panel.Visible 属性上，从而有效地显示或隐藏相关数据。

### 自定义主页

当我们说“自定义主页”时，实际上指的是以一种易于访问的方式呈现的公开个人资料页面。这实际上是一个花哨的 URL，例如 **http://www.fisharoo.com/profiles/atulgupta**。我们甚至可以更进一步，使用 **http://atulgupta.fisharoo.com** 或类似的 URL！但在我们的例子中，我们将坚持使用第一个示例。

为了实现这一点，我们将使用 ASP.NET 路由支持。ASP.NET 4 在很多方面改进了其 URL 路由功能。现在有了对 SEO 友好的 URL，ASP.NET 4 MVC 在很大程度上依赖这一功能。从核心来说，URL 路由是一项允许应用程序接受不映射到物理文件的 URL 的功能。我们上面提到的友好 URL（**http://www.fisharoo.com/profiles/atulgupta**）并不映射到服务器上的物理文件，但我们的应用程序仍然能够提供该 URL 的内容。

这部分的实现逻辑相当直接。我们首先需要定义一个我们希望应用程序接受为有效的 URL 模式。这个模式会被添加到 RouteTable 中。表中的条目还指定了当模式匹配时要重定向到哪个页面。最后，在特定页面中，我们查询传入该页面的值（类似于查询字符串参数），然后按照我们希望的方式继续处理。RouteTable 条目是在 Application_Start 方法中添加到 Global.asax.cs 文件中的。
