```
gravatarURL += "&rating=r";
gravatarURL += "&size=80";
gravatarURL += "&default=" + defaultAvatar;
return gravatarURL;
}
```

The properties that we discussed are fairly easy to figure out. However, you can get a full listing of how to work with the Gravatar service here: http://site.gravatar.com/site/implement#section_1_1.

# Public profile

Now that we have figured out privacy, provided a way for our users to manage their profile data, and allowed them to upload and appropriately format their avatar, we need to provide our users with a page that displays all their data.

For the most part this is simply another "plumbing page", meaning that we are just displaying the data here. Nothing overly complex! So we will skip most of the legwork and get right to the nitty gritty, which is how our privacy stuff is handled.

In the Profile.aspx page, we have stored everything within the panels such as this:

```
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