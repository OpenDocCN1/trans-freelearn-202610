# # Building a Custom Search Implementation

As outlined previously, there are advantages and disadvantages to the MCMS Connector search controls. The most obvious is the fact that the SearchResultControl does not allow us to configure the results returned by the SPS search. We will now build our own search implementation that will leverage the SPS search Web Service, offer advanced and specialized searching to our users, and present the results in a customizable manner.

# ## About the SharePoint Portal Server Query Service

Everything we are about to build depends upon the Query Service Web Service, included in SPS, that exposes search functionality to remote clients, such as our website. This web service accepts a request in the Microsoft.Search.Query XML format and returns a response in the Microsoft.Search.Response XML format. In order to build a robust solution, the request we submit will use the Microsoft SQL Syntax for full-text Search. One method offered by the Query Service is QueryEx, which we will use as it returns results in the form of a DataSet.

> > For more information and documentation on the Microsoft SharePoint Portal Server Query Service Web Service, see the MSDN documentation at:
> > http://msdn.microsoft.com/library/default.asp?url=/library/en-us/spptsdk/html/cPSQueryService_SV01004360.asp.

# ## Building a Search Input Control

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