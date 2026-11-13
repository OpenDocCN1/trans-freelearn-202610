# Where it's snap – coding the capture Fragment

Now we can add the capture functionality to the Where it's snap app. We will implement the functionality of capturing an image from the camera as well as allow the user to add a title and tags for the image. We will not, however, add any functionality to save any of this data, as you will first want to learn about databases before we do.

In preparation to save the data, however, we will meet the Android Uri and File classes, which allow us to identify and locate the photos that the user captures.

The completed app, as it is at the end of this chapter, can be found in chapter 22/Where Its Snap 2.

We will first add a layout that CaptureFragment will use, and we will then add some code, some of which should look fairly familiar by now, in order to capture an image. The next two chapters will then look at databases to save and show all our data.