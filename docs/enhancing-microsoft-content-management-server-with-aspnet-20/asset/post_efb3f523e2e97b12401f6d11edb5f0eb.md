
8. In the **Select File** dialog, select the **Basic.aspx** file inside the **Templates** folder, and click **Select**.

![](img/18a0aa8d36427a4c069b30410f998b8e_95_0.png)

9. Click the ellipsis (...) button on the right of the **PlaceholderDefinitions** property.

10. In the **Placeholder Definition Collection Editor** dialog, click **Add**, followed by **OK**.

![](img/18a0aa8d36427a4c069b30410f998b8e_95_1.png)

11. Click the **Save** icon at the top of the **Template Explorer** and click the **Solution Explorer** tab.
12. Double-click the **Basic.aspx** file inside the **Templates** folder and click the **Design** button at the bottom of the code editor.
13. Drag and drop **DefaultConsole.ascx** inside the **Console** folder from the **Solution Explorer** onto the design surface.
14. From the **Content Management Server** toolbox, drag and drop an **HtmlPlaceholderControl** onto the design surface.
15. Click the arrow on the **PlaceholderToBind** property within the **Properties** window, and select **NewHtmlPlaceholderDefinition1**.

![](img/18a0aa8d36427a4c069b30410f998b8e_96_0.png)

16. From the **Build** menu, choose **Build Web Site**.
17. From the **Tools** menu, choose **Content Management Server** followed by **Web Author**. Internet Explorer will open at `http://localhost/channels`.
18. Click **Switch To Edit Site**.
19. Click **Create New Page**.
20. In the **Template Browser**, click **Templates** followed by the select icon for the **Basic** template. The new posting opens in Edit mode.
21. Enter some text into the HTML placeholder and click **Save New Page**.

22. In the **Save New Page** dialog, enter **test** as the **Name** and click **OK**.
23. Click **Approve** followed by **Switch To Live Site**. Note that the text entered in the placeholder is saved in a new posting.

![](img/18a0aa8d36427a4c069b30410f998b8e_97_0.png)

We have now tested our custom MCMS web application project templates. Before moving on, it is a good idea to tidy up our test files and projects. We can delete our test posting, its template gallery item, and the two MCMS applications we have created: **mcmstest** and **mcmssp2**.

## Summary

We have reviewed the basics of the new Visual Studio 2005 MCMS development environment, configured the 'CMS' Virtual Directory, and have created our own custom Visual Studio 2005 templates for MCMS. We are now ready to dive into MCMS development with ASP.NET 2.0!

## 3

## Using Master Pages

One of the best features introduced with ASP.NET 2.0 is master pages, which allow developers to enforce common layout and behavior across pages within an application. While at first pass many master pages concepts are similar to those of MCMS templates, there are a number of benefits to be gained by taking advantage of master pages within MCMS applications.

This chapter provides an overview of the benefits of using master pages and a step-by-step guide for implementing them in your MCMS applications, where they become master templates!

### Overview and Benefits of Master Pages

A **master page** includes common markup and one or more **content placeholders**. From this master page, new **content pages** can be created, which include **content controls** that are linked to the content placeholders in the master page. This provides an ideal way to separate common site branding, navigation, etc., from the actual content pages, and can significantly decrease duplication and markup errors.

Master pages make working with template files a lot easier than before. You can add common markup and shared controls such as headers, footers, and navigation bars to master pages. Once a master page has been built, you can create MCMS template files based upon it. The template files will immediately adopt the look and feel defined in the master template.

You can also mark certain regions of the master page to be customizable by introducing content placeholders (note that these are controls designed specifically for master pages and are not to be confused with MCMS placeholder controls). The space marked by content placeholders provides areas where you could add individual template markup as well as MCMS placeholder controls, as shown in the following diagram:

![](img/18a0aa8d36427a4c069b30410f998b8e_100_0.png)

Although at first glance both master pages and MCMS templates offer a way to standardize the look and feel of a site, their similarities end there. Don’t be mistaken into thinking that master pages take over the role of MCMS templates completely. A key difference between the two is that the use of master pages is reserved solely for site developers to ensure that template files created have a common look and feel. You can’t create a master page and expect authors to use it to create postings.

In fact, master pages work alongside template files and offer a number of benefits to MCMS developers.

- **Avoids duplication in MCMS Template files:** Often MCMS templates contain common page layout code (usually an HTML table) along with navigation bars, headers, and footers (usually web user controls). This code has to be copied and pasted into each new template file after it is created or abstracted into user controls. In addition a change in the layout of this common code has to be applied to all template files. So, for example, an MCMS application with ten template files will duplicate this markup ten times. By placing this markup within a master page, this duplication can be removed.
- **Separation of site-wide markup from template markup:** One of the biggest drawbacks to MCMS is that the task of developing templates cannot be easily separated. It is a common requirement to separate the tasks of defining site branding, layout, and the development of controls such as navigation (performed by webmasters and programmers) from the task of designing template layouts (performed by business users). While master pages and Visual Studio 2005 do not address this completely due to MCMS’s inherent architecture, they offer a substantial improvement in this area.
- **Avoids issues with MCMS Template File Visual Studio templates**: The MCMS Project Item Templates have a number of issues, and do not fully embrace the Visual Studio 2005 project system. Although any web form can be MCMS ‘enabled’, master pages offer a more seamless development experience with less manual tweaks required.
- **Visual Studio 2005 Designer support**: One of the common problems with using user controls within template files in Visual Studio .NET is that the template design view doesn't provide an adequate experience for template developers. Visual Studio 2005 offers an improved design-view experience including rendering of user control content, and this is especially valuable when working with master pages.
- **Experience of Master Pages**: Just as MCMS is a great way to learn ASP.NET, MCMS SP2 is a great way to learn ASP.NET 2.0! In addition, master pages are a fundamental building block of future Web Content Management offerings from Microsoft.

> MCMS placeholder controls in the master page will work, but are not officially supported.

As we will see in this chapter, master pages provide an ideal way to separate common site branding, navigation, etc., from the actual content pages, and can significantly decrease duplication and markup errors.

### The TropicalGreen Web Site

Tropical Green is the fictitious gardening society upon which the book’s sample website is based. In the book, *Building Websites with Microsoft Content Management Server* from Packt Publishing (ISBN 1-904811-16-7), we built the Tropical Green website from scratch using ASP.NET 1.x.

In this book, we will attempt to rebuild parts of the website using MCMS SP2 and ASP.NET 2.0. While the code will be rewritten from the ground-up, we won’t start with a blank database. Instead, we’ll take a shortcut and import the TropicalGreen database objects from the `TropicalGreen.sdo` file available from the support section on Packt Publishing’s website (http://www.packtpub.com/support).
