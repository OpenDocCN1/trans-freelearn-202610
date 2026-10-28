# User Interactivity

Finally, we have a method that sets the **lblRateThis** (link to show modal popup) and the **pnlModalPopup** visibility. This method is interacted with directly from the presenter:

```
```csharp
//Fisharoo/Web/UserControls/Ratings.ascx.cs
public partial class Ratings : System.Web.UI.UserControl, IRatings
{
    - - -
    public void LoadOptions(List<PESystemObjectRatingOption> Options)
    {
        repRatingOptions.DataSource = Options;
        repRatingOptions.DataBind();
    }
    - - -
    protected void btnSave_Click(object sender, EventArgs e)
    {
        _presenter.btnSave_Click(sender, e, SystemObjectID,
                                  SystemObjectRecordID);
        pnlModalPopup.Visible = false;
        lblRateThis.Visible = false;
        Rating1.Visible = false;
        lblThankYou.Visible = true;
    }
    protected void lblRateThis_Click(object sender, EventArgs e)
    {
        pnlModalPopup.Visible = true;
    }
    public void CanSetRating(bool Visible)
    {
        lblRateThis.Visible = Visible;
        pnlModalPopup.Visible = Visible;
    }
}
```
```

# RatingsPresenter.cs

In the RatingsPresenter, we have started off with the Init() method. This method loads all the options for our Ratings control. It then moves on to determine if the user is logged in or not. If they aren't, then we disable the ability to set a rating. If the user is logged in, we check to see if the user has provided a rating for this control before or not. If they have, then we also disable the ability to set a rating. Otherwise, we allow the user to provide the ratings. Finally, we set the current rating for this control.