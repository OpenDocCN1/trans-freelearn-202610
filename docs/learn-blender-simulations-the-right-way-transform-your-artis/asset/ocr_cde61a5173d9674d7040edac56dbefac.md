3. We don’t need collision on **Cylinder 2** as the sphere will not interact with it, so let’s skip to **Cylinder 3**. Select it and add collision. If we play the simulation, you will notice that this happens:

![](bbox=[0.22, 0.13, 0.73, 0.44])

Figure 7.6 – Collision example

The reason this is happening is that the **Inner** and **Outer** thickness zones are overlapping. Remember in the last chapter when we went over the **Outer** and **Inner** zones? In this case, the **Cylinder 3** object is thin enough that the collision boundaries on the outside and inside of the mesh are overlapping. This causes the soft-body object to get stuck.

4. To fix this, set the **Inner** thickness of **Cylinder 3** to the lowest value, 0 . 001. Now it should collide properly without getting stuck.

To save time in this chapter, there is already some basic animation of **Cylinder 3** opening and closing. This will cause the sphere to fall out and go down the obstacle course.

![](bbox=[0.29, 0.68, 0.66, 0.85])

Figure 7.7 – Cylinder opening