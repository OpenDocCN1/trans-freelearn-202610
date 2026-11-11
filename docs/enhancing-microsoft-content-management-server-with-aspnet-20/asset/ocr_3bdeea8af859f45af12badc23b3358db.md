```
```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!Page.IsPostBack)
    {
        // Get the first name
        lblFirstName.Text = PreviousPage.FirstName;

        // Get the last name
        lblLastName.Text = PreviousPage.LastName;

        // Get the address
        lblAddress.Text = PreviousPage.Address;

        // Get the plant
        lblPlant.Text = PreviousPage.Plant;
    }
}
```
```

6. Toggle back to **Design** view and double-click on the `btnConfirm` button to get to the `btnConfirm_Click()` event handler. Here, we will simply notify the user that the order has been successfully placed. Of course, in an actual website, you would probably do more than just print a message on the screen.

```
```csharp
protected void btnConfirm_Click(object sender, EventArgs e)
{
    lblMessage.Text = "Plant has been successfully ordered.";
}
```
```

7. The page is complete. Save and build the website.

# Adding the "Order Plants" Menu Item

Now that we have created both the ordering and order confirmation screens, let’s add a new item to the right menu for our "Order a Plant" wizard to give visitors to the site access to it. Recall that the menus are driven by the site’s channel structure. Therefore to add a menu item, we will simply insert a new channel and set its script URL to point to the order page.

+   1. Open **Site Manager** and log in with an account that has administrator privileges.