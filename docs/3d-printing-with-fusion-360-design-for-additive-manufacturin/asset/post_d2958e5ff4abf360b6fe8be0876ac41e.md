
within the enclosed volumes. Adding a drain hole to relieve pressure where cupping may occur will help our prints be successful, eliminate support structure failures, and remove layer-shift problems.

Now that we have covered why we need drainage holes and how big they should be, let’s discuss where we generally should place them. In resin printing, it is best to place drainage holes as close to the build plate of the printer as possible in the orientation chosen to be for the part to be 3D printed. This means we have to orient our model for 3D printing before designing drain holes. We will cover orienting a component in detail in Chapter 8. Once we have our orientation selected, we can place our drain holes on the bottom surface of our model close to the build plate. This will ensure that we don’t have pressure building up in our hollow model during the printing process for the resin printers. We don’t have the same issues about drain hole placement for SLS printers, as cupping and pressure buildup during printing is generally not an issue. This means we can place them anywhere on our model as long as they are away from critical surfaces.

Now let’s demonstrate how we can create drain holes using Fusion 360. Figure 4.8 shows the same hook model we worked on in the previous section already oriented to be printed using an SLA/DLP printer. To better visualize the orientation of the part with respect to the build plate, I have also included a construction plane along the XY plane. In addition, the part opacity is down to 50% to show that the part has been hollowed out.

![](img/baa027bed101c08b496f1f420cee8a5b_137_0.png)

Figure 4.8 – Hollowed-out version of the hook model oriented for resin 3D printing

For this part, we will need a minimum of two 2-millimeter diameter holes. The best placement for these holes is at the base of the hook on the back face of the component, as shown in Figure 4.9. As this model allows for the creation of bigger holes, I have chosen the hole diameter to be 3 mm for both holes.

![](img/baa027bed101c08b496f1f420cee8a5b_138_0.png)

Figure 4.9 – Creating a sketch and drawing two circles

Once our sketch is done, we can create the drain holes using the **EXTRUDE** command or the **HOLE** command. In Figure 4.10, you can see how we can create the drain holes using the **HOLE** command and have detailed control over the size, depth, and shape of the hole.

![](img/baa027bed101c08b496f1f420cee8a5b_138_1.png)

Figure 4.10 – Creating two drain holes using the HOLE command

After creating our holes, we can add a **SECTION ANALYSIS** as shown in *Figure 4.11*. During the creation of the section analysis, if we choose the XY plane from the Origin node within the **BROWSER** as the cut plane, offset it in the positive Z direction by approximately 22 millimeters, and flip the section view, we can see the bottom of the part and observe that we have created two small holes where the resin can drain during 3D printing:

![](img/baa027bed101c08b496f1f420cee8a5b_139_0.png)

Figure 4.11 – Section analysis shows two drain holes

Creating drain holes for a mesh-based model is similar to creating them for a solid model. After opening our mesh body, hollowing it out, and rotating it to the orientation we wish to 3D print it in, we will have to access the **DIRECT EDIT** command located within the **MESH** tab | the **MODIFY** panel, as shown in *Figure 4.12*:

![](img/baa027bed101c08b496f1f420cee8a5b_140_0.png)

Figure 4.12 – Utilizing the Direct Edit mode for a mesh body

Once we are in the DIRECT MESH EDITING tab, we can create a plane through three points using the functionality located within the CONSTRUCT panel. We will later utilize this construction plane to create a sketch so we know the location of our holes.

![](img/baa027bed101c08b496f1f420cee8a5b_140_1.png)

Figure 4.13 – Creating a construction plane on a mesh body

Once the construction plane has been created, we can select the **FINISH DIRECT EDITING** command in the ribbon, which will get us back to the parametric design mode of Fusion 360. Now we are ready to create a sketch. We can access the **SKETCH** command within the **SOLID** tab | the **CREATE** panel. Once we are in the **SKETCH** tab, we will need to create multiple circles to represent the drain holes. Figure 4.14 demonstrates two circles 20 millimeters apart from each other that have 3-millimeter diameters. Once our sketch is done, we can select the **FINISH SKETCH** command in the ribbon:

![](img/baa027bed101c08b496f1f420cee8a5b_141_0.png)

Figure 4.14 – Creating two circles within the sketch environment

Now, we are ready to extrude those two profiles and create two cylinders. Using the **EXTRUDE** command, located within the **SOLID** tab | the **CREATE** panel, we can select our newly created circular profiles and extrude them so that they are slightly taller than the thickness of the part. In the previous section, when we were hollowing out this part, we designed the wall thickness to be 5 millimeters. Therefore, creating a 10-millimeter extrude should be sufficient for our holes.

![](img/baa027bed101c08b496f1f420cee8a5b_142_0.png)

Figure 4.15 – Extruding circular profiles to make cylinders to represent drain holes

In *Chapter 2*, we talked extensively about how to work with CAD and mesh files using Fusion 360. One of the workflows we covered in that chapter was tessellating solid models to convert the solid bodies into mesh bodies in order to combine them with other mesh bodies. That is exactly what we will do next. Let's start by using the **TESSELLATE** command located within the **MESH** tab | the **CREATE** panel, on the two new cylindrical solid bodies.

![](img/baa027bed101c08b496f1f420cee8a5b_142_1.png)

Figure 4.16 – Tessellating the two cylindrical solid bodies

Once we have converted the solid bodies into mesh bodies, using the COMBINE feature located in the MESH tab | the MODIFY panel, we can select the REMOVE operation and remove them from the direct edited version of the tessellated hook body, as shown in Figure 4.17:

![](img/baa027bed101c08b496f1f420cee8a5b_143_0.png)

Figure 4.17 – Using the Combine command to remove the two cylinders from the hook

Just as we added a section analysis to the solid body to display the drain holes, we can apply a section analysis along the XY plane to the meshed body and move it up in positive Z by 22 millimeters to show the drain holes:

![](img/baa027bed101c08b496f1f420cee8a5b_144_0.png)

Figure 4.18 – Section Analysis to show drain holes within the mesh body

In this section, we covered how to work with solid bodies and mesh bodies to add drain holes once they are properly oriented for resin printing. The same principles also apply to adding drain holes to geometries that utilize other 3D printing technologies, such as SLS or Multi Jet Fusion.

## Creating internal lattice structures

In previous sections, we covered various ways we can hollow solid or mesh bodies. We demonstrated how to add drain holes so that any trapped powder or resin can be removed after the part has been 3D printed. Our main goal in hollowing parts was to lightweight them so that we don't waste material and energy while printing our parts.

Whenever we create hollow objects, we should also pay attention to whether those parts are self-supporting to be 3D printed, based on the 3D printing technology we have chosen. You will remember from previous chapters that if we are 3D printing using SLS, we don't need support structures. However, if you are 3D printing using a resin-based printer, such as SLA or DLP, portions of your model with overhang angles above a critical angle will require support structures. We often think about support structure as material printed on the outside of a part so that it will support our part during printing. A similar concept also applies to parts that we hollow. If we hollow a part, we are essentially creating internal surfaces, which may also require support structures, so that our parts can be printed successfully with a resin printer. In this section, we will talk about how to create those internal support structures.
