2. Next, select the plane object. In the Physics panel, select Dynamic Paint, leave Type set to Canvas, and hit Add Canvas.

3. For this simulation, we will be using Image Sequence for Format, and for Resolution, let's go with 2048. The higher Resolution is, the longer it will take to bake. If you have a slow computer, I recommend setting this to 1024. This will still give you a nice image, but it won't look as sharp.

![](bbox=[0.28, 0.2, 0.69, 0.48])

Figure 13.21 – Adding the canvas

4. One thing that needs to be mentioned is Sub-Steps in the canvas settings. Normally, with fast-moving brushes, you would turn this setting up to get a nice smooth paint line. The problem is, since the location of the dynamic paint brush (the instanced cube) is being driven by the particle system and not the cube itself, the Sub-Steps value is not going to affect the simulation. This means that if the paintbrush object is moving quickly along the curve, you may get a spotty paint line rather than a smooth continuous line even if you increase the Sub-Steps value.

![](bbox=[0.26, 0.63, 0.71, 0.81])

Figure 13.22 – Fast paint line