13. With all that done, the **Soft Body Sphere** object should follow the curve at **Frame 420** and make its way through the **Glass Tube** object. However, we also want to turn off **Goal** when it reaches the end. To do this, play the animation until the sphere is about to exit the tube.

![](bbox=[0.27, 0.16, 0.71, 0.4])

Figure 7.22 – End position

Add a keyframe to both **Min** and **Max** with a value of 0.5.

14. Then, on the next frame, let's set the **Min** and **Max** values to 0 and add another keyframe. Now the **Goal** will turn off, and the sphere will act like a normal soft body.

15. You may notice that when we play the animation, the sphere jumps to the right just slightly. This is because the origin point is moving right at the start of the simulation.

![](bbox=[0.27, 0.57, 0.73, 0.83])

Figure 7.23 – Sphere start

To fix this, all you have to do is set the **Simulation Start** value in the **Cache** tab to 0 instead of 1.