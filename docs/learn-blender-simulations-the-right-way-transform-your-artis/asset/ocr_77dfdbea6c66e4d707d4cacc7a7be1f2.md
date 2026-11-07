Smoke and fire simulations are made up of what we call **voxels**. You can think of a voxel as a 3D pixel of the simulation. The smaller the pixel, the better the simulation will look. You can change the size of these voxels by increasing the **resolution** of a simulation. Higher values will make the simulation more detailed but will cause the simulation to run slower and take longer to bake:

![](bbox=[0.18, 0.2, 0.82, 0.48])

Figure 1.4 – Example of a voxel

In the preceding screenshot, the left simulation has a resolution of 8. This is quite small, so that is why the voxel is very large. The simulation on the right has a resolution of 64 and, as you can see, the voxel size is much smaller. You can also tell the size of the voxel by looking at the bottom-left corner of the **domain** object:

![](bbox=[0.18, 0.6, 0.82, 0.88])

Figure 1.5 – Domain voxel size