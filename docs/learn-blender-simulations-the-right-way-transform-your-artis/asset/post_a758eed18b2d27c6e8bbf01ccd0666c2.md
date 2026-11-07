
Both **Format** types have their advantages and disadvantages, which we will talk about once we get into the settings!

After you decide which **Format** type to use, you can then select which **Surface Type** option you want!

![](img/2bdfac576180ebfe5ee8960a6eec7f99_295_0.png)

Figure 12.3 – Surface Type settings

With the **Vertex** format selected, there are four **Surface Type** options (**Weight** is not available with the **Image Sequence** format):

- The **Paint** type is exactly what you would expect. It outputs color information that is displayed on the canvas. This can be used to create paintbrush effects, cool and interesting materials, and a huge variety of other things!

![](img/2bdfac576180ebfe5ee8960a6eec7f99_295_1.png)

Figure 12.4 – An example of the Paint type in action

- The **Displace** type displaces your mesh based on the size of the **Brush** object. This can be useful for creating tire tracks, footprints, and much more!

![](img/2bdfac576180ebfe5ee8960a6eec7f99_296_0.png)

Figure 12.5 – An example of the Displace type in action

- **Weight** allows you to create a dynamic weight paint map. Blender uses weight paint for almost everything – simulations, particle systems, modifiers, materials, and more. For example, in *Chapter 15*, we will be using **Weight** to control where the fire simulation is on our mesh. Using this **Weight** type can be extremely powerful and can allow you to create some very interesting simulations!

![](img/2bdfac576180ebfe5ee8960a6eec7f99_296_1.png)

Figure 12.6 – An example of the Weight type in action

- And finally, **Waves** allows you to create waves as if your brush was moving through water. This can be very useful for creating ripples in an image of water, raindrops, or skipping stones on a lake.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_297_0.png)

Figure 12.7 – An example of the Waves type in action

There are a lot more ways to customize the canvas to your liking, which we will discuss in this chapter! Before we dive into all the settings and values, let’s quickly cover the Brush object!

To add a dynamic paint brush, select a mesh object, then head over to the Physics panel and select Dynamic Paint. Change Type to Brush and click Add Brush:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_297_1.png)

Figure 12.8 – The Brush settings

In the settings, you can change **Paint Color**, the **Proximity** around the brush that will affect the canvas, the **Depth** of the displacement, and the strength **Factor** of the waves. You can even use a particle system as a **Brush**! This could be very useful for creating raindrops and particle paint effects.

Are you starting to see the power of dynamic paint and all its potential in Blender? There are so many ways to use this physics system to create interesting and dynamic (pun intended) animations!

Now that you have an understanding of what dynamic paint is all about, let's create a simple paint simulation together!

## Creating a paint simulation

Creating a basic dynamic paint simulation is a little tricky to set up, so let's create one together and go over the process! To make things easier to understand, I recommend downloading the `Paint Startup.blend` file from here: https://github.com/PacktPublishing/Learn-Blender-4-Simulations-the-Right-Way/tree/main/Chapter%2012.

This file includes a basic animation of a sphere following a curve on a plane:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_298_0.png)

Figure 12.9 – The Paint Startup.blend example

Let's start out by adding a dynamic paint simulation to the plane and the sphere object:

1. Select the **Plane** object and head over to the **Physics** panel. Select **Dynamic Paint**, make sure **Type** is set to **Canvas**, and then click on **Add Canvas**:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_299_0.png)

Figure 12.10 – Adding the canvas object

2. Let’s do the same thing for the Sphere object, except let’s set Type to Brush and click Add Brush!

3. Even though we have a brush and a canvas in the scene, we can’t see anything when we play the animation. This is because we need to set up the Output details for the color information first! When using the Vertex format, these values are output to the Color Attribute field of the object.

Select the Canvas object and head down to the Output tab at the bottom:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_299_1.png)

Figure 12.11 – The Output tab

4. Click on the + icon for both Paintmap Layer and Wetmap Layer. Paintmap Layer is just the basic color information from the brush and Wetmap Layer gives us values that determine how “wet” or how “dry” the paint is. We will cover both of these layers in more detail later, in the Surfaces section of this chapter!

5. With that done, we are now outputting the paint and wetness information to the vertex colors! You can also see this by looking at the Object Data Properties panel in the Color Attributes tab:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_299_2.png)

Figure 12.12 – The Color Attributes tab

When using the **Vertex** format, we are directly affecting the color of each vertex of the mesh. These colors are then output as **Color Attributes**, as you can see in *Figure 12.12*. Keep in mind that the color is only affecting the vertices, not the edges or faces. If you have a large gap between the vertices, there will be a gradient of color to the connecting vertices.

6. If you would like to see this paint color affect the canvas, you could do so in a couple of ways:

- You can open the material node setup, add a **Color Attribute** node (see *Figure 12.13*), and select **dp_paintmap** from the dropdown menu. You can also select **dp_wetmap** in the **Color Attribute** node as well, if you would like to preview what the wetmaps look like.

Now the paint color will appear when you render.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_300_0.png)

Figure 12.13 – The Color Attribute node

- Another way to see the paint in the viewport is to switch to the **Vertex Paint** mode in the top left corner. You can toggle between the **dp_wetmap** or **dp_paintmap** by selecting from the dropdown menu.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_301_0.png)

Figure 12.14 – Vertex Mode

- And, lastly, if you don’t want to be stuck in **Vertex Paint** mode, you can head to the **Viewport Shading** menu and select the **Attribute** mode:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_301_1.png)

Figure 12.15 – The Attribute mode in the Viewport Shading menu

Now, you should be able to see the brush painting on the canvas in **Object** mode!

![](img/2bdfac576180ebfe5ee8960a6eec7f99_302_0.png)

Figure 12.16 – A Brush object painting on the canvas

If you would like to see how the wetmap layer looks, you can do that by selecting **dp_wetmap** in the **Color Attribute** tab. Take a look at Figure 12.12 to see that menu. Here's a preview of what a wetmap would look like:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_302_1.png)

Figure 12.17 – A wetmap preview

Now that we understand how to set up a dynamic paint simulation, let's go over the settings, starting with the canvas!

## Understanding the canvas

We have covered how to create a dynamic paint simulation, but let's now get a bit more technical and discuss all the settings for the Canvas object:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_303_0.png)

Figure 12.18 – The canvas settings

### Settings

Starting at the very top, let's cover the Settings tab:

- **Remove Canvas**: This obviously allows you to remove the canvas from the object you have selected.
- **Surface layers**: Below **Remove Canvas**, you have your surface layers. In *Figure 12.18*, the name of the layer is **Surface**; you can rename it by double-clicking on it. You can add or remove layers by clicking the + and – buttons on the side and you can also toggle the visibility by checking the checkbox.

These layers all work independently from each other, meaning you can have multiple layers with different surface types, and you can change the settings for each individually. For example, in the following figure, there are three surface layers. One is set to **Paint**, another set to **Displace**, and the last is set to **Waves**:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_303_1.png)

Figure 12.19 – Three surface types
