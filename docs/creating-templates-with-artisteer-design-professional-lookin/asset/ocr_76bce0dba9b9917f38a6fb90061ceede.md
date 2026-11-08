17. You should now have the following:

```
<a href="images/1-large.jpg" rel="Lightbox[myGallery]" title="This is the first image of our gallery">
```

18. Repeat this operation for all the links in our gallery (remember that we have eight images). Click on the OK button to close the editor window. Export the project overwriting the previously exported files.

19. The last thing is to modify the exported code and to enrich our site with the real Lightbox effect. Open the folder with the exported project and copy the entire Lightbox folder we prepared before to this folder. Then edit the index.html file and find the part of the code where jQuery is attached. We are looking for the following line (near the beginning of the file):

```
<script src="jquery.js"></script>
```

20. Just below this line (make some space if needed), put the following code:

```
<!-- we add Lightbox effect -->
<link rel="stylesheet" href="Lightbox/css/Lightbox.css" />
<script src="Lightbox/js/Lightbox.js"/>
<!-- end of our modifications -->
```

21. Save the file and double-click on the index.html file. Our gallery should work as expected, but the close and loading images are missing. To fix this, just copy or move the close.png and loading.gif files from the Lightbox/images folder to our template images folder (from the ImageGallery/Lightbox/images directory to the ImageGallery/images directory).

That was the last thing to do. You should now see our project with beautiful, Lightbox2 effect image gallery with smooth display effect, navigation arrows, and image descriptions

# Summary

In this chapter, you have learned some tips that allow you to expand the functionality of the generated templates, and to do some things that are not possible from Artisteer directly. While modifying all the exported source code may seem complicated to you, the intention was to show you that automatically generated code can be extended to satisfy custom needs. What's next? Experiment, try to do new things, and don't be afraid. No matter how complicated the task seems to be, all in all it's just HTML. It's also the end of this book. I hope you enjoyed reading it and have found it useful.