
    - - - code continues - - -
}
}

The code gets a reference to the `HierarchyItem` whose properties you wish to view. For template galleries, templates, resource galleries, and resources, this is obtained by getting the GUID from the `CMSObjectGuid` querystring parameter and using the `Searches.GetByGuid()` method. Channels and postings are obtained from the `CmsHttpContext.Current.ChannelItem` property.

In Design view, drag and drop the `Styles.css` file and `DataGrid` onto the `Properties.aspx` web form and give the `DataGrid` the following property values:

| Property | Value |
|---|---|
| Auto Format | Simple 3 |
| Font-Size | 10pt |
| ID | DataGrid1 |

![](img/442e26bf28f2df81fbf8dff3478f88e3_53_0.png)

Below the Page_Load() event handler, add the `ListProperties()` method. The method first creates a `DataTable` containing the following columns:

+   - The property name
+   - The property value
+   - Whether or not the property can be written to

We use the `GetType.GetProperties()` method to retrieve a collection of all properties associated with the hierarchy item. Next, iterate through each property of the current `ChannelItem` and add each one as a row to the `DataTable`. Notice that the property value is obtained by calling `PropertyInfo.GetValue()` and passing in the hierarchy item as an input parameter. Finally, we bind the `DataTable` to the `DataGrid`.

In the Page_Load() event handler, add code to read the Description of the current hierarchy item and display it on the screen.

```
private void Page_Load(object sender, System.EventArgs e)
{
    // Put user code to initialize the page here
    CmsHttpContext cmsContext = CmsHttpContext.Current;
    hlItem = null;

    string cmsObjectGuid = "";
    if (Request.QueryString["CMSObjectGuid"] != null)
    {
        // template gallery items and resource gallery items
        cmsObjectGuid = Request.QueryString["CMSObjectGuid"];
        hlItem = cmsContext.Searches.GetByGuid(cmsObjectGuid);
    }
    else
    {
        // channels and postings
        hlItem = cmsContext.ChannelItem;
    }

    // list all properties and their values in the grid
    if (hlItem != null)
    {
        litCurrentItem.Text = hlItem.Path;
        if (!Page.IsPostBack)
        {
            txtDescription.Text = hlItem.Description;
        }
        ListProperties();
    }
}
```

Toggle to Design mode and double-click on the btnUpdate button. In the btnUpdate_OnClick() event handler, we will write the code that updates the Description property value of the HierarchyItem based on the text entered into the textbox.

```
private void btnUpdate_Click(object sender, System.EventArgs e)
{
    try
    {
        // IMPORTANT: You must be in update mode for the code to work

        // update the description
        hlItem.Description = txtDescription.Text;

        // commit the change
        CmsHttpContext.Current.CommitAll();

        // refresh the page to ensure that the change shows up in the
        // datagrid
        Response.Redirect(HttpContext.Current.Request.Url.ToString());
    }
    catch(Exception ex)
    {
        CmsHttpContext.Current.RollbackAll();

        // after a rollback the CMS context needs to be disposed.
        CmsHttpContext.Current.Dispose();

        lblErrorMessage.Text = ex.Message;
    }
}
```

Note that the channel will not be permanently created until a call to `CommitAll()` is made.

> > Although we only set selected properties of the new channel, you could easily expand the interface to include all writeable properties of the channel object.

When you are done, save and build the project.

## Validating Names

MCMS applies the following rules when naming an object:

+   - The name may only contain the characters:
+     - A-Z a-z 0-9 (alphanumeric only)
+     - _ (underscore)
+     - - (hyphen)
+     - (space)
+     - () (opening and closing parenthesis)
+     - . (period)
+   - The name cannot end with a period.
+   - The name cannot contain two consecutive periods.

To ensure that all names given to channels, postings, template galleries, templates, and resources are valid, we will define a custom method named `ValidateMCMSObjectName()`. The method accepts the name of the object as an input parameter and returns a Boolean. A return value of `true` means that the name provided by the user meets all requirements by MCMS and is valid.

Since we will be adding dialogs to CMS Explorer that create all kinds of MCMS objects, we will add the `ValidateMCMSObjectName()` method to a class file, `Utility.cs`. In this way, we can re-use the method across all web forms within the project.

+   1. Add a class file to the project. Name the file `Utility.cs`.
+   2. Above the namespace declaration, add `System.Text.RegularExpressions`.

```
using System;
using System.Text.RegularExpressions;
namespace CMSExplorer
{
    - - - code continues - - -
}
```

+   3. Directly below the `Utility()` constructor, add the `ValidateMCMSObjectName()` method:

```
public static bool ValidateMCMSObjectName(string name)
{
    bool isValid;

    // test for other characters as the ones in the list
    Regex regEx1 = new Regex(@"[^A-Z0-9 _\-()\.]",
                             RegexOptions.IgnoreCase);
```

```
// test for "." at the end
Regex regEx2 = new Regex(@"[\\.]$", RegexOptions.None);

// test for double "."
Regex regEx3 = new Regex(@"\\.{2}", RegexOptions.None);

Match match1 = regEx1.Match(name);
Match match2 = regEx2.Match(name);
Match match3 = regEx3.Match(name);

// valid = no invalid chars, no double dot, and no dot at the end
isValid = !match1.Success && !match2.Success && !match3.Success;

return isValid;
}
```

The ValidateMCMSObjectName() method uses three regular expressions to check the validity of the name.

The first regular expression looks like this:

```
```text
[^A-Z0-9 _\\ (.-]
```
```

It’s a pattern that finds all characters in the name that are not alphanumeric, spaces, underscores, opening and closing parenthesis, periods and hyphens (note that the backslash character is used as an escape character), satisfying the first condition for a valid name.

The second regular expression tests to see if the name ends with a period:

```
```text
[\\.]$
```
```

Again, the backslash has been used as an escape character. The period between the square brackets indicates that we are looking for periods. The dollar sign instructs the pattern to match the end of the string.

The third and last regular expressions tests for double periods in the string:

```
```text
\\.{2}
```
```

As before, the backslash is used as an escape character. The number 2, surrounded by curly braces, indicates that we are looking for double occurrences of the period.

Finally, we run all three regular expressions. As long as there are no invalid characters, periods at the end of the name, or consecutive periods, the name is considered to be valid.

To use the ValidateMCMSObjectName() method within a dialog, we will simply call it from within the code. Here's how it's done for the Create Channel dialog. Add the following code to the btnCreate_Click() event handler:

```
```csharp
private void btnCreate_Click(object sender, System.EventArgs e)
{
    - - - code continues - - -

    try
    {
        - - - code continues - - -

        // get the name of the channel
        string Name = txtName.Text.Trim();

        // check to see if the name is valid
        if (!Utility.ValidateMCMSObjectName(Name))
```
```

```
{
    throw new Exception("Specified channel name is not "
        + "valid. Must be only alphanumeric "
        + "characters, open or close parens, "
        + "hyphens, underscores, periods, or "
        + "spaces. No period at the end and no "
        + "consecutive periods are allowed.");
}

// create the new channel
Channel newChannel;
newChannel = parentChannel.CreateChannel();
}
    - - - code continues - - -
catch(Exception ex)
{
    - - - code continues - - -
}
```

Should ValidateMCMSObjectName() detect that the name is invalid, we throw an exception. Save and build the project.

## Testing the Create Channel Dialog

Let's try out the Create Channel dialog:

+   1. Navigate to http://localhost/CmsExplorer.
+   2. Select New | New Channel from the DropDownList. This will open a new browser window containing the Create Channel dialog.
+   3. In the Create Channel dialog, enter the following values:
+   4. Click Create Channel and then click Close.
+   5. Click Refresh in the main window.

| Property | Value |
|---|---|
| Name | MyNewChannel |
| DisplayName | My New Channel |
| Description | This channel was created using the PAPI! |

If all goes well, the new channel is created and appears in CMS Explorer as shown below:

![](img/442e26bf28f2df81fbf8dff3478f88e3_65_0.png)
