# # Tags Page

Now let's take a look at the tags page.

# ## Tags.ascx

From the client side, the tags control that we are creating appears to be very simple.
This control has two panels—one to show the tags and the other to collect them.

```
```aspx
//Fisharoo/Web/UserControls/Tags.ascx
<asp:UpdatePanel ID="UpdatePanel1" runat="server">
    <ContentTemplate>
        <asp:Panel runat="server" ID="pnlTag" Visible="false">
            <asp:TextBox ID="txtTag" runat="server"></asp:TextBox>
            <asp:Button CssClass="SiteButton" ID="btnTag" runat="server"
                        Text="Tag It!" OnClick="btnTag_Click" />
        </asp:Panel>
        <asp:Panel runat="server" ID="pnlTagCloud" Visible="false">
            <asp:PlaceHolder ID="phTagCloud" runat="server" />
        </asp:Panel>
    </ContentTemplate>
</asp:UpdatePanel>
```
```

# ## Tags.ascx.cs

In this file we have defined both a class and an enum. The enum of TagState is defined to modify the various states of the tag control. We have the following entries defined:

+   - **ShowCloud:** This option shows only the cloud for the object that the control is configured for.
+   - **ShowTagBox:** This option shows the text box to collect tags. It does not show any cloud.
+   - **ShowCloudAndTagBox:** This option shows both the cloud and the text box.
+   - **ShowParentCloud:** This option shows the cloud for a parent. This option can be used to show all the tags for something like a photo album.
+   - **ShowGlobalCloud:** This option shows all the tags from the site's point of view.

The Tags class implements a Display property, which is of type TagState (our previously mentioned enum). In the Page_Load() method, we initialize our TagsPresenter and in doing so set the state of the control and load the tags that were already collected.