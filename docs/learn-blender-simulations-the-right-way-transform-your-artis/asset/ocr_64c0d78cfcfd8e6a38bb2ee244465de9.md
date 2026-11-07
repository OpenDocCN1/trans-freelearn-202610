# Rendering the animation

Now would be a good time to match the simulation to the end frame. Since we baked 150 frames, let’s set the End frame property in the Timeline to 150 as well:

![](bbox=[0.25, 0.178, 0.715, 0.28])

Figure 1.21 – Timeline example

Next, let’s choose which rendering engine to use. There are two default render engines to choose from in Blender: EEVEE and Cycles. Both have their strengths and weaknesses. For example, EEVEE will render much faster than Cycles, but the fire won’t look as good. If you have a slower computer, I recommend using EEVEE because rendering smoke/fire in Cycles is pretty GPU/CPU-intensive. To choose a rendering engine, head over to the Render properties section and click Render Engine:

![](bbox=[0.28, 0.43, 0.688, 0.58])

Figure 1.22 – Render Engine

For my animation, I used EEVEE, but if you want to use Cycles, I will provide instructions for both!

# Cycles

With the Cycles engine selected, let’s change some of the settings so that Blender will render faster and get a better result:

+   1. Open the Sampling panel and, in the Render tab, set the Max Samples number to 20.
+   2. Open the Color Management tab, set View Transform to Filmic, and set Look to High Contrast. This will give the fire much more of a pop and make it not look as dull.

Now that you’ve completed the steps for rendering in Cycles, feel free to skip the EEVEE instructions and head on over to the Setting the output section!