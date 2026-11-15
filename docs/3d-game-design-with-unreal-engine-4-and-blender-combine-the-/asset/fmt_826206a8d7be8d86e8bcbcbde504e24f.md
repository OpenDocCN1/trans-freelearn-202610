
## Unwrapping the Artifact Pieces

### Bottom Piece

4. Finally, complete the process by selecting edges in a vertical line that runs down from the top marked edge loop to the bottom loop. Mark this as a seam as well, just like we would to unwrap a cylinder.

5. Time to `Unwrap`! Set your `Screen Layout` to `UV Editing`. Now turn off `Face select` mode and press `A` to select all your faces. Finally, press `U` and select `Unwrap` from the menu. Check out the results and see if they are similar to the UV map shown previously.

And we're done with the bottom! Not too hard is it? We will find that many of the pieces of the Artifact unwrap very similarly to cubes and cylinders. This gives us an easy starting point for each piece, taking the guesswork out of getting started:

![Unwrapping Cylinders](OEBPS/Images/B05189_07_12.jpg)

### Top Piece

The results of unwrapping the top

The top should be just like the bottom piece:

![Unwrapping Cylinders](OEBPS/Images/B05189_07_13.jpg)

The seams are marked just like the bottom portion and it's similar to unwrapping a cylinder primitive

Let's dive right in:

1. Grab the edge loop around the top polygon and mark that as a seam for the `Unwrap` tool.

2. Select one of the bottom edge loops around where the top and the bottom pieces meet and mark this as a seam as well.

3. Create a vertical seam to connect the other two seams, similar to what we did for the bottom piece.

4. Finally, select all of the faces and use the `Unwrap` tool again. The result should be very similar to what we have for the bottom.

### Gear

Now that we have the top and the bottom pieces unwrapped, let's switch gears and take a look at the gear (switch gears, get it?):

![Unwrapping Cylinders](OEBPS/Images/B05189_07_14.jpg)

We will use `Smart UV Project` to unwrap the gear section

For this piece, we are going to use `Smart UV Project`.

1. In `Object` mode, select the gear and head into `Edit` mode. Make sure your selection mode is set to `Face` select and then press `A` to select all of our faces.

2. Press `U` to open the `Unwrap` menu and select `Smart UV Project`. Your results should look similar to the preceding screenshot.

### Pistons

This leaves us only two sets of objects left, the pistons and the tentacles. The processes for both will be similar. We will delete all but one and UV map it. When we copy the mapped section again to replace the deleted pieces, they will keep all of our UV mapping. Now that's working smart!

![Unwrapping Cylinders](OEBPS/Images/B05189_07_15.jpg)

Unwrapping the bottom portion of the piston

Now to take a look at the Piston:

1. Choose which set of piston parts you wish to unwrap and delete all the others. We will be replacing them shortly: ![Unwrapping Cylinders](OEBPS/Images/B05189_07_16.jpg)

    Unwraps just like a cylinder!

2. Start with the bottom piece of the piston. The UV map for this will be similar to a cylinder. Starting at the top, select the top edge loop where the sides and the top meet and mark that as a seam.

3. Next, select the edge loop right before the bottom starts to curve away from the side, and mark that a seam.

4. Finally, open the `Unwrap` menu and select `Unwrap`!

5. Now, delete the top part of the piston and copy the bottom, since the top is just a duplicate of the bottom.

6. This next part covers a few steps. For each of the smaller middle sections, they are simply cylinders and unwrap using the method we spoke of earlier in the chapter. Unwrap all three cylinder sections: ![Unwrapping Cylinders](OEBPS/Images/B05189_07_17.jpg)

    Duplicating the piston parts, just like we did before

7. With all of the parts unwrapped, select all of the piston parts in `Object` mode and duplicate them to replace the pistons we deleted. Congratulations! We have now unwrapped seven pistons!

### Tentacle

Lastly, let's take a look at what it will take to unwrap the tentacle:

![Unwrapping Cylinders](OEBPS/Images/B05189_07_18.jpg)

The unwrap for the tentacle. Don't be scared!

Although it may look complicated, it is not much different than a cylinder. However, there is a bit of prep we have to do before we unwrap it:

1. Select one tentacle piece and delete the others. We will unwrap one and copy it: ![Unwrapping Cylinders](OEBPS/Images/B05189_07_19.jpg)

    Delete every tentacle except for one

2. We are going to use the `Decimate` modifier to reduce the triangle count on our tentacles. Select the `Modifier` menu (the small wrench) over in the Properties pane. Click on the `Add Modifier` dropdown and choose `Decimate` from the options. In the `Decimate` modifier itself, turn on the `Triangulate` option. UE4 will triangulate it anyway, so why not have `Decimate` get a head start. The goal of using `Decimate` is to find a ratio that lowers the number of tris that are part of an object, while still maintaining its overall shape. Now, click and slide on the `Ratio` option. We are going to `Decimate` our tentacle by lowering the ratio to somewhere between `.70` and `.80`. This will remove about 20-30% of the triangles and make the entire tentacle more manageable for UE4. Once you have found a Ratio that looks good, click on `Apply`: ![Unwrapping Cylinders](OEBPS/Images/B05189_07_20.jpg)

    Marking the seams on the tentacle. Again, it is very similar to a cylinder.

3. Surprisingly, unwrapping our tentacle is not unlike unwrapping a cylinder, and we will treat it roughly the same way. First, turn off the modifiers we placed on the tentacle by clicking on the eye icon on each of the modifiers in the `Modifier` menu. Then, select the edge loops around the top and bottom polygons and make them seams. Finally, create a vertical seam to connect these two seams, press `U` to bring up the `Unwrap` menu, and select `Unwrap`.

Each of our pieces is now unwrapped and ready to go! Back in [Chapter 3](#ch03.xhtml "Chapter 3. It's Time to Customize!"), *It's Time to Customize!* we exported our shape out of Blender as a `.FBX` file and then brought it into Unreal. This time, we are going to take a look at the different types of texture map that go into making a top notch material and create something spectacular using Allegorithmic's Substance Painter.

# Using different maps to create a more realistic look

Materials in Unreal Engine 4 are made of several different textures working together to provide a realistic result. Generally, a material for a game asset has at least two to three different texture maps that are being manipulated by different mathematical nodes. These maps are usually a `Diffuse map`, a `Specular map`, and a `Normal map`:

![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_21.jpg)

The coloring of the Artifact is determined by the Diffuse map

### Diffuse Map

Diffuse maps are the basic colors that we have painted on our objects. Unreal accepts these textures in the `Material Editor` as the object's `Base Color`. These maps generally contain color and pattern information that is painted directly onto a copy of the object's UVs in programs such as Photoshop. Simply put, this type of map contains the surface colors of our object.

### Specular Map

Next is the Specular map:

![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_22.jpg)

The Specular map controls how light behaves when it hits a surface

Specular maps control how shiny or dull a surface appears to be and can be hand painted or exported out of different programs, such as Autodesk Mudbox. These maps are a great way to make parts of an object appear as if they have been shined or cleaned. When looking at a specular map, the lighter areas of the map are where players will see a shiny surface, whereas the darker areas of the map will appear dulled.

### Normal Map

Finally, there is the normal map:

![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_23.jpg)

The normal map can help fake some details without adding to our tri count
