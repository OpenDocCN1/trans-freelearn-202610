12. Use the Language Selector control to switch between languages. The column titles will be shown with the correctly translated values. Notice that the paging control has also been localized:

![](bbox=[0.14, 0.145, 0.855, 0.35])

# How it works...

Localizing the Datagrid headers requires using the LocalizeDataGrid API call found in the DotNetNuke.Services.Localization namespace. It requires that we provide the string translations in the local resource file.

# See also

The LocalizeDataGrid routine will localize the Datagrid headers, but to have the query results change requires passing the current locale to the backend stored procedure that pulls the results from the database. To see how this is done, see the previous recipe Localizing a stored procedure.