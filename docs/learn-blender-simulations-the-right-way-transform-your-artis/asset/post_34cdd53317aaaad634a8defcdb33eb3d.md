
III. Take **Fac** from **Noise Texture** and plug it into the **Height** on the **Bump** node. Then take the **Normal** output and plug that into the **Normal** input of the **Principled BSDF** shader.

IV. Now for the values. Set **Scale** to 500, **Roughness** to 0.9, and **Strength** of **Bump** to 0.3. Here is the full setup.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_353_0.png)

Figure 13.35 – Adding Bump

If you would like to make the animation a bit more interesting, you could have the **Image Sequence** node plug into the **Emission** input of the **Principled BSDF** shader. This will give the look of the brush painting glowing paint on the canvas!

![](img/2bdfac576180ebfe5ee8960a6eec7f99_353_1.png)

Figure 13.36 – Glowing paint material

Another idea would be to plug an Image Texture node into the B color input of the MixRGB node. So, instead of painting a solid red color, the brush will paint an image texture instead! Feel free to get creative and experiment with different images to create some interesting effects!

Once you are happy with how the canvas looks, we can move on to creating the hair material!

## Creating the hair material

This next material we will create is pretty easy to set up! What we will be doing is creating a gradient effect where the top of the hair is one color, and it blends into another color near the bottom:

- 1. To get started, let's first hide the instanced cubes so we can see the hair particles. Up in Outliner, click on the Eye and Camera icon to hide this object. This will hide it from the view and from the render as well.
- 2. Next, select the hair particles and click New to create a new material.
- 3. To create that gradient effect, there are two nodes we need to add. The first one is a Curves Info node. Press Shift + A, go to Input, and select Curves Info.
- 4. The other node we need is Color Ramp. Add it between Curves Info and the Principled BSDF shader.
- 5. In the Curves Info node, the Intercept output is what we need. Let's plug it into Color Ramp, then take Color Ramp and plug that into Base Color of the Principled BSDF shader.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_354_0.png)

Figure 13.37 – Hiding the instanced cubes

The hair particles should now have turned into a black-and-white gradient.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_355_0.png)

Figure 13.38 – Hair particle gradient

- 6. We can control the colors of the gradient with the Color Ramp handles. For the handle on the right, choose the same color as you did for the paint. As for the left handle, let's go with a light gray color. Here is the full node setup:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_355_1.png)

Figure 13.39 – Hair material node setup

That is basically all we really need to do for this material! If you want to add more to this material, you could take the Intercept output from the Curves Info node and have that control the Roughness value in the Principled BSDF shader. This can give the look of wet paint on the particles, but I will leave that up to you. For now, let's move on to the final render settings, and then we can render this animation!

## Setting up the render

This tutorial is almost done! The final steps we need to do are setting up the render, animating the camera to follow the paintbrush, and rendering the animation. Let's start by taking a look at the EEVEE settings and changing them to make the whole scene look much better:

- 1. In the **Render** panel, let's turn on **Raytracing** to get nice reflections in the materials. Set **Resolution** to **1:1**.
- 2. In the **Color Management** tab, let's set **Look** to **High Contrast**. This will really make the colors in our scene pop!
- 3. For the **Camera** animation, we recommend jumping to **Frame 1**, positioning **Camera** to the view you want, and then adding a keyframe. You can do this by selecting **Camera**, hitting *I*, and selecting **Location & Rotation**.
- 4. Around **Frame 100**, rotate and move **Camera** so that it's facing the paintbrush, then add another keyframe. Repeat this step at around **Frame 160** and then **250**. Remember, you can always move the keyframes by selecting them in **Timeline** and pressing G.
- 5. If you want to delete a keyframe, select it and press X, and choose **Delete Keyframes**.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_356_0.png)

Figure 13.40 – EEVEE render settings

![](img/2bdfac576180ebfe5ee8960a6eec7f99_356_1.png)

Figure 13.41 – Deleting keyframes

- 6. If you would like to add a nice look to the camera, enable **Depth of Field** in the **Camera Data Properties** panel. For **Focus on Object**, select **Paint Brush**.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_357_0.png)

Figure 13.42 – Depth of Field

Once you have your camera set up, you are ready to render the animation. If you need a refresher on how to render animations in Blender, check out *Chapter 4*.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_357_1.png)

Figure 13.43 – Final result

If you would like to see the final result, or if you want to download this chapter's project file, you can do that by visiting the following link: https://github.com/PacktPublishing/Learn-Blender-4-Simulations-the-Right-Way/tree/main/Chapter%2013

Feel free to add to or change up the animation however you like! Maybe add two or three paintbrushes and have the colors mix together. You could even spell out a word using the brushes! Just remember that **Sub-Steps** does not work using this method, so you will have to slow down the brush so the paint line is not spotty.

## Summary

We have now finished this tutorial and created a paintbrush animation using dynamic paint in Blender. Hopefully, now you have an even greater understanding of this powerful simulation and all the fun and unique things that can be achieved. Just like always, let’s recap so everything sinks in.

First, we learned how to have an object follow a curve using the **Follow Path** constraint. After that, we created hair particles and gave them physics, so they bend and deform. Then we instanced a long cube on each hair particle and used that as the brush in the dynamic paint simulation. We created the canvas, set up all the settings, and made an image sequence. With all the images exported from dynamic paint, we imported images into the material for the canvas and then learned how to customize the colors easily. We also discussed the hair particle material and created a nice gradient. Finally, we set up the camera and rendered the animation!

I hope you are ready because, in the next chapter, we will be moving away from **Paint Surface Type** and instead be taking a look at **Waves** and learning how to create a really nice raindrop effect!

## Creating a Raindrop Effect

For the next project in this book, we will be using dynamic paint and particles to create a realistic raindrop effect on a lake! With the knowledge gained in this chapter, you will be able to add raindrop effects to puddles on a road or create a boat traveling across an ocean, leaving a trail of waves! There are many different ways to use this effect, and it can definitely add another level of detail to your renders and animations!

We will first start by adding all the objects we will need for the scene. From there, we will learn how to use the particle system to create rainfall! After that, we will jump into the dynamic paint simulation and learn how to use the Waves surface type to create realistic raindrops. Finally, we will create a nice water material and render the animation using EEVEE!

In this chapter, we'll be covering the following topics:

- Setting up the scene
- Creating the rain particles
- Simulating the waves
- Creating the water material
- Setting up the render

## Technical requirements

This chapter requires you to have Blender version 4.2 or above installed.

To download Blender, visit www.blender.org.

Make sure to also download the startup file found here: https://github.com/PacktPublishing/Learn-Blender-4-Simulations-the-Right-Way/tree/main/Chapter%2014.
