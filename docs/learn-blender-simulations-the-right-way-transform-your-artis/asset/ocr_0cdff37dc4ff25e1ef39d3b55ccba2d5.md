It’s also important to note that the normals on the Cylinder 1 object are pointing toward the inside of the mesh rather than the outside.

![](bbox=[0.22, 0.13, 0.78, 0.37])

Figure 7.4 – Direction of the normals

This is because the collision is based on the direction of the normals. If they were pointing to the outside of the mesh, the soft body would not collide but simply fall through. View the previous chapter on collision objects for more of an explanation.

If you want to see the direction of the normals of your object, you can go into edit mode and, in the Mesh Edit Mode menu, turn on the Vertex, Edge, or Face normals and change the size. This will bring out the blue lines in your mesh (Figure 7.4) and show which direction the normals are facing.

![](bbox=[0.38, 0.55, 0.62, 0.79])

Figure 7.5 – Mesh Edit Mode menu