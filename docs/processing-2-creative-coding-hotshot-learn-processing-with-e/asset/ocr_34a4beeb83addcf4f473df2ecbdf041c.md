# # Objective Complete - Mini Debriefing

In this task, we defined a class for our stick figures. We moved the code that accesses the 3D coordinates and the drawing of the dancer to our class to simplify our `drawDancer()` method in the main sketch. The new class also enabled us to draw more than one dancer.

To prevent our dance company from looking like an army of clones, we added the `randomize()` method, which scales the stick figures a bit and chooses a random color for each one.

# # Mission Accomplished

In this project, we taught Processing how to see by connecting the Kinect and installing the OpenNI framework.

We used the depth image provided by the SimpleOpenNI framework and located the user in the image using the tracking capabilities of OpenNI. For every tracked user, the framework provides us with a bitmap defining which pixels belong to the tracked user and which don't. For the last two tasks, we used the depth image with the colored pixels that we created in the second task as an HUD display to enable the player to see what is currently being tracked.

Starting with our third task, we used the skeleton tracker to access the 3D coordinates of the player's limbs and joints and used the information to draw a stick figure that followed the player's moves.

In our final task, we refactored the drawing code for our stick figure to a class and used this class to draw multiple dancers so that our stick figure had some company while dancing.

# # You Ready to go Gung HO? A Hotshot Challenge

In this project, we only scratched the surface of the possibilities that Kinect offers. It's impossible to describe all of it in one project; you could write a whole book about it. So try to expand the knowledge you've gained in this project to try one of the following:

+   - Use the image of the RGB camera to display the player instead of one of the stick figures.
+   - Use a more complex 3D model rather than a stick figure for your dancers.
+   - Record the moves of the player and replay them after a short delay.
+   - Currently, our sketch supports only one player. The OpenNI framework can also track multiple users, so try to add multiplayer support.