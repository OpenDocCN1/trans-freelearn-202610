These steps complete our implementation to animate a zoomed view. Now, let's make some adjustments to the FirstPersonProjectile Blueprint.

# Increasing the projectile's speed

Now that we have given the player character a new gameplay option to navigate the world, our focus will be back on the shooting mechanics. Right now, the shots fired from the gun on the controller are spheres that slowly arc through the air. We want to better approximate the fast-moving bullets that we are used to from traditional shooters.

To change the properties of Projectile, we need to open the Blueprint called FirstPersonProjectile, which is in the Content|FirstPersonBP|Blueprints folder. Once opened, look at the Components panel and click on Projectile. This is a projectile movement Component that has been added in the FirstPersonProjectile Blueprint to define how the sphere will travel once it is created in the world:

+   1. In the Details panel, you will see that Projectile has a lot of variables that describe its movement. We are interested in only a few of these at this time: