The following image shows an example of a complex graphical mesh that has been given a much more simplified mesh for its Mesh Collider:

![](bbox=[0.139, 0.164, 0.861, 0.452])

Simplifying the rendered mesh into convex shapes with lower polygon counts in this way will greatly reduce the overhead needed to determine bounding volume overlaps with other Colliders. Depending on how well the original object is estimated, there should be minimal noticeable gameplay differences, especially in the case of this axe, which we expect to be moving quickly as creatures swing it during attacks, making it unlikely that players will notice the difference between the two meshes as Colliders. In fact, the simplified mesh is much less likely to be missed by Discrete collision detection and is also preferable for that reason.

# # Avoid complex physics Components

Certain special physics Collider Components, such as `TerrainCollider`, `Cloth`, and `WheelCollider`, are orders of magnitude more expensive than all primitive Colliders and even Mesh Colliders in some cases. We should simply not include such Components in our Scenes unless they are absolutely necessary. For instance, if we have terrain objects in the distance that the player will never approach, there’s little reason to include an attached `TerrainCollider`.