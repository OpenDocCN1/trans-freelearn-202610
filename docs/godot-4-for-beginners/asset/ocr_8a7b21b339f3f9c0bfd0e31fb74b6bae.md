5. Click on the Mesh menu at the top of the screen and click on Create Collision Shape... as shown in Figure 8.10:

![](bbox=[0.2, 0.15, 0.85, 0.31])

Figure 8.10 – Autogenerating a collision shape for the Bridge scene

After clicking on Create Collision Shape..., a pop-up menu appears. For Collision Shape placement, choose Sibling, and for Collision Shape Type, choose Trimesh. This is shown in Figure 8.11:

![](bbox=[0.35, 0.44, 0.71, 0.6])

Figure 8.11 – Options for Godot to create the collision shape

+   - Choosing **Sibling** ensures that the generated collision shape is added as a separate node alongside the mesh, rather than as a child of it. This keeps the scene hierarchy clean and makes it easier to manage the mesh and its collision shape independently.
+   - Selecting **Trimesh** as the collision type is ideal for static 3D models (such as level geometry) that are not meant to move. A **Trimesh** collision creates a detailed shape that matches the mesh surface, which allows for accurate collision detection. However, it’s best used only for static objects, as it is computationally expensive and not suitable for dynamic or moving objects.