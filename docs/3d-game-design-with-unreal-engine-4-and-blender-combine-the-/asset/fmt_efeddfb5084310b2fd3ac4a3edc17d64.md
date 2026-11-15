
# Stitch Tool

Sometimes when marking seams and using the `Unwrap` tool, we may wish to reconnect a couple of faces in our `UV` map, such as when a seam looks obvious after we place a material on an object and we would like to move it. The `Stitch` tool can be found in the `UV` menu of the `UV Unwrap` window. Using it is a two-step process. After moving two `UV` islands (a collection of faces that are connected together in a `UV` map) that have a shared edge close to each other, we can check whether two faces have shared edges by clicking on an edge in the `UV` map and looking to see where another orange highlight appears. If you click one edge, another highlights as well; those two edges can be stitched. After lining up the edges, head to the `UV` menu and select `Stitch`, or press the `V` key. Once the tool is on, we have a few options. The `Tab` key changes whether we are stitching in edge mode or vertex mode. The `S` key turns the `Snap` mode on and off. Snapping will snap the `UV` island that was first selected to the island that was selected second. We can also choose to stitch the islands at their midpoint, by pressing `M` and turning on the `Midpoint` option. Finally, pressing the `I` key will swap which island will move during stitching. Once we have the settings the way we would like, pressing `Enter` will finalize the stitch.

# Average Island Scale

![Average Island Scale](OEBPS/Images/B05189_07_04.jpg)

`Average Island Scale` can be found in the `UV` menu.

Sometimes when unwrapping a complex object, the unwrapping process will create `UV` islands that are out of scale with the rest of the map. `Average Island Scale` can restore even scaling to `UV` islands, which will minimize stretching and blurring across the textured area. Simply select the `UV` islands you wish to average, select the `UVs` menu at the bottom of the `UV Unwrap` window, and select `Average Island Scaling` or press `Ctrl+A`.

# Pack Islands

![Pack Islands](OEBPS/Images/B05189_07_05.jpg)

`Pack Islands` will attempt to arrange and scale your `UV` islands to achieve the best results.

Once we have arranged all of our `UV` islands, we can use the `Pack Islands` tool and Blender will automatically arrange and scale them to best fit the `UV` space. This is a great tool to finalize our `UV` map and get the most out of Blender's `UV` space. We will use this at the end of our process to try to achieve the best result.

# Using Smart UV Project

Before we get started unwrapping each piece of the Artifact, I would like to mention the Smart UV unwrapper one more time. Remember that we used `Smart UV Project` to give us a basic `UV` map for our crate back in [Chapter 3](#ch03.xhtml "Chapter 3. It's Time to Customize!"), *It's Time to Customize!*. This tool is great for basic shapes and still has a place in our process for unwrapping more complex objects:

![Using Smart UV Project](OEBPS/Images/B05189_07_06.jpg)

Smart UV Project still has a place inside our expanded toolbox.

Since our Artifact contains 42 different pieces, it is likely that `Smart UV Project` will be able to provide us an excellent unwrap solution for some of them. Then, using tools such as `Pack Islands` and `Average Island Scale`, we can add those objects unwrapped with `Smart UV Project` to our overall `UV` map.

# Custom Marking Seams

Let's take a look at using seams effectively. Remember that the `Unwrap` tool will essentially flatten your object for texturing, splitting, and flattening along the lines we have marked as seams. For different primitive shapes such as cubes and cylinders, there are some best practices to be aware of. Knowing these standard unwraps can help us later when we run into more complex shapes, such as our tentacles.

## Unwrapping Cubes

![Unwrapping Cubes](OEBPS/Images/B05189_07_07.jpg)

Practice makes perfect! Cubes and other primitive shapes are a great way to brush up on UV unwrapping.

When unwrapping a cube, it is important to minimize the number of seams used. This is why we unwrap in a cross shape with the top face at the center. Let's try it!

1. Head up to the `Scene Layout` dropdown and select `UV Editing` from the list.
2. Select the default cube in the scene and head to `Edit` mode. Choose `Line selection` from the selection modes at the bottom of the screen and locate the top face. This will be our reference point as we add our seams.
3. Locate the front face. This can be done by pressing `1` on the number pad or selecting `Front` from the `View` menu. This will be our second point of reference as we unwrap:
   ![Unwrapping Cubes](OEBPS/Images/B05189_07_08.jpg)
   Select the front two edges and mark them as seams.
4. Time to create our first seams. Now that we know where our top and front faces are, let's add our first two seams there. Select the two edges shown in the preceding screenshot, click on the `Shading / UVs` tab in the `Tools` panel, and click on the `Mark Seams` button. This will turn the lines red.
5. Now mark the other two vertical edges on the back face. This will create the cross layout we want.
6. Lastly, we want to detach the bottom face from all the other faces except the front. Select the three edges that connect the bottom face to the side and back faces. Mark those as seams.
7. Let's `Unwrap`! Click on the face selection mode and press `A` to select all of the cube's faces. Now press the `U` key and select `Unwrap` from the menu. Your `UV` map should look like our unwrapped cube.

### Tip

Turning on the `Stretch` option in our `UV Layout` view will shade the selected objects' `UV` islands blue, making them easier to see. Turning on `Keep UV and Edit mode mesh selections in sync` will help as well.

## Unwrapping Cylinders

![Unwrapping Cylinders](OEBPS/Images/B05189_07_09.jpg)

An unwrapped cylinder. Many of the Artefact's 42 shapes will be similar to this.

Unwrapping a cylinder is even easier than unwrapping a cube. To unwrap this type of primitive shape, create your seams all the way around the top and bottom faces and then one seam that runs from top to bottom. Try it out and see if your results are similar to what is shown in the preceding screenshot. It's kind of like putting a label on a soup can.

### Tip

Want more practice UV unwrapping? It can be good practice to learn to unwrap all the primitives Blender has in the Mesh portion of the `Add` menu!

Once we have a good handle on unwrapping basic primitives, it's time to start tackling the Artifact, piece by piece!

![Unwrapping Cylinders](OEBPS/Images/B05189_07_10.jpg)

Results of unwrapping the bottom.

Let's get started with the bottom portion:

1. In `Object` mode, select the bottom portion of the Artifact and press `Shift+H`. This will isolate our selection by hiding everything that is not selected. Next, head up to the `Scene Layout` drop-down box and select `UV Layout`. This will split your screen into two large viewports: one for manipulating your object and another for working with your `UV` map:
   ![Unwrapping Cylinders](OEBPS/Images/B05189_07_11.jpg)
   Marking the seams on the bottom portion of the Artefact.

   ### Tip

   You can isolate a piece of your model by selecting it in `Object` mode and pressing `Shift+H`. To make everything visible again, use `Alt+H`.

2. Time to cut some seams. Unwrapping this part of our creation is not unlike unwrapping a cylinder. Let's first create the seam to detach the inside. Select the top edge loop by selecting `Line` mode, click the first line of the edge loop, hold the `Alt` key, and select the second line of the loop. This will select the entire loop for you. Now head to the tools panel and select the `Shading/UVs` tab. Now click on `Mark Seam`. This will highlight your selected edge loop in red, letting you know that Blender will use this loop as a seam in your `UV` unwrap.
3. Now repeat the process to select the edge around the bottom face and turn that into a seam as well.
