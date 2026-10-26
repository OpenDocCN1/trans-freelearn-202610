
+   1. Right-click the project and select Add | Add Web Form.
+   2. Give the new page the name SearchResults.aspx.
+   3. In Design view, drag and drop the Styles.css file from Solution Explorer onto the form to apply the stylesheet to the page.
+   4. Change the page layout to FlowLayout.
+   5. Drag the following user controls into the designer:
+      - \UserControls\TopMenu.ascx
+      - \UserControls\RightMenu.ascx
+   6. Switch to HTML view and modify the body tag as follows:
+      <body topmargin="0" leftmargin="0">
+   7. Add the following HTML code to the page between the <form> tags, replacing the two controls we just added:

```
<form id="Form1" method="post" runat="server">
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tr>
<td width="100%" colspan="2" valign="top" bgcolor="#ffcc00">
<img src="/tropicalgreen/images/Logo.gif">
</td>
<td valign="top" rowspan="10">
</td>
</tr>
<tr bgcolor="#66cc33">
<td colspan="2">
<uc1:TopMenu id="TopMenu1" runat="server">
</uc1:TopMenu>
</td>
</tr>
<tr>
<td valign="top" style="PADDING-RIGHT:30px; PADDING-LEFT:30px; PADDING-BOTTOM:30px; PADDING-TOP:10px">
<p>&nbsp;</p>
<table cellspacing="0" cellpadding="10" border="1" bordercolor="#669900">
<tr valign="top">
<td>
<b>Tropical Green Search:</b>
</td>
</tr>
<tr>
<td valign="top">
<b>Advanced Search</b>
<p>
<b>Search Results</b>
</td>
</tr>
</table>
</td>
<td class="RightMenuBar" width="20%" valign="top" height="100%" align="center" rowspan="2" bgcolor="#669900">
<uc1:RightMenu id="RightMenu1" runat="server">
</uc1:RightMenu>
</td>
</tr>
</table>
</form>
```

We now have the basic layout for our advanced search and search results page, which looks similar to the other templates in our site. Let's add some controls for our advanced search.

+   1. In Design view, drag a TextBox from the Toolbox and place it under the Advanced Search text.
+   2. In Design view, drag a Button from the Toolbox and place it to the right of the TextBox.
+   3. The next thing we need to add is a DataGrid to contain the results of the search. In Design view, drag a DataGrid control from the Toolbox to just under the Add Search Results Here text. We'll worry about formatting this control later, for now we just need something to show us our data.
+   4. Set the properties of the controls we just added according to the following table:

| Property | Value |
|---|---|
| TextBox | ID = txtAdvancedSearch |
| Button | ID = btnAdvancedSearch<br>Text = Go |
| DataGrid | ID = dgrSearchResults |

Our advanced search page should now look like this:

![](img/442e26bf28f2df81fbf8dff3478f88e3_159_0.png)

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

Add the following method to the SearchResults.aspx.cs file after the Page_Load() event handler. This method will ensure the current thread is running under the original security context regardless of any impersonations that may have been invoked previously:

```
```csharp
// Get reference to the RevertToSelf method
[DllImport("ADVAPI32.DLL")]
public static extern int RevertToSelf();

/// <summary>
/// Builds the appropriate MSQuery,
/// submits the query to the SPS Query Service,
/// and returns the results as a DataGrid.
/// </summary>
/// <param name="keywords">String of keywords to search for.</param>
/// <returns>DataSet of search results.</returns>
private DataSet ExecuteSearch(string keywords)
{
    // decode the list of keywords
    keywords = HttpUtility.UrlDecode(keywords);

    // create reference to the Query Service Web service
    net.tropicalgreen.portal.QueryService spsQueryService =
        new net.tropicalgreen.portal.QueryService();

    // use the current application pool identity to login
    // to the SharePoint Query Service Web service
    WindowsIdentity CurrentUser = WindowsIdentity.GetCurrent();

    try
    {
        // use the Application Pool account to do access the
        // SharePoint Search Services
        RevertToSelf();
        spsQueryService.Credentials = CredentialCache.DefaultCredentials;
    }
    catch(System.Exception exception)
    {
        throw new System.Exception(exception.Message);
    }
    finally
    {
        // ensure that the original user is being impersonated again
        CurrentUser.Impersonate();
        CurrentUser = null;
    }

    // build MSQuery XML string to send to the Query Service
    // - change the content source to "CMSChannels" if you used SearchSetup.exe
    string msQuery = BuildMSQuery(keywords, "Tropical Green website");

    // execute the query and return the dataset
    return spsQueryService.QueryEx(msQuery);
}
```
```

> If you used the SearchSetup.exe program to create your content sources, you should use the content source group "CMSChannels" instead of "Tropical Green website" in the code above.

Our ExecuteSearch() method calls another method, called BuildMSQuery(), which constructs the MSQuery for sending to the QueryEx() web method. An MSQuery is composed of XML tags that provide instructions to the Query Service, such as the number of results to return in the request

and a Microsoft SQL full-text (MSSQLFT) query. Building the MSSQLFT query and MSQuery is likely to be the most complicated task in implementing the SharePoint search. We'll break it into two tasks: building the actual MSSQLFT query and building the MSQuery XML string. We'll first build the full-text query that our MSQuery will use in the construction of the XML string we'll send to the Query Service.

## Building the Microsoft SQL Full-Text Query

SPS's search process uses full-text indexes and queries for fast keyword lookups in order to provide timely responses to the end user. Full-text queries are very similar to regular T-SQL queries in Microsoft SQL Server, but you have additional functions, or predicates, that you can use to give your query more power. One of these predicates that is useful when searching SharePoint indexes is FREETEXT. FREETEXT takes a list of words separated by spaces, determines which words and phrases are significant, and uses that information to build an internal query to search the targeted data in an efficient manner.

First, you need to be aware of the various fields, or properties, available to you in your query. SharePoint provides a list with this information in the Site Settings administration page of your portal.

+   1. Open a new instance of Internet Explorer and navigate to our portal: http://portal.tropicalgreen.net.
+   2. Click on the Site Settings link in the upper right.
+   3. Under the Search Settings and Indexed Content section, click on the Manage properties from crawled documents link.

For each document crawled, the Manage Properties of Crawled Content page lists all properties that SharePoint could potentially contain indexed data on. For our purposes, we're only going to look at the two fields below, as they contain information for our search results page:

+   - DAV:href
+   - DAV:getlastmodified

![](img/442e26bf28f2df81fbf8dff3478f88e3_162_0.png)
