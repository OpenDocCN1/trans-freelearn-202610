+   8. Now, let's give it a new particle system by clicking the + sign in the **Particle System** panel just like before.
+   9. We are going to be doing something very similar to the previous system. Set **Number** to 2000, **Frame Start** to 10, **End** to 14, and **Lifetime** to 5.
+   10. In the **Velocity** tab, set **Normal** to 7 once again, but set **Randomize** to 3. This will give a tighter spread of particles. It might be hard to see what this looks like with the first particle system, so you can hide the icosphere temporarily by clicking the *eye* icon in the **Outliner** at the top right:

![](bbox=[0.3, 0.23, 0.66, 0.32])

Figure 5.7 – Hiding objects

The circle particle system should look something like this now:

![](bbox=[0.19, 0.38, 0.78, 0.63])

Figure 5.8 – Circle particle system

+   11. Let's duplicate the circle twice (*Shift + D*) and place one rotated upward and the other rotated left of the icosphere. Feel free to play around with the position, rotation, and scale of these circles to get more variation.
+   12. Unless you are creating an explosion in the air, we need a floor for the particles to hit. Press *Shift + A* and add a plane. Scale it up pretty big and make sure it's below all the objects.
+   13. To add collision to this plane, head over to the **Physics** panel and choose **Collision**. In the **Particle** tab, set **Friction** to 0.9; this will make sure the particles don't slide around the plane.