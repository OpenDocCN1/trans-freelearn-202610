+   7. In **Liquid Settings**, set **Narrow Band Width** to 8. This will increase the number of particles in the simulation, giving us a better result. Also, enable **Fractional Obstacles**. This will make sure fluid doesn’t stick to any **Effector** objects.
+   8. Now, let’s add foam particles! Scroll down till you see the **Particles** panel and check **Foam**. We don’t need **Spray** or **Bubbles** particles; I found that only using **Foam** works best for this simulation.
+   9. Set both **Potential Radius** and **Particle Update Radius** to 3. This will help smooth out the particle grids, giving us a better result. Sometimes, particles will flicker around, and setting this value higher will help prevent that.
+   10. Next, let’s set **Wave Crest Particle Sampling** to 300. This will give us more particles in the crest of the wave. The following screenshot gives an overview of the settings:

![](bbox=[0.11, 0.29, 0.83, 0.55])

Figure 4.6 – Foam particles

The remaining settings and values can remain at their default states.

+   11. Now, open up the **Mesh** panel and make sure **Upres Factor** has a value of 2. This will work nicely in this simulation, but if you find the mesh looks a bit **low polygon** (**low poly**), you can increase this value.
+   12. With the **Mesh** panel checked, a small mesh around each **Liquid** particle will be created. With a **Particle Radius** value of 2, the mesh will look a bit blobby and not very realistic. Let’s bring this value down to 1.2. Now, the radius around each particle will be much smaller, and this will produce a much better result. View the following screenshot for an example. The left side has a **Particle Radius** value of 3 and the right has a value of 1: