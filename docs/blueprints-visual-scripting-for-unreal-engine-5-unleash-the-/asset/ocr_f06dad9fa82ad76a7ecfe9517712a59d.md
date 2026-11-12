Let's change the **Projectile Gravity Scale** from `1.0` to `0.1`. That way, the bullet appears very light and thus not affected by gravity much.

Additionally, you might have noticed that the current projectile bounces off walls and objects as if it were a rubber ball. However, we want to mimic a harder and more forcefully impacting projectile. To remove the bouncing, look for the **Projectile Bounces** section in the **Details** panel and uncheck the box next to **Should Bounce**. The other values dictate the way in which the projectile bounces only if **Should Bounce** is checked, so there is no need to adjust them.

One last change to make in **Event Graph** is to connect the **False** pin of the **Branch** node to the **DestroyActor** function so that the projectile is always `Destroyed` when it collides with anything:

![](bbox=[0.123, 0.406, 0.866, 0.69])

# Figure 6.17 – Projectile Event Graph

Now, compile, save, and click on **Play**. You will find that shooting the gun results in a much further-reaching projectile and that the projectile does not bounce off the walls anymore.

We modified the projectile's speed and unchecked the bouncing property to make the projectile behaves like a bullet. The next Blueprint we will modify is the `BP_CylinderTarget` to simulate an explosion when it is destroyed.