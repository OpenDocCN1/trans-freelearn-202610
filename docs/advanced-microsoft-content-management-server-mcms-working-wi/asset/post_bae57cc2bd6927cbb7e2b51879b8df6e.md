
## Creating a New MCMS Web Entry Point

We now have a SharePoint portal created in our virtual server. Now we need to configure MCMS to use our virtual server as an MCMS Web Entry Point.

1. Start the Server Configuration Application by pointing to Start | All Programs | Microsoft Content Management Server | Server Configuration Application.
2. Click the Web tab to view all virtual servers and manage their MCMS configuration.

![](img/442e26bf28f2df81fbf8dff3478f88e3_516_0.png)

3. You can see that our new virtual server, portal.tropicalgreen.net, is not configured for MCMS. Click the Configure button to edit the virtual servers listed.
4. Set the dropdown for our virtual server in the MCMS column to Yes – Read Write. Click OK through the dialog and the configure page.

We now have a SharePoint portal and have configured our new virtual server to be an MCMS Web Entry Point. However MCMS will not currently receive any requests because SharePoint will consume all URLs destined for our virtual server.

## Configuring SharePoint to Allow MCMS Requests

As outlined above, the MCMS requests will not be processed correctly at this point because the SharePoint ISAPI filter consumes all requests coming into our virtual server. If a request can't be processed by the SharePoint ISAPI filter, it will simply return an HTTP 404 – File Not Found response to the requestor.

As an example of this, try navigating to a path that does not exist, such as http://portal.tropicalgreen.net/fakepath. As this response is sent directly by the

If you see our Tropical Green plant catalog page, MCMS is working. If you don't see our plant catalog homepage, there's likely an issue with the excluded managed paths. Verify the last step.

You'll notice the last URL we tested looks quite strange as it contains two domains. The code from the first book *Building Websites with Microsoft Content Management Server* (Packt Publishing, January 2005, ISBN 1-904811-16-7), which we'll assume you're using, contains two top level channels: **international.tropicalgreen.net** and **www.tropicalgreen.net**. The first book implemented the MCMS Map Channel Names to Host Header Names option, which is why these channels have domains for their names. Unfortunately the Map Channel Names to Host Header Names feature is not supported when MCMS and SharePoint are running on the same virtual server. Therefore we have to disable this option and then the name of the top level channel we need to test (**www.tropicalgreen.net**) needs to be included in the URL. Usually you would change the channel structure in such a situation so that the domain name is not included in the URL.

If you continue to have problems, refer to Microsoft Knowledge Base article #835248: http://support.microsoft.com/?id=835248.

## Summary

In this appendix, we have explained how to successfully create a SharePoint portal and an MCMS Web Entry Point on the same virtual server. After configuring the virtual server, we showed how to modify the managed path exclusions in the SharePoint virtual server settings to ensure that the SharePoint ISAPI filter ignores HTTP requests that are to be handled by MCMS.

![](img/442e26bf28f2df81fbf8dff3478f88e3_522_0.png)

You can download the MCMS Connector for SharePoint Technologies here:
http://www.microsoft.com/downloads/details.aspx?FamilyID=6E9925C4-91DA-404A-86DD-78D51BCF0A51&displaylang=en

The only question you are prompted to answer during installation (other than the license agreement and user information—the registration key is already provided) is whether to install the sample data, which includes a demo project and an SDO file. The SDO file will automatically be imported into your MCMS installation when the sample data is installed. The SDO file contains the necessary templates, channels, and postings used by the sample package to demonstrate the features of the Connector.

## Leveraging SharePoint Portal Search within an MCMS Site

The Connector includes three controls that enhance your MCMS templates. Two of these controls are used to collect search query information and to display the results of the query after it has been submitted to the SPS:

- **SearchInputControl**: The SearchInputControl displays an input form where a user can enter keywords to search for.
- **SearchResultControl**: The SearchResultControl is responsible for taking the keywords and advanced properties submitted in the SearchInputControl, constructing the query, and calling the SPS Search service. The control then takes the response from the Search Service web service and displays the search results.

The third search control included in the Connector is:

- **SearchMetaTagGenerator**: This control generates HTML META tag information for MCMS posting properties and custom properties. It is used in combination with an XML mapping file and a command-line utility to configure the SharePoint indexer. This allows users to implement a basic taxonomy for the search function.

The standard and custom properties for which META tags are generated are managed using an XML file called SearchPropertyCollection.xml and a console application called SearchPropertiesSetup.exe, which configures SharePoint Portal Server search using the advanced properties defined by the XML file.

The SearchPropertyCollection.xml file can be found in the following directory:
C:\Program Files\Microsoft Content Management Server\Server\IIS_CMS\WssIntegration.

The SearchPropertiesSetup.exe utility program can be found in the following directory:
C:\Program Files\MCMS Connector for SharePoint Technologies\WSS\Bin.

If SharePoint Portal Server and MCMS reside on separate physical servers, the SearchPropertyCollection.xml file needs to exist on both servers. The SearchInputControl uses this XML file on the MCMS server and the SearchPropertiesSetup.exe utility uses the file on the SharePoint Portal Server.

Some configuration is necessary in order to use SPS as a search engine for your MCMS site. This involves creating a new content source, source group, and source scope within SPS. Fortunately Microsoft included another command-line utility program called SearchSetup.exe, which creates the new content source and site path rules within SPS for all top-level channels in your MCMS site.

The SearchSetup.exe utility program can be found in the following directory: C:\Program Files\MCMS Connector for SharePoint Technologies\WSS\Bin.

Refer to Chapter 5, Searching MCMS with SharePoint, for more information regarding MCMS and SharePoint Portal Server search integration.

## Integrating MCMS Content into a Portal site

By utilizing a single web part included in the Connector, you can display a list of postings in your SharePoint portal. The Page Listing Web Part displays the postings within a specified channel in one of six different list formats. The following three formats are hard coded in the web part:

- **Page Summary List View**: This view shows all postings with a short title (or display name), description, last modified timestamp, and a summary image. This is the default view for the web part. Each item is linked to the posting's published URL.

- **Basic List View**: The Basic List View displays the postings as a list, each with an importance icon and its short title (or display name).

- **Detailed List View:** Very similar to the Page Summary List View, this view does not display a summary image but it does add the posting owner, the user who last modified the posting, and the status of the posting (if it's published or not).

The web part provides entries for three additional views, which are not shipped with the Connector. These views can be added by a site developer and completely tailored to his or her needs. Refer to Chapter 6, *Publishing Content Between MCMS and SharePoint*, for more information on creating a custom view for the Page Listing Web Part.

## Publishing SharePoint Document Library Documents within an MCMS Site

The last integration point Connector for SharePoint Technologies covers is publishing documents of a SharePoint (WSS or SPS) document library within an MCMS posting. The connector ships with a new placeholder definition and a new placeholder control: the **SharePointDocumentPlaceholder** definition and the **SharePointDocumentPlaceholderControl**. A developer can add a **SharePointDocumentPlaceholder** definition to a template definition and a **SharePointDocumentPlaceholderControl** to the template file to display files from a SharePoint document library.

The placeholder displays the documents in a list of the document names, linked to the actual document. If the file type is registered within Microsoft Windows Server, the Connector will generate the icon. Otherwise a default icon for the file type is generated by the Connector. You can also override the icon used for the file type by specifying a value in a stylesheet.

Presentation of the list is entirely customizable via XSLT stylesheets, which are used to transform the properties and contents of the documents in the SharePoint document libraries to HTML.

Refer to Chapter 6, *Publishing Content Between MCMS and SharePoint*, for more information on the **SharePointDocumentPlaceholder** and **SharePointDocumentPlaceholderControl**.

## Additional Utilities Included

In addition to the SharePoint web parts, server controls, and MCMS placeholders provided by Connector for SharePoint Technologies to allow content integration, Microsoft included additional SharePoint web parts and command-line utilities to assist you with configuring SharePoint and managing your MCMS website.

The following two administrative SharePoint web parts are included in the Connector to assist MCMS site Authors, Editors, and Moderators with maintaining their MCMS site:

- **Pages in Production Web Part:** This web part displays all postings the current user is currently working on, but has not yet published, which is comparable to the "Production Manager" dialog in MCMS.
- **Pages Waiting for Approval Web Part:** This web part displays all postings that are pending approval on the MCMS site, which is comparable to the "Approval Assistant" dialog in MCMS.
