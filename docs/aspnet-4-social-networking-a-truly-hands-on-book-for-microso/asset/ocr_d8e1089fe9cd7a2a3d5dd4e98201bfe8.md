```
```aspx
<asp:PlaceHolder ID="phPrivacyFlagTypes"
runat="server"></asp:PlaceHolder> </div>
<div class="divContainerFooter">
<asp:Label ID="lblMessage" runat="server"
ForeColor="Red"></asp:Label>
<asp:Button CssClass="loginButton" ID="btnSave"
runat="server" Text="Save Privacy Settings" OnClick="btnSave_Click" />
</div> </fieldset> </div>
</asp:Content>
```
```

This then brings us to our code behind. Of course, it inherits from an interface so that we can pass a reference from this page to our ManagePrivacyPresenter class. As with most Model View Presenter (MVP) pages, we have to set up our presenter class. To get started we pass a reference of this page to the presenter in Page_Load.

```
```csharp
//Fisharoo/Web/Profiles/ManagePrivacy.aspx.cs
namespace Fisharoo.Web.Profiles
{
    public partial class ManagePrivacy : System.Web.UI.Page,
    IManagePrivacy
    {
        private ManagePrivacyPresenter _presenter;

        protected void Page_Load(object sender, EventArgs e)
        {
            _presenter = new ManagePrivacyPresenter();
            _presenter.Init(this);
        }

        public void ShowPrivacyTypes(List<PrivacyFlagType>
        PrivacyFlagTypes,
        List<VisibilityLevel> VisibilityLevels,
        List<PrivacyFlag> PrivacyFlags)
        {
            foreach (PrivacyFlagType type in PrivacyFlagTypes)
            {
                //Add the field name to the display
                phPrivacyFlagTypes.Controls.Add(new
                LiteralControl("<p>"));
                Label lbl = new Label();
                lbl.Text = type.FieldName + ":";
                lbl.Width = 100;
                phPrivacyFlagTypes.Controls.Add(lbl);

                //Create the visibility drop down
```
```