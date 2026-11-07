![](bbox=[0.248, 0.089, 0.719, 0.262])

Figure 4.12 – Effector settings

With that done, we are now ready to bake in the simulation! Head back over to the Domain settings and click on Bake Data at the top. Remember—if you want to save the cache from being deleted, make sure to set a custom directory in the Cache panel.

Once the initial bake has finished, you will need to scroll down to the Particles panel and Mesh panel and bake both of those as well. After all three bakes are done, make sure to play the animation a little bit and double-check that everything looks good. If it does, continue by baking in the particles and the mesh. When everything is done baking, we will create foam particles! Also, don’t forget to save the Blender file by pressing Ctrl + S or by going up to File | Save!

# Adding foam particles

Now that the simulation is done baking, we can set up the foam particles. Foam particles add another level of detail to the simulation. These particles will be added near the top of the fluid, acting like foam or whitewater. Just like in real life, foam or whitewater is created in the midst of rapids or turbulence in the fluid and that is what we will be replicating.

At the moment, the particles are just halos, which means they are just points in three-dimensional (3D) space and won’t show up in the render. You can see an example of this here: