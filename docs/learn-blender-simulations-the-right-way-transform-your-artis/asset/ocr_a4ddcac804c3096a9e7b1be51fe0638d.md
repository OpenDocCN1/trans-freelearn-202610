![](bbox=[0.16, 0.09, 0.8, 0.36])

Figure 11.17 – The elevator rising

Next up, we will animate a cube smacking into the sphere and shooting it along the platform!

# Animating the rigid bodies

When the sphere reaches the top of the elevator, we are going to have a cube slide out and hit it, sending it around the half circle! To do this, follow these instructions:

+   1. Firstly, we need to extend the rigid body simulation because currently it only lasts for 250 frames. So, open the Scene Properties panel and, in the Rigid Body World tab, underneath Cache, set End to 500.

![](bbox=[0.24, 0.6, 0.73, 0.8])

Figure 11.18 – The Cache panel

Now, the simulation will last for 500 frames rather than only 250.