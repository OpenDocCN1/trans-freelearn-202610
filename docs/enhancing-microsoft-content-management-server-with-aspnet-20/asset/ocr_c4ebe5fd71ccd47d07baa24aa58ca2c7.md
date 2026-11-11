+   - **Avoids issues with MCMS Template File Visual Studio templates**: The MCMS Project Item Templates have a number of issues, and do not fully embrace the Visual Studio 2005 project system. Although any web form can be MCMS ‘enabled’, master pages offer a more seamless development experience with less manual tweaks required.
+   - **Visual Studio 2005 Designer support**: One of the common problems with using user controls within template files in Visual Studio .NET is that the template design view doesn't provide an adequate experience for template developers. Visual Studio 2005 offers an improved design-view experience including rendering of user control content, and this is especially valuable when working with master pages.
+   - **Experience of Master Pages**: Just as MCMS is a great way to learn ASP.NET, MCMS SP2 is a great way to learn ASP.NET 2.0! In addition, master pages are a fundamental building block of future Web Content Management offerings from Microsoft.

> MCMS placeholder controls in the master page will work, but are not officially supported.

As we will see in this chapter, master pages provide an ideal way to separate common site branding, navigation, etc., from the actual content pages, and can significantly decrease duplication and markup errors.

# The TropicalGreen Web Site

Tropical Green is the fictitious gardening society upon which the book’s sample website is based. In the book, *Building Websites with Microsoft Content Management Server* from Packt Publishing (ISBN 1-904811-16-7), we built the Tropical Green website from scratch using ASP.NET 1.x.

In this book, we will attempt to rebuild parts of the website using MCMS SP2 and ASP.NET 2.0. While the code will be rewritten from the ground-up, we won’t start with a blank database. Instead, we’ll take a shortcut and import the TropicalGreen database objects from the `TropicalGreen.sdo` file available from the support section on Packt Publishing’s website (http://www.packtpub.com/support).