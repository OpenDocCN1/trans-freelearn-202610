+   - The TITLE will be displayed above both the picture and the description. It can be left blank if you do not want to display a title.
+   - The DESCRIPTION is displayed immediately below the title but above the picture. This can also be omitted if it is not required.
+   - The TARGET is used in conjunction with the LINK and describes how the new page will be opened when the image is clicked. Valid values are: `_blank`, `_parent`, `_top`, and `_self`. The `_blank` option, which is the default, will open the link in a new window or tab, depending on the browser. The `_self` option will open the link in the same window that the slideshow is being displayed in (and will therefore replace the slideshow). The `_parent` and `_top` values are only used by sites that use frames.

Multiple images can be inserted by separating each img section from the previous one with a comma.

The following filter tag is used on the Good Eatin' home page to display a few pictures for the restaurant.

```
[slideshow:2, img=|http://goodeatin.drupalbyexample.com/image/view/3/preview||Elephant Smile|This Elephant is one of the many animals you may see as you take a Safari through the Good Eatin' restaurant.||, img=|http://goodeatin.drupalbyexample.com/image/view/4/preview||Zebra Friends|Enjoy the sun with these Zebras on a Good Eatin' Safari meal. ]
```

# Setting up a slideshow to display all images in a folder

Although creating a slideshow with individual images gives you an enormous amount of control over how the slideshow is displayed, it can be very convenient to create a slideshow that simply displays all of the pictures in a folder. This type of slideshow uses the following filter tag format:

```
[slideshow:VERSION, dir=|DIR_IMAGE|DIR_RECURSIVE|DIR_LINK|TITLE|DESCRIPTION|TARGET|]
```

The VERSION, TITLE, DESCRIPTION, DIR_LINK, and TARGET have the same usage as the individual img version, with the exception that the values will be applied to each image in the folder. The folder-specific settings are as follows:

+   - DIR_IMAGE is the folder where the files are located on your web server. This path will be relative to the folder where Drupal is installed. Slideshow Creator will include all PNG, GIF, JPEG, JPG, and BMP pictures found within this folder in the slideshow.