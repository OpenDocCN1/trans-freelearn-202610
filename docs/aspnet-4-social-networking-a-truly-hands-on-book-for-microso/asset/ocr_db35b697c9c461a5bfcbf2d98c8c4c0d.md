# User Interactivity

```
```aspx
<asp:Literal Visible="false" ID="litFileID" Text="<%#((PEFile)Container.DataItem).FileID %>" runat="server"></asp:Literal>
<asp:Literal Visible="false" ID="litImageName" runat="server" Text="<%#((PEFile)Container.DataItem).FileSystemName %>"></asp:Literal>
<asp:Literal Visible="false" ID="litFileExtension" runat="server" Text="<%# ((PEFile)Container.DataItem).Extension %>"></asp:Literal><br />
</li>
```
```

Once we have the controls physically plugged into the page, we would want to set their **SystemObjectID** to 5, which is the **File** object (in our database).

We would also want to set the **SystemObjectRecordID** to the **FileID** of the current iteration. Then for the Tags control we would want to set the **Display** property **ShowCloudAndTagBox**, which will allow our users to see the already assigned tags and also add their own tags.

# Forums UI

The other set of controls (Votes, MarkAnswer and Medals) will be useful in the forums pages like ViewForum.aspx and ViewPost.aspx. We will look at ViewPost.aspx here as all three controls are used in it. You can look at the associated code to see changes made for ViewForum.aspx, the code behind, and the presenter as well.

As done with photos UI, we first register the controls to the page:

```
```aspx
<%@ Register Src="~/UserControls/Votes.ascx" TagName="Votes"
TagPrefix="Fisharoo" %>
<%@ Register Src="~/UserControls/Medals.ascx" TagName="Medals"
TagPrefix="Fisharoo" %>
<%@ Register Src="~/UserControls/MarkAnswer.ascx" TagName="MarkAnswer"
TagPrefix="Fisharoo" %>
```
```

Next we plug them in at the appropriate places. The MarkAnswer and Votes controls are added just before the body of the post and the Medals are added next to the user details. Note the appropriate property bindings for these controls below:

```
```aspx
<ItemTemplate>
    <table style="background-color: #dddddd; vertical-align: top;
    text-align: left;width:100%">
        <tr><td rowspan="2" style="width:70px" >
            <div style="background-color:#ffffff">
                <Fisharoo:MarkAnswer ID="MarkAnswer1"
IsAnswer="<%#((PEBoardPost)Container.DataItem).IsAnswer %>"
PostID="<%#((PEBoardPost)Container.DataItem).PostID %>"
Visible="<%#((PEBoardPost)Container.DataItem).DisplayMarkAnswer %>">
```
```