
The first thing we need to do is add the Web Part to the Web Part Page. We'll put it below the MCMS Page Listing Web Part we were previously working with. To add the Web Part, repeat the same steps we followed when adding the MCMS Page Listing Web Part:

- 1. Navigate to a SharePoint portal in Internet Explorer:
  http://portal.tropicalgreen.net.
- 2. Switch to edit mode by clicking the Edit Page link in the vertical left-hand Actions menu.
- 3. Click the Modify Shared Page link in the upper left. Using the drop-down menu that appears, select Add Web Parts and then Browse.
- 4. Switch to the Virtual Server Gallery to see our installed Web Parts for the current virtual server.
- 5. Now that we have our Web Part on the page, we need to set its properties. Click the small down arrow in the upper-right corner of the MCMS Pages in Production Web Part we just added to our page and select Modify Shared Web Part.
- 6. Expand the Custom Properties section by clicking the icon to the left of the Custom Properties section heading.
- 7. We need to tell the Web Part the URL of our MCMS web application. Enter http://www.tropicalgreen.net/MCMS/ for the MCMS Site Root URL property. Click the Apply button at the bottom of the tool pane.

## The MCMS Pages Waiting for Approval Web Part

The MCMS Pages Waiting for Approval Web Part displays a list of postings that are pending the approval of the current user. This Web Part has exactly the same features, implementation steps, and configuration settings as the previous MCMS Pages in Production Web Part. The only difference between the two is the state of the postings. Since the two Web Parts are so similar, we won't walk through the steps in adding and configuring this Web Part on a SharePoint Web Part Page.

## Why does the MCMS Page Listings Web Part work with a guest-enabled Forms Authentication MCMS site but MCMS Pages in Production Web Part and MCMS Pages Waiting for Approval Web Part not work?

When your site is configured for guest access, you have likely granted the account set as the MCMS Guest Login Account the Subscriber role in Site Manager. This is all the rights the guest account needs to enumerate through the postings within the specified channel and display them in the MCMS Page Listings Web Part. However, the MCMS Pages in Production Web Part and MCMS Pages Waiting for Approval Web Part require higher rights—such as author, editor, or moderator rights—than the Subscriber role provides, and thus, these Web Parts will display a permission error message: "The windows credentials provided could not be validated."

If our Tropical Green MCMS site wasn't set to guest access, we'd have the same problem with the MCMS Page Listings Web Part for the same reasons.

If your MCMS site was set up to use Windows Authentication and you configured the MCMS Pages in Production Web Part and MCMS Pages Waiting for Approval Web Part to use this Windows Authenticated MCMS site, the two Web Parts would look something like this:

![](img/442e26bf28f2df81fbf8dff3478f88e3_188_0.png)

As we've demonstrated, the three Web Parts included with the Connector provide limited presentation (in the case of the MCMS Page Listing Web Part) and workflow administration (in the case of the MCMS Pages in Production and MCMS Pages Waiting for Approval Web Parts) of MCMS content. What if you need to display content from within a specific posting or a collection of channels and postings in a list?

While the MCMS Page Listing Web Part displays the postings within a channel, the presentation is somewhat restrictive. As we saw previously, each posting is listed within a one-cell-per-row HTML table. We have control over what HTML we'll put in the table cell. What if we need more control over the presentation? Or what if we wanted to show a tree-like organization of channels, sub channels, and postings?

## Limitations of the MCMS Connector for SharePoint

The Connector addresses some of the most common needs and uses when publishing content from an MCMS site to SharePoint. However, as the previous questions indicate, there are many more publishing possibilities. To satisfy these requirements, we have the option of building custom Web Parts that would pull content from MCMS into SharePoint in a much more configurable manner. In order to do this, we need access to the content. This can occur in one of three ways:

- 1. Extend a virtual server as a SharePoint site and configure it as an MCMS Web Entry Point. This option would allow Web Parts in the SharePoint site to access the MCMS content directly via the MCMS PAPI. This is demonstrated in the Web Part we create in Chapter 7, Building SharePoint Web Parts.
- 2. Configure MCMS and SharePoint to run on two separate virtual servers. The MCMS content you wish to publish can be exposed via Web Services on the MCMS site. A SharePoint Web Part could then consume those Web Services and present the data on the SharePoint Web Part Page. Building and leveraging Web Services to expose the MCMS postings and channels is covered in Chapter 11, InfoPath with MCMS Web Services.

> We have already explored all the controls and placeholders involved in implementing indexing and search capabilities in Chapter 5, Searching MCMS with SharePoint.

The SharePoint Document Placeholder enables content owners to link a document managed by SharePoint Portal Server to be presented within an MCMS posting. The content owner can also display properties of the linked document in the MCMS page. The presentation of the document and properties are controlled using XSLT stylesheets, which can be centrally controlled or individually applied to where they are needed. Once a document is linked to an MCMS posting, users can access the document either via SharePoint Portal Server or an MCMS site, which provides a seamless user experience for users viewing the document contents, regardless of the environment they access it from.

Any document stored within a SharePoint Document Library can be added to a posting via the SharePoint Document Placeholder. If the display view you choose includes an icon, the placeholder will use the icon associated with that document type on the MCMS Server. If the document type is a Microsoft Office InfoPath form or a Word Markup Language (WordML) document, you can have the contents of the document displayed in the placeholder Control. Conversely, all other document types are shown as attachments and/or the document properties. This is controlled by which XSLT options you choose to render the document library items.

> If the document is an InfoPath form, you need to ensure the user (content consumer) has rights to the SharePoint site containing the form. This is due to the fact that the SharePoint Document Library contains the template used to display the form in InfoPath.

When a document is added to an MCMS posting via the SharePoint Document Placeholder, the document is copied from the SharePoint Document Library into the MCMS repository as a local attachment. If the document is of type XML, such as an InfoPath or WordML document, the XML is saved in the MCMS placeholder as well (this content in the placeholder is set to readonly from within the MCMS site). Changes to the document in the SharePoint Document Library are not automatically reflected in MCMS. The Document Updater utility is included with the Connector to synchronize changes done in the document library into MCMS (if the "update automatically" checkbox has been checked in the SharePoint Document Placeholder control when creating the posting). The tool, WssDocumentUpdater.exe, can be found in the Connector's installation directory, C:\Program Files\MCMS Connector for SharePoint Technologies\CMS\bin\. This command-line tool can be set to run at regular intervals using the Windows Task Scheduler.

## Using the SharePoint Document Placeholder

Let's see the SharePoint Document Placeholder in action. First, we need to create and upload a document to a SharePoint Document Library:

- 1. Create a new Microsoft Word document and enter the following text in the body of the document: This is content that is saved within a Microsoft Word file.
- 2. Save the document to your desktop with the filename MCMS Published Document.
- 3. Open Internet Explorer and browse to our SharePoint portal: http://portal.tropicalgreen.net/.
