+   - timeout: This controls how long each image is displayed for, before the next image starts loading. This is also set in milliseconds.
+   - fx: This controls what type of effect is used to transition between slides. Some of the available transitions are shuffle, zoom, fade, turnDown, curtainX, scrollRight, blindX, blindY, blindZ, cover, and uncover.

Any of these additional options used should be added before the list of images begins. For example, the following code adds a zoom transition effect to the slideshow and places the controls for the slideshow, under the pictures.

```
[slideshow:2,width=450,height=650, layout=top, fx=zoom, img=!http://goodeatin.drupalbyexample.com/image/view/3/preview||Elephant Smile|This Elephant is one of the many animals you may see as you take a Safari through the Good Eatin' restaurant.|, img=!http://goodeatin.drupalbyexample.com/image/view/4/preview||Zebra Friends|Enjoy the sun with these Zebras on a Good Eatin' Safari meal. ]
```

![](bbox=[0.098, 0.4, 0.165, 0.44])

Additional documentation for the Slideshow Creator Module is available at: http://drupal.org/node/237049.
Additional documentation for the JQuery Cycle module, which is used to build the slideshow, is available at: http://www.malsup.com/jquery/cycle/.

# Version-specific issues

The author of Slideshow Creator is currently providing the module only to the users of the current version of Drupal (version 6 at the time of writing). If you are using another version, you may be able to find a download that is compatible with your version, but support may not be available through the original author.

# Showing images over the page with Lightbox2

The Slideshow Creator module allows you to view slideshows within a page. However, sometimes it looks better to display images in a new window that floats over the main page. This is especially true for pictures on a photography or art web site, where the pictures are the main focus of the page.

Showing the slideshow over the main page also improves the appearance of the slideshow when all of the pictures are not the same size.