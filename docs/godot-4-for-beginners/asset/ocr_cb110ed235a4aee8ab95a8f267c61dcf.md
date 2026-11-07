> Note
> 
> In 2D, the positions and sizes of objects are measured in *pixels*, whereas in 3D, Godot uses the *metric system*, so each unit is 1 meter and represents an area of 1 meter by 1 meter.

We could make a 3D stick man using multiple of the simple shapes in *MeshInstance3D*. Have a look at the following result and try to recreate it yourself using the *Selection*, *Move*, *Rotate*, and *Scale* tools. You *do not* have to use the same shapes as I have, so your stick man can look very different from mine, and that is okay.

![](bbox=[0.24, 0.34, 0.76, 0.74])

*Figure 3.8 – Making a stick man with simple mesh shapes*

As you can see in *Figure 3.8*, I have combined different basic shapes to create a stick figure.

Note that if you select the *parent StickMan* node, which is the original *Node3D*, whatever you do to it – such as using *Scale* or *Rotate* – it will do to all of the child nodes.