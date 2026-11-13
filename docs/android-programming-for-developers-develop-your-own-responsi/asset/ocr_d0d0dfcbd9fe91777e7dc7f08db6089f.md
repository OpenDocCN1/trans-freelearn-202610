# Laying out the animation demo

Create a new project called Animation Demo, leaving all the settings at their defaults as usual.

Here is how we can lay out the UI for this app:

+   1. Delete the default **Hello world!** widget.
+   2. Add **LinearLayout (vertical)**.
+   3. Inside the layout from the previous step, place **LinearLayout (vertical)** and **RelativeLayout**.
+   4. Set layout:weight on the two layouts that we added in the previous steps to .3 for LinearLayout and .7 for RelativeLayout.
+   5. Now, set layout:height of both to unset so that they fill three tenths and seven tenths of the screen, respectively.
+   6. For **LinearLayout (vertical)**, change its gravity property to center_horizontal.
+   7. Add **ImageView** to the inner **LinearLayout (vertical)**. Set the ImageView src property to @mipmap/ic_launcher to display the Android robot. Of course, you could choose any image that you have added to the drawable folder instead.
+   8. Set the id property of ImageView to imageView.
+   9. Directly below ImageView (still inside LinearLayout), add LargeText. Set the id property to textStatus and layout:margin top to 10dp. This should look like this next screenshot:

![](bbox=[0.17, 0.63, 0.59, 0.86])