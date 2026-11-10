# Exporting a 2D sprite animation from a 3D package

Drawing animated 2D sprites can be a challenge. Some artists create their characters in a 3D package and export them as 2D sprites to make the animation process easier. In this first recipe, we will create such a character and export it's animation as a sprite sheet.

# Getting ready

Before we start, you need to have an animated model in your chosen 3D package (we are using Blender). You can also download the provided example; open the project in Unity; go to the `Chapter 03 2D and user interface animation\Recipe 01 Exporting a 2d sprite animation from a 3d package` folder. You will find a scene called `Example.unity` there, with an animated coin sprite. In the `Animation` directory, you can find exported frames and a sprite sheet containing them. You can also find a `*.blend` file with the coin model; set to render out the animation.

# How to do it...

To export a 2D sprite animation from a 3D package and import it into Unity, follow these steps:

+   1. Create an animated object in your chosen 3D package.
+   2. Set the camera to render out the animated object. The camera can be perspective or orthographic, depending on what you would like to achieve. In this example (a rotating coin), it is set to perspective to better show the animation. Try to have the center of your object in the center of the frame.
+   3. Render the animation into separate frames with a transparent background (often 12 frames per second is enough for a good quality sprite animation). Use a logical naming convention (for example, `frame_01.png`, `frame_02.png`, and so on). All the frames should have the same size in pixels.
+   4. Create a new image in a chosen image editing software (we are using GIMP in this recipe). The image should have a transparent background.