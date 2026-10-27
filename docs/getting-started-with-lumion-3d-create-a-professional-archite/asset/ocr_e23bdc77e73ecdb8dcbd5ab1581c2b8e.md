When you try to add a material, you realize that Lumion will apply the material to the entire model. Why? Well, because when we model in SketchUp or any other 3D application, every time we create something using a default material. In SketchUp, the default material is **Default**, in Maya it is **Lambert**, and in 3ds Max it is **Blinn.**

That's why, while modeling, it is important to group objects that we know will share the same material, and then in the final stage, add a different material to each group. It doesn't have to be something fancy; just use the same material and rename it.

# Common issues

There are a few things that can go wrong while modeling. Here, we are going to see the most common problems and the means to solve them.

The first thing that may occur is missing faces. Have a look at the following figure:

![](bbox=[0.23, 0.39, 0.77, 0.62])

Can you see the difference in the SketchUp model and the Lumion model? I am sure you can easily spot that there is a face missing. The reason this happens is because this face points inside instead of pointing outside, and if you take a look at the SketchUp model, you will notice how the face has a different color. This is SketchUp's way of telling us, look, this face is pointing towards the wrong direction. How can we solve this issue?