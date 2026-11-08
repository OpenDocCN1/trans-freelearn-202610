
The slideshow is displayed as two containers, and the inner one contains its own inner containers – in particular, slides. Each slide is represented as a `<div>` element (before the jQuery script in the `<header>` section does its job).

5. To make each slide a link, we are going to add some basic JavaScript to each slide; specifically, we are going to handle `onClick` events (in other words, we will define what should happen when a visitor clicks on this `<div>` element representing a particular slide). Because this is only a single-line script, there's no need to separate it as a `<script>` element in the code.

We are going to add something according to the following pattern:

```
onclick=location.href('address_to_be_redirected_to')
```

For example:

```
onclick=location.href('www.google.com')
```

This `onclick` event will redirect the visitor to www.google.com.

6. Let's modify our code and add the links:

```
<div class="art-slider art-slidecontainer79">
  <!-- The external container, containing the slideshow -->
  <div class="art-slider art-slidecontainer79">
    <!-- Slideshow container -->
    <div class="art-slider-inner">
      <!-- First slide. Link to: www.artisteer.com -->
      <div class="art-slide-item art-slide790"onclick="location.href='http://www.artisteer.com'"></div>
      <!-- Second slide. Link to: www.google.com -->
      <div class="art-slide-item art-slide791" onclick="location.href='http://www.google.com'"></div>
      <!-- Third slide. Link to: www.example.com -->
      <div class="art-slide-item art-slide792" onclick="location.href='http://www.example.com'"></div>
      <!-- Fourth slide. Link to www.packtpub.com -->
      <div class="art-slide-item art-slide793" onclick="location.href='http://www.packtpub.com'"></div>
    </div>
  </div>
</div>
```

7. Save the file and view it in a browser. The slides now act as links. The solution works, but it would be useful to have the mouse pointer indicate that there is a link. We will achieve this with a little bit of CSS, setting the appearance of the mouse cursor. We need to add the following text to each slide definition:

```
style="cursor: pointer;"
```

8. After modifications, we will have the following code:

```
<!-- The external container, containing the slideshow -->
<div class="art-slider art-slidecontainer79">
    <!-- Slideshow container -->
    <div class="art-slider-inner">
        <!-- First slide. Link to: www.artisteer.com -->
        <div class="art-slide-item art-slide790"
onclick="location.href='http://www.artisteer.com'"
style="cursor: pointer;"></div>
        <!-- Second slide. Link to: www.google.com -->
        <div class="art-slide-item art-slide791"
onclick="location.href='http://www.google.com'"
style="cursor: pointer;"></div>
        <!-- Third slide. Link to: www.example.com -->
        <div class="art-slide-item art-slide792"
onclick="location.href='http://www.example.com'"
style="cursor: pointer;"></div>
        <!-- Fourth slide. Link to www.packtpub.com -->
        <div class="art-slide-item art-slide793"
onclick="location.href='http://www.packtpub.com'"
style="cursor: pointer;"></div>
    </div>
</div>
```

9. It's much better! The last thing we should do is add a hint to inform the visitor about which page he/she will be redirected to. To do this, we need to add a title parameter to each <div> representing a slide as follows:

```
title="description of the link"
```

10. Our final code looks as follows:

```
<!-- The external container, containing the slideshow -->
<div class="art-slider art-slidecontainer79">
    <!-- Slideshow container -->
    <div class="art-slider-inner">
        <!-- First slide. Link to: www.artisteer.com -->
        <div class="art-slide-item art-slide790"
onclick="location.href='http://www.artisteer.com'"
style="cursor: pointer;" title="Artisteer official page"></div>
        <!-- Second slide. Link to: www.google.com-->
        <div class="art-slide-item art-slide791"
onclick="location.href='http://www.google.com'"
style="cursor: pointer;" title="Search engine"></div>
    <!-- Third slide. Link to: www.example.com -->
    <div class="art-slide-item art-slide792"
onclick="location.href='http://www.example.com'"
style="cursor: pointer;" title="Example domain"></div>
    <!-- Fourth slide. Link to www.packtpub.com -->
    <div class="art-slide-item art-slide793"
onclick="location.href='http://www.packtpub.com'"
style="cursor: pointer;" title="Visit Packt Publishing"></div>
    </div>
</div>
```

11. That's all. After the modifications, each link in our slideshow is a link to another webpage. The mouse cursor is the shape of a hand, which is typical for hyperlinks. Besides, there's a short description about where you will be redirected to.

![](img/7f889df2ffdb68108cff34d798a2b9a3_129_0.png)

## Removing the vertical menu from some pages

This tip applies to HTML templates. An example project would be the chapterTwo.artx project file.

In Chapter2, The Template Step-by-Step, we prepared a template with horizontal and vertical menus. The horizontal menu was the main menu of our site, showing the items related to directly linked pages, and the vertical menu was a complementary one, showing details of our offer. We came to the conclusion that the best way of organizing menus would be to show the vertical menu only on pages that are sub-pages of the Offer page. This would be better and more logical than to display details of our offer on every page.

Unfortunately, Artisteer doesn't allow us to define on which pages a menu should be displayed, and both the menus are displayed on every page. The way to work around this limitation is to create the template with both the menus (as we did) and then remove the vertical menu from the pages in which we don't want it to be displayed. This way we will make the vertical menu act as a submenu of the horizontal menu. Let's begin:

- 1. Let's export the project from *Chapter 2, The Template Step-by-Step* once again and then open the `index.html` file (the home page of our site).
- 2. Now look at the files that Artisteer has created. You will see four `.html` files that are pages on the top level of the site structure, namely, `home`, `about me`, `offer`, and `contact`.
- 3. The subpages of the **Offer** page are located in a separate folder called `Offer`. The structure of folders and files reflects the site's logical structure. That makes our task simpler. All the `.html` files located in the `Offer` folder should display the vertical menu, while we should delete it from `.html` files located in the main project folder.
- 4. Let's open the `index.html` file and search for the vertical menu in the code. You will find something like the following:

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

We see that the main container of the entire vertical menu is a `<div>` element, with the `art-vmenublockcontent` class. This is the key information for our task. Then the menu items are a list. The next level of menu items is represented as the next level of this list.

- 5. To remove the vertical menu from the page, just remove the main menu container with all of its content (the previous code). Now, save the file and refresh the page in the browser. We see that the vertical menu has disappeared from the **HOME** page.
- 6. Great! Now, repeat the whole operation for the rest of our top-level sites, that is, the `about_me.html`, `offer.html`, and `contact.html` files.
  As a result, we don't have the vertical menu on these pages any more.
- 7. Because we haven't touched the pages from the `Offer` folder, the subpages of the **Offer** page (**Websites**, **WordPress**, **Joomla!**, and **DotNetNuke**) display the vertical menu.

![](img/7f889df2ffdb68108cff34d798a2b9a3_131_0.png)

We have removed the vertical menu from the HOME page
