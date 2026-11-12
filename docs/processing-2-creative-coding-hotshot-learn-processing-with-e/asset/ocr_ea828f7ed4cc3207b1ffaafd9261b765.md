In the second task, we filled the faces of our sphere and added a point light source to our world. We then added normal vectors to each vertex to smoothen the shading of our mesh. To make the lighting more even, we switched from our point light source to the default light sources in Processing.

In the third task, we turned our sphere into a globe using a world map as a texture. We extended the vertices of our mesh by the texture coordinates to make sure that the texture fits around our sphere. We also added satellite images from NASA's Visible Earth project and added a callback method that handles key press events to make the textures switchable.

In the fourth task, we finally created the visual effects we were aiming for by adding two GLSL shader-based filters to our draw() method. In our setup() method, we inverted the texture image and turned it green by adding a tint to it. We created a grid of blue lines by reactivating the stroke color in our makeSphere() method. Then, we added an edge detection GLSL shader that we used in our draw() method. To generate the glow effect, we created a second shader and used it to filter the result of our first filter.

We also added a Boolean variable that allowed us to activate the filters only when the first texture is selected and deactivate them when the satellite images are visible.

# You Ready to go Gung HO? A Hotshot Challenge

In this mission, we learned how to create a globe and how to use GLSL shaders as filters in Processing. We have only scratched the surface of what is possible with shaders using a modern graphics card; there is so much more that can be done with a globe besides making it rotate and look awesome. Why don't you try one of the following ideas to extend the sketch we have just created:

+   Create a "sun" that lights up the globe correctly based on the time of day, and use a GLSL filter that uses the day and night texture images and blends between the two at the day-night border.
+   NASA provides satellite images for the various planets and moons of our solar system to download for free. Why not make a whole solar system?
+   Create a different set of GLSL filters to recreate the hand-drawn black and white globes from the weekly newsreels in the cinemas of the 50s.