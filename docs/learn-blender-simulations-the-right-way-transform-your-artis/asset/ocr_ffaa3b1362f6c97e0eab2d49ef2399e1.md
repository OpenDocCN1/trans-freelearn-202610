![](bbox=[0.225, 0.095, 0.74, 0.235])

Figure 13.6 – Offset Factor 1

Now, when you restart and play the animation, the brush will move to the position and start following the curve! This is what it should look like on Frame 120:

![](bbox=[0.192, 0.33, 0.775, 0.55])

Figure 13.7 – Brush following the curve

Awesome! We now have the brush following the curve all the way around! Again, feel free to customize the curve shape and the position of the keyframes to your liking. But also make sure not to make the brush move too quickly, as this will cause issues with dynamic paint, which we will talk about in the Simulating Dynamic Paint section. Once you are happy with it, we can move on to creating hair particles, which we will use to paint on the plane object.

# Creating hair particles

Creating the bristles on a paintbrush may seem complex and hard to do, but when we use a hair particle system, we can easily add thousands of bristles to create that look. We can also add physics to the hairs so that they bend and deform as they collide with the plane, giving the look of a real paintbrush. Let’s jump straight into it:

+   1. First, select **Particle Object**, located at the bottom of the brush. This is the object that we will use to emit the hair particles.