Figure 14.8 shows a water simulation made in WebGL by Evan Wallace (the code is available at https://github.com/evanw/webgl-water/blob/master/index.html):

![](bbox=[0.22, 0.14, 0.78, 0.49])

Figure 14.8: Realistic water with waves and underwater caustics (courtesy of Evan Wallace)

In Figure 14.8, the waves of the sphere falling into the water can be seen. Also, the refraction of the walls created by the water and the refraction of the light on the bottom of the pool are clearly visible.

# Adding stunning post-processing effects

For even more realism in the virtual world, post-processing effects can be added to the renderers. The list of possible effects is long, so here is just a brief list of ideas that can be implemented quite quickly while creating great visuals:

+   - Lens flares for the illusion of looking through a camera
+   - God rays: visible rays in haze when the direct view into the sun is blocked
+   - Bloom effect simulating glowing objects
+   - Motion blur blurring the image when the view is moved
+   - Depth of field blurring the world around a sharp center
+   - Screen Space Ambient Occlusion darkening gaps and edges
+   - Screen Space Reflection, a cheap way to create reflective surfaces