Let's go through each one and talk about them:

+   - **Field Absorption**: This allows the collision object to absorb a percentage of a force field's strength. For example, if a wind force field is going through a collision object with the **Field Absorption** value set to 0 . 2, only 20% of the force field's strength will be able to pass through the collision. If the value is set to 1, it will absorb 100% of the force field, and no wind will be able to get through. It's also important to make sure, in the **Force Field** objects settings, that **Absorption** is checked for this to work properly.
+   - **Particle**: This tab only deals with particle systems and does not affect the cloth, so let's skip that for now.
+   - **Damping**: This option does not affect the cloth simulation either. It is only used for soft bodies.
+   - **Thickness Outer**: This option represents the boundary of the collision. Increasing this value will extend the collision boundary past the object's surface.
+   - **Inner**: This option represents the collision boundary for the inside of the object. If the cloth enters this zone, it will be pushed out.
+   - **Friction**: This denotes how slippery the cloth will be to the object. A value of 0 will make the collision behave like ice, and the cloth will slide around. A higher value will prevent sliding and increase the stickiness of the collision.
+   - **Single Sided**: When this is turned on, the collision will be based on the direction of normals. When it's turned off, normals will be ignored and the collision boundary will be over the entire object. This means that if you were to flip a plane upside down with **Single Sided** turned off, the cloth would collide with the underside of the plane rather than pass through, like it would with **Single Sided** turned on.
+   - **Override Normals**: This works with fast-moving collisions. When this setting is enabled and a collision hits the cloth, the cloth will shoot out in the direction of the normals of the collider. With a lot of testing, this setting seems to have very little effect on the outcome of the simulation.

Remember—if your collision seems to be acting strange, double-check the modifier stack and make sure it's at the very bottom! With that out of the way, let's jump straight into the **Cloth** settings and properties!

# Understanding cloth simulation settings and values

Now that we understand what a cloth simulation is all about and how collision objects work, it's time to jump into all the settings and values. You can see an overview of the **Cloth** settings here: