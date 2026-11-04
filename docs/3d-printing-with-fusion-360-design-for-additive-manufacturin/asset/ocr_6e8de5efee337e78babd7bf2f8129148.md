![](bbox=[0.115, 0.095, 0.852, 0.425])

Figure 1.18 – The Close Holes repair type and its impact on this part

Even though the outcome of this automatic repair is reported as a success, it is always a good idea to perform a visual check and review the details of the mesh body properties. Figure 1.19 shows the PROPERTIES dialog for the Clip_Broken mesh body after we performed the Close Holes repair type. Here, we can see that this mesh is made up of 66 shells. This is an unexpected outcome, and a visual inspection of the part shows problematic zones, as highlighted in red ovals. This part is a symmetrical part along the XZ plane, yet the automatic repair closed one of the holes instead of creating the desired countersink hole type outcome: