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