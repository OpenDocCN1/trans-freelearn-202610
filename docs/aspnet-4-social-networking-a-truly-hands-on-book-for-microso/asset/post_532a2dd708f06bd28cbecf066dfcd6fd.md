
## Comments.ascx

The comment control is responsible for taking in a user's comment for an object in the system. This control is relatively simple, if compared to the others that we will be creating. It has a text box and a button to take in the comment. It also has a PlaceHolder to contain all the comments that we collect over time.

```aspx
//Fisharoo/Web/UserControls/Comments.ascx
<asp:UpdatePanel runat="server">
    <ContentTemplate>
        <asp:Panel runat="server" ID="pnlComment">
            <asp:TextBox ID="txtComment" runat="server"></asp:TextBox>
            <asp:Button Text="Add Comment" ID="btnAddComment" runat="server" OnClick="btnAddComment_Click" />
            <asp:PlaceHolder ID="phComments" runat="server"></asp:PlaceHolder>
        </asp:Panel>
    </ContentTemplate>
</asp:UpdatePanel>
```

## Comments.ascx.cs

First, note that we are initializing the control in the overridden OnInit() method. This allows us to get the control's output into the ViewState so that we can access everything down the road.

There are two properties for this class—SystemObjectID, and SystemObjectRecordID. They allow us to know what data to save and load.

When the page is initialized, we check the state of the current user to see if they are logged in or not. We do not allow unauthorized users to post comments. If the user is logged in, we show the comment text box and button. If they are not, we hide those controls.

Next, we load the page. To load the page, we load the comments from the presenter. Then we have the event handler for capturing the added comments. We also have a method to toggle the visibility of the panel that shows the comments. There is also a method for clearing the panel of comments.

The most complicated method in this whole class is the LoadComments() method. This method is responsible for taking in a collection of comments, iterating through them, and adding them to the panel for display. Let's look at this method in particular:

```csharp
//Fisharoo/Web/UserControls/Comments.ascx.cs
public partial class Comments : System.Web.UI.UserControl, IComments
{
    ...
    public void LoadComments(List<Comment> comments)
    {
        if (comments.Count > 0)
        {
            phComments.Controls.Add(new LiteralControl("<table width=\"100%\">"));
            foreach (Comment comment in comments)
            {
                phComments.Controls.Add(new LiteralControl("<tr><td>"
                    + comment.CommentByUsername + " ("
                    + comment.CreateDate.ToShortDateString() + "): "
                    + comment.Body + "</td></tr>"));
            }
            phComments.Controls.Add(new LiteralControl("</table>"));
        }
    }
}
```

## CommentsPresenter.cs

The CommentsPresenter is responsible for handling all the logic behind this control. It determines whether or not to show the control based on whether or not the user who is viewing the control is logged in or not. It also handles loading the comments for the specified SystemObject and SystemObjectRecordID. Finally, there is a method for adding a comment.

```csharp
//Fisharoo/Web/UserControls/Presenters/CommentsPresenter.cs
public class CommentsPresenter
{
    ...
    public void LoadComments()
    {
        _view.LoadComments(_commentsService.GetCommentsBySystemObject(
            _view.SystemObjectID, _view.SystemObjectRecordID));
    }

    public void AddComment(string comment)
    {
        Comment c = new Comment();
        c.Body = comment;
        c.CommentByAccountID = _webContext.CurrentUser.AccountID;
        c.CommentByUsername = _webContext.CurrentUser.Username;
        c.CreateDate = DateTime.Now;
        c.SystemObjectID = _view.SystemObjectID;
        c.SystemObjectRecordID = _view.SystemObjectRecordID;
        _commentsService.SaveComment(c);
        _view.ClearComments();
        LoadComments();
    }
}
```

## Ratings Page

Now let's take a look at the ratings page.

## Ratings.ascx

The Ratings control is responsible for displaying a modal pop up that contains the various options that can be rated. It then captures those ratings and saves them away to the database. In addition to handling the user interaction, it also displays the overall rating average for that particular object.

In order to use the rating control or the modal pop up, we need to register the AjaxControlToolkit (version 4.1.1.40412.2). Once this is added, we can hook up the modal pop up to our pnlModalPopup. This panel contains a repeater that will show all the ratings available for this object. Outside of the modal pop up, we also have a rating control that displays the average rating for the object.

```aspx
//Fisharoo/Web/UserControls/Ratings.ascx
<asp:Panel ID="pnlRating" runat="server">
    <asp:UpdatePanel runat="server">
        <ContentTemplate>
            <asp:LinkButton ID="lbRateThis" runat="server" Text="Rate this!" OnClick="lbRateThis_Click"></asp:LinkButton>
            <asp:Label ID="lblThankYou" runat="server" Text="Your ratings have been saved! " Visible="false"></asp:Label><br />
            <ccl:Rating ID="Rating1" Enabled="false" ReadOnly="true" runat="server" MaxRating="5" EmptyStarCssClass="ratingStarEmpty" FilledStarCssClass="ratingStarFilled" StarCssClass="ratingStar"
                PopupControlID="pnlModalPopup"
                OkControlID="btnSave" CancelControlID="btnCancel" />
        </ContentTemplate>
    </asp:UpdatePanel>
</asp:Panel>
```

There are two things that are important to notice in this UI. The first is the line that assigns the SystemObjectRatingOptionID to the Tag property of the Rating control.

```aspx
Tag="<%# ((PESystemObjectRatingOption)Container.DataItem).SystemObjectRatingOptionID %>"
```

When a Rating is selected, it fires an event that contains the Rating control and the selected value (the star that was selected). We are passing the SystemObjectRatingOptionID in the Tag property of the Rating.

The second important thing to notice is the property in our btnSave button. The UseSubmitBehavior property turns off the submit behavior of the btnSave button. If we don't disable this property, then the modal pop up will disappear when you click the button and you will not be able to capture the btnSave_Click event. When you do turn this behavior off, we are able to capture the event.

## Ratings.ascx.cs

In this class, we have two properties—SystemObjectID, and SystemObjectRecordID. These allow us to load and capture the data for the appropriate object. Next, we initialize our presenter. In doing this, we load the current options, set the display, and set the current rating.

Next, we have the event handler that captures when a user selects a rating in our modal pop up. This is straight through to the presenter.

Then we have the btnSave_Click event handler. This method not only sends the event upstream to the presenter but also sets the visibility of the pnlModalPopup to false, thus effectively hiding the pop up. (If this is not done, the ModalPopup seems to break and display itself in the page rather than above the page.) We also hide the label that acts as the link to show the ModalPopup and the Rating control that shows the average rating for the control. Finally, we thank the user for his/her input!

We then get to the lbRateThis_Click method, which shows the modal popup panel pnlModalPopup.

Finally, we have a method that sets the **lbRateThis** (link to show modal popup) and the **pnlModalPopup** visibility. This method is interacted with directly from the presenter:

```csharp
//Fisharoo/Web/UserControls/Ratings.ascx.cs
public partial class Ratings : System.Web.UI.UserControl, IRatings
{
    ...
    public void LoadOptions(List<PESystemObjectRatingOption> Options)
    {
        repRatingOptions.DataSource = Options;
        repRatingOptions.DataBind();
    }
    ...
    protected void btnSave_Click(object sender, EventArgs e)
    {
        _presenter.btnSave_Click(sender, e, SystemObjectID,
                                  SystemObjectRecordID);
        pnlModalPopup.Visible = false;
        lbRateThis.Visible = false;
        Rating1.Visible = false;
        lblThankYou.Visible = true;
    }
    protected void lbRateThis_Click(object sender, EventArgs e)
    {
        pnlModalPopup.Visible = true;
    }
    public void CanSetRating(bool Visible)
    {
        lbRateThis.Visible = Visible;
        pnlModalPopup.Visible = Visible;
    }
}
```

## RatingsPresenter.cs

In the RatingsPresenter, we have started off with the Init() method. This method loads all the options for our Ratings control. It then moves on to determine if the user is logged in or not. If they aren't, then we disable the ability to set a rating. If the user is logged in, we check to see if the user has provided a rating for this control before or not. If they have, then we also disable the ability to set a rating. Otherwise, we allow the user to provide the ratings. Finally, we set the current rating for this control.
