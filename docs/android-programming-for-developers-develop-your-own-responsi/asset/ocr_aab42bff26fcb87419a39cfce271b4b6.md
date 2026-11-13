your layout should look like this next image:

![](bbox=[0.165, 0.115, 0.88, 0.415])

+   19. Moving on to the right-hand side of the form, drag an **ImageView** and drop it on the hard left and top of the right-hand RelativeLayout.

+   20. Set its **src** property to ic_launcher by browsing to it on the **Projects** tab after left-clicking the ... link, as we have done before.

+   21. Set the **layout:width** property to match_parent. This doesn't make the actual image larger but it prevents it being wrapped so that the next step will work.

+   22. Find the **layout:centerInParent** property and set it to horizontal. The Android robot image should now be in the center-top of the right-hand RelativeLayout.

+   23. Drag a **Large Text** widget from the palette onto the right-hand RelativeLayout. You should be able to center it and set the margin between the ImageView to 46 dp before letting go of it. If your mouse doesn't have quite the right sensitivity to achieve this, you can set **margin:top** in the **Properties** window. You can also use the **Properties** window to set the **layout:centerInParent** property to horizontal if necessary.

+   24. Now we will add the actual terms and conditions. Obviously, following in the long tradition of all terms and conditions, we want to make this text smaller than the rest of the text on the form. Drag a **Small Text** widget to 10 dp below and exactly central to the previous Large Text widget. You