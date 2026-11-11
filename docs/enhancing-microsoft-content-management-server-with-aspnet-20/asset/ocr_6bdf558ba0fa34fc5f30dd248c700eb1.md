+   2. Switch to **Design** view and drag a **SiteMapPath** control from the **Navigation** section of the toolbar and drop it onto the text marker that says **Space for SiteMapPath Control**. Then delete the text marker.
+   3. Save the file.

We’ve added the `SiteMapPath` control to the site. To see how it appears on the site, navigate to any page. Notice that the control displays all links that lead up to the site’s root channel.

Home > Plant Catalog > Aloe Vera

What’s happening behind the scenes is that the `SiteMapProvider` has supplied the `SiteMapPath` control with the parent channels that lead to the current page. Without any extra programming on our part, the two components have worked together to generate the trail.

# The Menu Control

The ubiquitous menu control needs little introduction. Menus provide users with a list of links from which to choose. They are traditionally placed either horizontally at the top of the page, or vertically on the left or right-hand side of the page. If you’ve built a website, chances are you’ve attempted to put a menu somewhere on the site either by using ready-to-use widgets or by building one entirely from scratch using a collection of scripts. Either way, a fair amount of code had to be written to get the menu working.

Creating menus with the menu control of ASP.NET 2.0 is a breeze. Let’s add a couple of menus to the TropicalGreen site. We will start by building a static horizontal menu for the site’s banner. Following that, we will attempt to do something a little more complex and build a dynamic vertical menu that displays sub-menus as popouts.

# Building a Horizontal Menu

Static menus are easy to build. We could simply hard-code the links in the master page and the job is done. However, when the URLs of links change or when new items are added to the list, a developer is still required to do the deed. Moreover, hard-coding also means that all links are visible to all visitors to the site. Without additional coding, it would be difficult to hide links to restricted sites.

It is therefore preferable for menu items to be dynamically generated even though the menu is static. In previous versions of ASP.NET (version 1.x), we had to write recursive functions to generate each menu item. The good news is that the