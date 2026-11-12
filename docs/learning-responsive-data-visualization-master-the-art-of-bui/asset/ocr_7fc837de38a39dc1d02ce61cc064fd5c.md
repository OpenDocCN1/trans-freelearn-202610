# # Orthographic projection

Now, we continue the brain experiment from before. Let's move to a 3D projection onto a 2D plane, which is called orthographic projection. As we can see in the following figure, when using orthographic projection, we lose the information about one side of the sphere:

![](bbox=[0.263, 0.164, 0.794, 0.568])

The Orthographic projection

We can create an orthographic projection in D3 using the `d3.geo.orthographic()` function.