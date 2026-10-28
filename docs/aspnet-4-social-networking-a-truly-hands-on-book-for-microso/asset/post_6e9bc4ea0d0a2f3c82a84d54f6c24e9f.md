
## User Profiles

### Gravatar

As we discussed earlier, Gravatar is a service that allows you to store your avatars in a central place. It allows you to associate an avatar to your email address so that other sites (like ours) can reference it later. This is great for a user who has profiles scattered all over the Internet!

In order for our users to use the Gravatar service, all they have to do is check the box on the initial page of the upload avatar screen. When they hit **submit**, we set the UseGravatar property of their **Profile** to 1. Then whenever we call the **ProfileImage.aspx** page to display the avatar, we must use the Gravatar instead of the locally stored avatar.

In order to make a call into the Gravatar service, we have to create an URL with a properly formatted email address in the form of a hexadecimal MD5 hash. We created a method in our Cryptography class to perform this conversion for us.

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

We then created a simple extension method that handles the conversion for us in a more simplified manner than calling into the Cryptography suite directly.

```csharp
public static string ToMD5Hash(this string s)
{
    return Cryptography.CreateMD5Hash(s);
}
```

With this background let's see the method.

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

The properties that we discussed are fairly easy to figure out. However, you can get a full listing of how to work with the Gravatar service here: http://site.gravatar.com/site/implement#section_1_1.

### Public profile

Now that we have figured out privacy, provided a way for our users to manage their profile data, and allowed them to upload and appropriately format their avatar, we need to provide our users with a page that displays all their data.

For the most part this is simply another "plumbing page", meaning that we are just displaying the data here. Nothing overly complex! So we will skip most of the legwork and get right to the nitty gritty, which is how our privacy stuff is handled.

In the Profile.aspx page, we have stored everything within the panels such as this:

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

This allows us to easily lock down an area of data as needed (to hide for example). In the presenter for this page, we have a TogglePrivacy() method.

```csharp
///Fisharoo/Web/Profiles/Presenters/ProfilePresenter.cs
private void TogglePrivacy()
{
    _view.pnlPrivacyIMVisible(_privacyService.ShouldShow((int)PrivacyFlagType.PrivacyFlagTypes.IM,_accountBeingViewed, _account, _privacyFlags));
    _view.pnlPrivacyAccountInfoVisible(_privacyService.ShouldShow((int)PrivacyFlagType.PrivacyFlagTypes.AccountInfo,_accountBeingViewed, _account, _privacyFlags));
    _view.pnlPrivacyTankInfoVisible(_privacyService.ShouldShow((int)PrivacyFlagType.PrivacyFlagTypes.TankInfo,_accountBeingViewed, _account, _privacyFlags));
}
```

This method makes a call to the PrivacyService.ShouldShow() method, that returns a Boolean value of whether or not the item should be displayed. This Boolean value is directly set to the Panel.Visible property in the view that effectively shows or hides the data in question.

### Custom homepage

When we say "custom homepage", we are really referring to the public profile page in an easy to get manner. This is really a fancy URL, something like **http://www.fisharoo.com/profiles/atulgupta**. We could even take it as far as **http://atulgupta.fisharoo.com** or something along those lines! We will however stick with the first example in our case.

To implement this we will use ASP.NET Routing support. ASP.NET 4 improves on its URL routing feature in a number of ways. There are now SEO-friendly URLs and ASP.NET 4 MVC relies heavily on this feature. At the very core, URL routing is a feature that allows your application to accept URLs that do not map to physical files. The friendly URL that we mentioned above (**http://www.fisharoo.com/profiles/atulgupta**) doesn't map to a physical file on the server, yet our application is capable of serving it.

Implementation logic for this is fairly straightforward. We need to first define an URL pattern that we would like to accept as valid for our application. This is added to a RouteTable. The entry in the table also specifies which page to redirect to in case the pattern matches. Finally in the specific page, we query for the values that were passed in to the page (similar to query string parameters) and then go ahead and process in the way we want to. The RouteTable entry is added to the Global.asax.cs file in Application_Start method.
