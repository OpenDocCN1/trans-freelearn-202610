# Searching MCMS with SharePoint

Let's see if our search is working. Save all changes to search.aspx, build the Tropical Green project, and go to http://www.tropicalgreen.net/TropicalGreen/Search.aspx in a browser. You should see a page similar to the one below:

![](bbox=[0.122, 0.15, 0.875, 0.39])

Enter a word you know will be found on the site, such as ficus. You will see the same list of search results that were returned when searching for the same string in the portal containing the content index.

![](bbox=[0.122, 0.475, 0.875, 0.675])

If you receive an error message stating "There was a problem loading the input control. The error returned by the system is: Could not find part of the path c:\inetpub\wwwroot\tropicalgreen\cms\wssintegration\searchpropertycollection.xml", double-check that you added the CMS virtual directory in your TropicalGreen web application.

At this point, we have got search capabilities on our site thanks to the MCMS Connector controls and SPS's search features. But this solution is very limited, for instance there is no way to change the look and feel of these controls and there is also no way to configure which properties are displayed in the result page, for instance to show a short description for the returned documents.

To address this, we will now build our own search controls.