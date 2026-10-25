# # Tweak Mesh Compression

Unity offers four different **Mesh Compression** settings for imported mesh files: **Off**, **Low**, **Medium**, and **High**. Increasing this setting will convert floating-point data into fixed values, reducing the accuracy in vertex position/Normal direction, simplifying vertex color information, and so on. This can have a noticeable effect on meshes that contain lots of small parts near one another, such as a fence or grate. If we're generating meshes procedurally, we can achieve the same type of compression by calling the `Optimize()` method of a `MeshRenderer` Component (of course, this will take some time to complete).

There are also two global settings found in **Edit | Project Settings | Player | Other Settings**, which can affect how mesh data is imported. These are named **Vertex Compression** and **Optimize Mesh Data**.

We can use the **Vertex Compression** option to configure the type of data that will be optimized when we import a mesh file with **Mesh Compression** enabled, so if we want accurate Normal data (for lighting), but have less worry over positional data, then we can configure it here. Unfortunately, this is a global setting and will affect all imported meshes (although it can be configured on a per-platform basis since it is a **Player** setting).

Enabling **Optimize Mesh Data** will strip away any data from the mesh that isn't required by the Material(s) assigned to it. So, if the mesh contains tangent information, but the Shader never requires it, then Unity will ignore it during build time.

In each case, the benefits are reducing the application's disk footprint at the cost of extra time loading the mesh since extra time must be spent decompressing the data before it's needed.

![](bbox=[0.1359, 0.6109, 0.2016, 0.6859])

3D mesh building/animation tools often provide their own built-in ways of automated mesh optimization in the form of estimating the overall shape and stripping the mesh down to fewer total polygons. This can cause significant loss of quality and should be tested vigorously if used.

# # Use Read-Write Enabled appropriately

The **Read-Write Enabled** flag allows changes to be made to the mesh at runtime either via Scripting or automatically by Unity during runtime, similar to how it is used for texture files. Internally, this means that it will keep the original mesh data in memory until we want to duplicate it and make changes dynamically. Disabling this option will allow Unity to discard the original mesh data from memory once it has determined the final mesh to use, since it knows it will never change.