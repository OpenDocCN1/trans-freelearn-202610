And there we go – that is it for this project! At this point, we are ready to render out the final animation. If you need a refresher on how to render animations in Blender, there is a full breakdown in *Chapter 4*. The following figure is a rendered image of **Frame 80**.

![](bbox=[0.31, 0.16, 0.69, 0.47])

Figure 15.24 – Frame 80 rendered

If you would like to download this project's Blender file, you can do that here as well: https://github.com/PacktPublishing/Learn-Blender-4-Simulations-the-Right-Way/tree/main/Chapter%2015

One last thing that needs to be mentioned is the order of the simulations we created and why it's important. If any of the three simulations we used weren't created in the order that was shown in this chapter, this project would not have worked properly. If we created the cloth simulation first, it would be much harder to create a smooth transition with the brush because the plane would be flowing and moving in the wind. Likewise, the fire simulation would not work either because we needed the dynamic paint simulation to determine where the fire would be on the plane.

So, for future projects when you decide to use multiple simulations at once, be mindful of which simulation needs to be created first in order for everything to work properly!