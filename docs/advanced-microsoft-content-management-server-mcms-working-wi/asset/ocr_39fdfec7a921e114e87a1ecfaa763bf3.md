![](bbox=[0.32, 0.12, 0.62, 0.24])

4. The LinkButton we created will take the user to the search results page, which we'll add some advanced searching features to later. Double-click our LinkButton. Visual Studio .NET will create an empty event handler for the Click() event. Add a single line of code to this empty event handler to redirect the user to the search results page:

```
private void lnkAdvancedSearch_Click(object sender, System.EventArgs e)
{
    Response.Redirect(Request.ApplicationPath + "/SearchResults.aspx");
}
```

5. Next, we need to create an event handler for when a user clicks our Go button. We'll take the keywords entered in the TextBox and send the search request to the search results page. Double-click the Go button and add the following code to the event handler:

```
private void btnExecuteSearch_Click(object sender, System.EventArgs e)
{
    string keywords = this.txtSearchInput.Text;
    keywords = HttpUtility.UrlEncode(keywords);
    Response.Redirect(Request.ApplicationPath
        + "/SearchResults.aspx?keywords="
        + keywords);
}
```

Let's see if everything is OK with our new search input control. Save your changes and build the project. If you receive any error messages, retrace your steps and ensure that there are no typos.

Before this control can be used, we need to add it to an existing template. While we'd ideally want to provide the search on all pages on our site (typically by adding it to a global heading control), we'll just add it to the homepage for now.

+   1. Open the \Templates\HomePage.aspx template and drag our new SearchInput.ascx into the top cell, to the right of the logo.
+   2. Switch to HTML view and find the control we just added. It will likely have an opening tag of uc1:SearchInput. Wrap this control in an HTML DIV and set its alignment to right as shown in the following code:

```
<td width="100%" colspan="2" valign="top" bgcolor="#ffcc00">
    <img src="/tropicalgreen/images/Logo.gif">&nbsp;
    <div align="right">
        <uc1:SearchInput id="SearchInput1" runat="server"></uc1:SearchInput>
    </div>
</td>
```