Now it's time to start coding our search logic. First, we need to add an event handler for our advanced search button.

+   1. In Design view, double-click the btnAdvancedSearch button to create a click event handler. Visual Studio .NET will add an event handler method to the code-behind file.
+   2. Add the following code to the btnAdvancedSearch_Click() event handler:

```
private void btnAdvancedSearch_Click(object sender, System.EventArgs e)
{
    string keywords = this.txtAdvancedSearch.Text;
    keywords = HttpUtility.UrlEncode(keywords);
    Response.Redirect(Request.ApplicationPath
        + "/SearchResults.aspx?keywords="
        + keywords);
}
```

Next, we need to check the querystring in the Page_Load() event handler to see if any keywords were passed from our SearchInput.ascx control or the txtAdvancedSearch TextBox.

Add the following code to check if there are any keywords supplied and execute the search if so:

```
private void Page_Load(object sender, System.EventArgs e)
{
    if (Request.QueryString["keywords"] != null
        && Request.QueryString["keywords"] != String.Empty)
    {
        string keywords = Request.QueryString["keywords"];
        DataSet ds = ExecuteSearch(keywords);
        this.dgrSearchResults.Visible = true;
        this.dgrSearchResults.DataSource = ds;
        this.dgrSearchResults.DataBind();

        // autofill the keyword input box with the search keywords
        this.txtAdvancedSearch.Text = keywords;
    }
    else
    {
        this.dgrSearchResults.Visible = false;
    }
}
```

Now we need to create the method that will execute the search against our SPS content index. This method will:

+   - Create an instance of the Query Service Web Service we just added to the project.
+   - Call a method that will build the MSQuery to submit to the Query Service.
+   - Execute the search.
+   - Bind the search results to a DataGrid.

Import the following namespaces in the SearchResults.aspx.cs file:

```
using System.Security.Principal;
using System.Runtime.InteropServices;
using System.Net;
using System.Text;
using System.Text.RegularExpressions;
using Microsoft.ContentManagement.Publishing;
```