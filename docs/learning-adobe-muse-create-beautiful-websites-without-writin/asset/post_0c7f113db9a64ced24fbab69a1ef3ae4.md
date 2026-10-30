
## Creating a slideshow presentation

Earlier in this chapter we created a photo gallery which required the user to click on a thumbnail to see the larger image. This kind of functionality is also available using the Lightbox Slideshow widget or the Thumbnails Slideshow widgets. They work in a very similar way to the photo gallery we already created so rather than recreate those steps, in this next example we'll create a slideshow using the Blank Slideshow widget. We'll change the settings so that the slideshow plays automatically, running through a series of images. We'll specify the transition effects and the speed at which the slideshow plays. Perform the following steps to create a slideshow presentation:

- 1. Open your page in the Design view.
- 2. In the Widgets Library menu, choose the Blank Slideshow widget and drag it onto the page.
- 3. The Blank Slideshow widget consists of a dark gray rectangle which will hold our large image, a caption area, an image counter, and a next and previous image button for navigation.
- 4. Click on the blue arrow to access the Options menu. Click on the folder icon at the top, next to the Add Images... option.
- 5. Navigate to your images folder and choose as many images as you want to add to your slideshow. Hold down Ctrl/Cmd to choose multiple images, and then click on Open.
- 6. Your images will be added to the content area and the numbers in the image counter will increase to reflect the number of images in the slideshow. The default images from Muse remain in the slideshow. We simply delete these after adding our own images.

7. Click again on the blue arrow to open the widget's Options menu, as shown in the following screenshot:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_212_0.png)

8. Set the following options:

- Transition: Horizontal
- Auto Play: Medium
- New Hero: Fill Frame Proportionally (Hero is just a name for the main image)
- New Thumbnail: Fill Frame Proportionally – this is irrelevant here because we're not using thumbnails
- Lightbox: Deselected
- Parts: Deselect all options to hide the captions, counter, next, and previous buttons

Setting these options leaves us with a single image on the page.

9. Choose Preview to see how the slideshow will work in the browser. When all of the images have been displayed, the slideshow will loop around to the first image again and continue to play.

10. If you want to change the size of your slideshow, go back to the Design view and drag one of the corner handles of the Hero image frame. Hold down Shift as you drag to constrain the proportions of the slideshow. As you drag, you'll see the dark gray area increase in size, as well as the image itself.

In its current setup, this slideshow requires no user interactivity. If you decide that you want to let the user control the slideshow, click on the blue arrow again to open the options for the widget.

11. To provide thumbnails to correspond with the large images, select Thumbnails in the Parts section.

12. To provide the next and previous image controller buttons, select those options in the Parts section.

13. You can place each part of the widget in different parts of the page. Remember to click once to select the whole widget, and then click on the individual parts of the widget to position them where you want.

14. Select and position the thumbnails under the main image area.

15. Select and position the next and previous buttons at the top and right of the main image, as shown in the following screenshot:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_213_0.png)

## Insert arbitrary HTML

While Muse writes the HTML and CSS for everything we've looked at throughout this book, there may be times when you need to add a little bit of HTML yourself. Don't worry though; you won't have to write this code either. Let's say for example you want to add a Google map to your site to show clients or interested parties where you are based. Google will provide you with the code, you just copy it and then simply paste it back into your page in Muse.

Another example would be if you want to add a YouTube video. You don't need to host the video, you just paste in some code provided by YouTube and the video will appear, embedded in your page. A big advantage of using a service like this is that you don't need to worry about hosting the video content, it's still hosted by YouTube, and it just looks as if it's on your site.

Working with arbitrary HTML on a page in your site allows you to set aside a window in your page where third-party content can appear. As you can imagine, this opens up huge possibilities for you in terms of what you can add to a page without the need to write your own code. You could let your visitors know what the weather is going to be, show them a map of your hometown, impress them with audio and video, or provide them with stock market quotes.

## Adding a Google Map to your page

Let's add a map to a new page using the following steps:

- 1. In the Plan view, click on the plus symbol (+) under the Home page to add a new page.
- 2. Rename the new page as Location. Notice that the new page automatically takes on the A-Master page because it is a child page or a Home page which uses the A-Master page.
- 3. Double-click on the new Location thumbnail to open it in the Design view. Add a heading to Location and style it using the Heading style you created in the previous exercise.
- 4. Open an Internet browser and go to Google Maps at maps.google.com.
- 5. Type in the address or a search term to find the location you want to appear on your map.
- 6. Click on the Link button which you'll see appear at the top left of the interface.
- 7. Click on the Link button that appears in the upper-right part of the interface. Click once again on the option Customize And Preview Embedded Map.

8. A new Customize window appears which allows you to change the size of the map that you'll embed on your page. Select the Custom radio button in Map size to set Width to 800 pixels and Height to 400 pixels, as shown in the following screenshot:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_215_0.png)

9. You may need to reposition the map within the window to ensure that the pin and address text is fully visible. You can do this by simply clicking-and-dragging the map around until the pin and address are where you want them to be.

- 10. Google instantly generates the code for the map, which you can select and copy.
- 11. Go back to Muse. Choose Object | Insert HTML to open the HTML code window. Paste in the code you've copied using the keyboard shortcut to paste the code (Ctrl + V for Windows and Cmd + V for Mac).
- 12. Use the Selection tool to position the map where you want it on the page, as shown in the following screenshot:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_216_0.png)

In the Design view, you will be able to get an idea of how the Google Map will appear to your visitor. In order to see how the map is working interactively, click on Preview to see how it will appear on the live site. You can zoom in and out and move around the map which is embedded on your page.

## Adding a Twitter (or any other type of) feed to your site

Unless you've been living in a cave, there is a strong possibility that you have heard of, or use Twitter. Many Twitter users add a feed of their "tweets" to their site or blog and this maybe something you'd like to do with your own Muse-built site. Whether you're adding a Twitter feed or an RSS feed or any other type of dynamic data to your site, the process is essentially the same. The following steps show how to do it for a Twitter feed:

- 1. You'll need to have a Twitter account. Twitter is free to sign-up for and free to use. When you're registered, go to twitter.com/goodies/widgets where you can grab a widget to add to your web page.
- 2. You can choose how you want your Twitter widget to work. You can display only your tweets, the tweets of your favorite Twitter profiles, your favorite tweets, or a stream of topical tweets about a subject of your choice.
- 3. Once you've made that choice, you can then set up your widget. You can make changes using the Settings, Preferences, Appearance, and Dimensions menu. Choose a color scheme that matches or complements your own site's color scheme.

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_217_0.png)
