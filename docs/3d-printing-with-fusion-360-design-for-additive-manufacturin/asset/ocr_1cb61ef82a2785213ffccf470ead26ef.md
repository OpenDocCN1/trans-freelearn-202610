As the main objective of this subsection is applying lattices, let's go back to the **Volumetric Lattice** within the browser, right-click, and edit it, as shown in Figure 4.30. When we are editing our volumetric lattice, we can access the **Solidity** tab and increase the solidity from 0 to 0.2, thereby applying both a skin and a lattice to this part, which already has a drain hole:

![](bbox=[0.11, 0.18, 0.87, 0.48])

Figure 4.30 – Volumetric model shelled and latticed

Once we have our design finalized, Fusion 360 offers two methods for 3D printing a volumetric model. The first method is to switch to the manufacturing workspace, creating an additive setup with the appropriate 3D printer, and slice the volumetric model natively in Fusion 360 to be 3D printed. The second method is to create a mesh body so that we can export it to third-party slicers. To convert a volumetric body into a mesh body, we have to select it from the browser and right-click. As shown in Figure 4.31, after choosing the **Create Mesh** option, we will be able to create a mesh representation of the volumetric body: