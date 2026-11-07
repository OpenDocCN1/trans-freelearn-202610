+   - **Bake Image Sequence**: When this is clicked, Blender will export all the images from the simulation and place them in the directory set by the **Cache Path** field. You can choose which folder the images will export to by clicking the **Cache Path Folder** icon.
+   - **UV Map**: For most basic canvas objects, such as planes, spheres, or cubes, there should already be a UV map you can select. Selecting **UV Map** tells Blender how to properly export the images so they can be mapped to the canvas. If, however, you modeled or changed the canvas, you may need to UV unwrap your object. UV unwrapping is a topic that we don't have time to cover in this chapter. If you would like to learn more about this topic, here is a great resource: https://youtu.be/7JUN1j6mROU?si=-pZsq8Nw1nd5RChY
+   - **File Format**: There are two types of file formats when exporting images: **PNG** and **OpenEXR**. **OpenEXR** is a better and higher-quality image file than **PNG**, but the file size is much bigger. Normally, **PNG** will work perfectly fine for most simulations.
+   - **Premultiply Alpha**: This allows you to export transparent images so that only the paint will show up and not the canvas. And at the bottom, you can decide to either export the **Paintmaps**, **Wetmaps**, or both.

And there we go! We have now talked about everything there is to know about the **Canvas** object in the dynamic paint simulation. The last thing we will learn about in this chapter is the **Brush** object and all its settings.

# Using brushes

Now that we have covered the canvas, let's discuss the brush and all the ways to customize how it behaves in the simulation!

# Settings

Let's start by looking at the **Settings** tab:

![](bbox=[0.29, 0.7, 0.67, 0.88])

Figure 12.55 – The Brush settings