
+   4. Change the page layout to **FlowLayout**.
+   5. Drag the /Styles/styles.css, /UserControls/TopMenu.ascx, and /UserControls/RightMenu.ascx files from Solution Explorer onto the designer.
+   6. Switch to HTML mode and modify the body tag as follows:
+   `<body topmargin="0" leftmargin="0" rightmargin="0">`
+   7. Add the following code between the `<form>` and `</form>` tags, replacing the two user controls that were just added:
+   ```
+   <form id="Form1" method="post" runat="server">
+   <table width="100%" border="0" cellspacing="0" cellpadding="0">
+   <tr>
+   <td width="100%" colspan="2" valign="top" bgcolor="#ffc00">
+   <img src="/tropicalgreen/images/Logo.gif">
+   </td>
+   <td vAlign="top" rowSpan="10">&nbsp;</td>
+   </tr>
+   <tr bgColor="#66cc33">
+   <td colspan="2"><uc1:TopMenu id="TopMenu1" runat="server"></uc1:TopMenu></td>
+   </tr>
+   <tr>
+   <td vAlign="top" style="PADDING-RIGHT:30px; PADDING-LEFT:30px; PADDING-BOTTOM:30px;">
+   <p>&nbsp;</p>
+   <table cellspacing="0" cellpadding="10" border="1" bordercolor="#669900">
+   <tr vAlign="top">
+   <td>Tropical Green Search:</td>
+   </tr>
+   <tr>
+   <td vAlign="top">
+   </td>
+   </tr>
+   </table>
+   </td>
+   <td class="RightMenuBar" width="20%" valign="top" height="100%" align="center" rowspan="2" bgcolor="#669900">
+   <uc1:RightMenu id="RightMenu1" runat="server"></uc1:RightMenu>
+   </td>
+   </tr>
+   </table>
+   </form>
+   ```

## Why did we drag the user controls onto the page and then replace the resulting HTML?

Dragging the user controls onto the page adds the `<%@ Register %>` lines to the ASPX for us as well as adding the user control ASP.NET tags to the HTML. We then only need to modify the HTML to make it more presentable.

## Searching MCMS with SharePoint

You should now have a page that looks like the following when viewed in Design mode:

![](img/442e26bf28f2df81fbf8dff3478f88e3_152_0.png)

Let's save our new search page, build the Tropical Green project, and navigate to it in a browser to make sure everything is in order before we go about adding the search input and results controls.

+   1. Save all changes to the search.aspx page.
+   2. Right-click the TropicalGreen project and select Build.
+   3. If there are no errors in the build, open a browser and navigate to: http://www.tropicalgreen.net/TropicalGreen/Search.aspx.
+   4. If there are any issues, retrace the steps we've taken to this point, address the errors, and retry the URL.

Now that we have a working search page, we need to add some functionality to it. We'll add the two MCMS Connector server controls, make some configuration changes, build the solution, and test our search page.

+   1. Open the search.aspx page in Visual Studio .NET if it's not already open, and switch to Design view.
+   2. Open the Visual Studio .NET Toolbox and drag the SearchInputControl and SearchResultsControl into the table cell below the Tropical Green Search cell. Refer to the following image for placement:

![](img/442e26bf28f2df81fbf8dff3478f88e3_153_0.png)

3. Select the SearchInputControl we added to search.aspx and set the following properties in the Visual Studio .NET property window:

| Property | Value |
|---|---|
| SearchMode | Simple |
| SearchResultPage | /TropicalGreen/Search.aspx |

4. Select the SearchResultControl we added to the search.aspx page and set the following properties using the Visual Studio .NET property window:

| Property | Value |
|---|---|
| PortalUrl | http://portal.tropicalgreen.net/ |
| SearchResultPageSize | 10 |

> We're using the URL of the portal created in Appendix A. Replace this URL with whatever portal you configured for the content source and search group in the steps already covered in this chapter.

## Searching MCMS with SharePoint

Let's see if our search is working. Save all changes to search.aspx, build the Tropical Green project, and go to http://www.tropicalgreen.net/TropicalGreen/Search.aspx in a browser. You should see a page similar to the one below:

![](img/442e26bf28f2df81fbf8dff3478f88e3_154_0.png)

Enter a word you know will be found on the site, such as ficus. You will see the same list of search results that were returned when searching for the same string in the portal containing the content index.

![](img/442e26bf28f2df81fbf8dff3478f88e3_154_1.png)

If you receive an error message stating "There was a problem loading the input control. The error returned by the system is: Could not find part of the path c:\inetpub\wwwroot\tropicalgreen\cms\wssintegration\searchpropertycollection.xml", double-check that you added the CMS virtual directory in your TropicalGreen web application.

At this point, we have got search capabilities on our site thanks to the MCMS Connector controls and SPS's search features. But this solution is very limited, for instance there is no way to change the look and feel of these controls and there is also no way to configure which properties are displayed in the result page, for instance to show a short description for the returned documents.

To address this, we will now build our own search controls.

## Building a Custom Search Implementation

As outlined previously, there are advantages and disadvantages to the MCMS Connector search controls. The most obvious is the fact that the SearchResultControl does not allow us to configure the results returned by the SPS search. We will now build our own search implementation that will leverage the SPS search Web Service, offer advanced and specialized searching to our users, and present the results in a customizable manner.

### About the SharePoint Portal Server Query Service

Everything we are about to build depends upon the Query Service Web Service, included in SPS, that exposes search functionality to remote clients, such as our website. This web service accepts a request in the Microsoft.Search.Query XML format and returns a response in the Microsoft.Search.Response XML format. In order to build a robust solution, the request we submit will use the Microsoft SQL Syntax for full-text Search. One method offered by the Query Service is QueryEx, which we will use as it returns results in the form of a DataSet.

> For more information and documentation on the Microsoft SharePoint Portal Server Query Service Web Service, see the MSDN documentation at:
> http://msdn.microsoft.com/library/default.asp?url=/library/en-us/spptsdk/html/cPSQueryService_SV01004360.asp.

### Building a Search Input Control

The first thing we'll do is build a search input control that will submit a search query to a page for processing. This implementation will allow us to add a small search component to all of our templates quickly. Upon submitting a search query, our user control will add the query parameters to the querystring and redirect the request to the results page.

Let's first start by creating a new user control.

+   1. In Visual Studio .NET, right-click the User Controls folder in the Tropical Green project, and select Add | Add Web User Control.
+   2. Name the new control SearchInput.ascx.
+   3. While in Design view, drop controls from the Toolbox onto the Web Form and arrange them as shown below:

| Control | Properties |
|---|---|
| TextBox | ID = txtSearchInput |
| Button | ID = btnExecuteSearch<br>Text = Go |
| LinkButton | ID = lnkAdvancedSearch<br>Text = advanced search options |

![](img/442e26bf28f2df81fbf8dff3478f88e3_156_0.png)

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
