
# Using multiple shapes within Blender

The last thing I want to mention is something we will use after we have finished with the model. When we export an object out of Blender and bring it into Unreal, the game engine turns all of our nice polygons into triangles for its own purposes. Now, for the most part this won't be a problem. Unreal was able to handle our simple crate pretty well. However, the more complicated the object, the greater the chance that Unreal will mess it up when it imports the file. We can help this process along by triangulating our object before we export it from Blender. Adding the `Triangulate` modifier is done in the `Modifier` menu of the `Properties` pane. Remember that you must be in `Object` mode to apply any modifier:

![Triangulate modifier tool](OEBPS/Images/BO5189_06_05.jpg)

`Triangulate` is essential, since most game engines require game assets to be made from triangles anyway.

With these additional tools we should be able to create an amazing Artifact. Time to start building!

### Tip

Don't forget to set up your file with the appropriate settings like we did in [Chapter 3](#ch03.xhtml "Chapter 3. It's Time to Customize!"), *It's Time to Customize!*

Now, let's start with a cylinder:

![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_06.jpg)

Starting with a cylinder

Let's start the build with the bottom of the object:

1.  Delete the starting objects from the scene, if you have them. Go to the `Add` menu located at the bottom of the 3D viewport, go to `Mesh`, and select `Cylinder` from the options. In the bottom of the `Tools` pane, you can see the options for our newly created `Cylinder`. Set the `Sides` to `24` (using just enough sides to stay round), set the `Radius` to `60`, and the `Depth` to `60`. Remember that these are based on the measurements we took of our whitebox prop in Unreal.

2.  Now move our newly created object up so that it looks like it's sitting on the grip. This will give our Artifact a good pivot point in Unreal:

    ![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_07.jpg)

    Inset the top

3.  Time to start shaping this thing. Press `Tab` to go to `Edit` mode. Select the top polygon and use the `Inset` tool to start giving it an inside. I set my `Inset` value to about `7.10`:

    ![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_08.jpg)

    Use the Extrude to create the inside of the Artifact

4.  Next, take the new polygon created by the `Inset` and `Extrude` it down into the base. Not too much; we want to give the illusion that it has a lot of space inside, but we don't want to push the polygon through the bottom.

5.  Now let's taper the bottom a bit. Grab the bottom polygon of our cylinder and `Scale` it down a bit. Be careful not to let the inside polygon poke through the sides. I scaled it by `0.75`.

6.  But wait! We still need a flat section for the locking mechanisms and legs to connect to! We can achieve this effect by creating a new edge loop around the middle section and scaling it back out:

    ![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_09.jpg)

    Creating the flat section for the legs

7.  Change your selection to `Line` mode and select all the lines around the middle. This can be achieved by pressing `B` to do a window selection, rotating your view, and doing another window selection until you have selected all of the lines. With them selected, click on the `Subdivide` tool in the `Tools` pane. This will create our new loop. Now select all the lines that make up that loop and `Scale` it outward to again flatten out the upper part of our base:

    ![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_10.jpg)

    Beveling the edges

8.  Let's round it out. Unlike the crate we created earlier, the shapes we are creating now are more rounded and organic. Select the lines that make up the inside and outside edges of the top of our Artifact. Press `W` to open up the `Specials` Menu and select `Bevel`. Using `Bevel` on an edge splits it into two edges. By using it on those two edge loops we can create a rounded element to our shape:

    ![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_11.jpg)

    Bevel once again to round out the shape

    Remember to be careful when using `Bevel`, we don't want the shape to turn itself inside out!

9.  One more `Bevel` and we can call this part done. Select the edge loop we created in Step 6 and give it a `Bevel`. This will give the bottom portion a good round shape as well. What you should have now should look like the beginnings of a flower pot, a very sinister flower pot.

10.  Looking at our sketch, the top portion looks similar to the bottom. We can save ourselves some time by duplicating the bottom piece and flipping it over:

    ![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_12.jpg)

    Duplicate the bottom to create our starting shape for the top

11.  In `Object` mode, select the bottom piece and click `Duplicate`, or press `Shift` + `D`. This will create a copy, but it will start moving it in all directions and we want to keep our pieces aligned. Press `Z` to lock the movement to the `Z`-axis and move the copy up a bit. Now use the `Rotate` tool and press `X` to lock the rotation to around the `X`-axis. Finally, type in `180` and press `Enter` to flip it over:

    ![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_13.jpg)

    Elongating the top of the Artifact

12.  Rounding out the top will give us the egg shape we are looking for. Grab the top polygon of our new lid and `Extrude` it out some more. Then `Scale` the new top polygon down a bit. It will start to look more rounded. Repeat this step once or twice more until you get the shape you desire.

The Artifact now has a bottom and a top section and is beginning to look a bit like our design drawing:

![Using multiple shapes within Blender](OEBPS/Images/BO5189_06_14.jpg)

Starting the gear section with a cylinder

In the next few steps, we will build the first part of the lock, a gear that holds a talisman in its claws:

1.  We need a basic shape to start this piece off with. Since a gear essentially has a round, flat shape, a cylinder should start us off quite nicely. In `Object` mode, click on the `Add` menu at the bottom of the 3D viewport, select `Mesh`, and choose `Cylinder` from the list. I created mine with `16` `Vertices`, a `15cm` `Radius`, and a `10cm` of `Depth`. Move it into position on the model and rotate it 90 degrees to sit it flat against the bottom portion of the Artifact. Part of the gear will be inside the Artifact.

2.  Now that things are getting more complicated, let's use the `Scene Outliner` to name our objects and hide the ones we aren't working on. Click on the bottom portion of the Artifact. You will see in the `Scene Outliner` that it is named `Cylinder` and everything after that is `Cylinder.001`, and so on. If we double-click the name, we can rename the object whatever we wish. I have named my components `Bottom`, `Top`, and `Gear`. Names aren't super important here, but you should name them something you can recognize. You may also notice that each object in the `Scene Outliner` has an eye next to it. Clicking on this eye will cause it to close and will hide that object in the 3D viewport. This is a great way to work on different sections of a complex game asset without the other pieces getting in your way.
