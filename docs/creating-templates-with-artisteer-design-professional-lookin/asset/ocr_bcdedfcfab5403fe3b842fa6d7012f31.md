Unfortunately, Artisteer doesn't allow us do define on which pages a menu should be displayed, and both the menus are displayed on every page. The way to work around this limitation is to create the template with both the menus (as we did) and then remove the vertical menu from the pages in which we don't want it to be displayed. This way we will make the vertical menu act as a submenu of the horizontal menu Let's begin:

+   1. Let's export the project from *Chapter 2, The Template Step-by-Step* once again and then open the `index.html` file (the home page of our site).
+   2. Now look at the files that Artisteer has created. You will see four `.html` files that are pages on the top level of the site structure, namely, `home`, `about me`, `offer`, and `contact`.
+   3. The subpages of the **Offer** page are located in a separate folder called `Offer`. The structure of folders and files reflects the site's logical structure. That makes our task simpler. All the `.html` files located in the `Offer` folder should display the vertical menu, while we should delete it from `.html` files located in the main project folder.
+   4. Let's open the `index.html` file and search for the vertical menu in the code. You will find something like the following:

```
<div class="art-vmenublockcontent">
<ul class="art-vmenu">
<li>
<a href="offer.html">Offer</a>
<ul>
<li>
<a href="offer/websites.html">Websites</a>
</li>
<li>
<a href="offer/wordpress.html">WordPress</a>
</li>
<li>
<a href="offer/joomla.html">Joomla!</a>
</li>
<li>
<a href="offer/dotnetnuke.html">DotNetNuke</a>
</li>
</ul>
</li>
</ul>
</div>
```