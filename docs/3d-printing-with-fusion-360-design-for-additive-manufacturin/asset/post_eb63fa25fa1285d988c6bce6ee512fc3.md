
By the end of this chapter, you will have a better understanding of how to generate support structures for both solid and mesh models using Fusion 360. You'll learn how to create bar-shaped supports including lattices, volume supports, and base plate supports depending on the 3D printing technology. You will learn the differences between top-down and bottom-up resin printing technologies and their unique support requirements. You will also gain a solid understanding of how to create bar supports over an area, an edge, or a local minima point in order to successfully 3D print parts using SLA/DLP printers.

## Technical requirements

Everything covered in this chapter is accessible to users with the personal, trial, commercial, startup, or educational Fusion 360 license types.

There are certain Fusion 360 additive machines, such as **metal powder bed fusion (MPBF)** 3D printers and their respective print settings and support structure generation capabilities, which are not available until gaining access to the Additive Build extension. As with all other Fusion 360 extensions, the Additive Build extension is not available for personal use. As we will not cover topics specific to metal 3D printing in this chapter, you should be able to follow along with all the examples in this chapter with no issues.

If you have a commercial license of Fusion 360, to create an additive setup with a metal powder bed fusion machine, you will also need access to the Additive Build extension (https://www.autodesk.com/products/Fusion-360/additive-build-extension).

Autodesk offers a 14-day trial for the Additive Build extension for all commercial users of Fusion 360. You can activate your trial within the extensions dialog of Fusion 360.

The lesson files for this chapter can be found here: https://github.com/PacktPublishing/3D-Printing-with-Fusion-360.

## Volume and bar supports for FFF printing

**Fused filament fabrication (FFF)** 3D printers can generally print parts with overhang angles of less than 45 degrees without needing support structures. If you want to print a part using an FFF printer, one of the first things to do is to try and find an orientation that will allow you to print that part with no support structures.

However, certain models cannot be printed without supports. This is because no matter which orientation you choose, certain parts will end up with down-skin surfaces with overhangs larger than 45 degrees that span distances larger than you can bridge with your 3D printer. In such cases, you will need to generate support structures using your CAD tool or your slicer. As Fusion 360 is both a CAD software and a slicer, you can generate support structures parametrically within the MANUFACTURE workspace for any additive setup.

In this section, we will talk about how to generate volume and bar supports for an additive setup with an FFF 3D printer. To demonstrate how to generate volume supports, we will use a Fusion 360 document named Bracket.f3d. This document already contains an additive setup within the **MANUFACTURE** workspace. As shown in *Figure 10.1*, **Setup 1** is an additive setup with a **Prusa i3MK3S +** printer and a **PLA (Direct drive)** print setting. The component has been arranged within the 3D printer such that it is in the middle of the build volume. A suitable orientation has also been selected for this part. Based on this orientation, we will need to generate support structures in order to print this part successfully. *Figure 10.1* shows the two options we have to generate support structures located within the **SUPPORTS** panel of the **ADDITIVE** tab for this setup.

![](img/baa027bed101c08b496f1f420cee8a5b_301_0.png)

Figure 10.1 – Support structure types for an additive setup with an FFF 3D printer

If you select the **Solid Volume Support** command located within the **SUPPORTS** panel, a new dialog called **SOLID VOLUME SUPPORT** will become visible. In this dialog, there are two tabs. The **Geometry** tab allows you to select which faces or bodies should be supported with this operation. *Figure 10.2* shows how the selected faces are highlighted in blue while the **SOLID VOLUME SUPPORT** dialog is active.

Within this dialog, you also have the ability to control the Support Overhang Angle. Fusion 360 will automatically generate a support structure for sections of downward-facing surfaces that have an overhang angle smaller than the entry in this input field. In other words, if we increase the Support Overhang Angle input, Fusion 360 will generate supports over a larger area.

![](img/baa027bed101c08b496f1f420cee8a5b_302_0.png)

Figure 10.2 – Selecting faces of a solid model for a volume support

In the GitHub folder for Chapter 10, you will find another file called Bracket-Mesh.f3d. This Fusion 360 document is a duplicate of the previous one with one small difference. As you can see in Figure 10.3, in this second document, the component named Bracket is not a solid body, but a mesh body. The solid volume support dialog allows you to select not only entire bodies but also individual face groups when the component being supported is a mesh body. In order to select individual face groups, you have to make sure that the mesh face groups are displayed. The easiest way to display mesh face groups while you are within the MANUFACTURE workspace of Fusion 360 is to use the keyboard shortcut Shift + F. If the mesh body in the active setup has more than one face group, each mesh face group will be displayed with unique colors, as you can see in Figure 10.3. After displaying the mesh face groups within the MANUFACTURE workspace for a given additive setup, you can select face groups as input objects to be supported within the SOLID VOLUME SUPPORT dialog.

![](img/baa027bed101c08b496f1f420cee8a5b_303_0.png)

Figure 10.3 – Selecting three face groups of a mesh body

Now that we have highlighted how to select face groups for a mesh body when generating volume supports, we can switch back to the original model (Bracket.f3d) and create our volume supports on the previously selected two faces of the solid body. Once the support structure is generated by clicking the OK button on the SOLID VOLUME SUPPORT dialog, Fusion 360 will create a support structure as shown in Figure 10.4, and you will be able to visualize it as a blue volume under the overhanging surfaces. Fusion 360 also adds a new line item called Solid Volume Support1 within the Supports folder of the active setup in the browser.

![](img/baa027bed101c08b496f1f420cee8a5b_304_0.png)

Figure 10.4 – Visualizing a solid volume support

In Figure 10.4, you may have noticed that the blue-colored support structure does not enclose the entire opening, but rather supports roughly half the arc face. This is because we used a support overhang angle of 40 degrees and Fusion 360 did not support any point on that arc with an overhang angle greater than 40 degrees. It is important to remember that overhang angles are measured from the horizontal plane as demonstrated by the tooltip image in the preceding figure.

Another option we have within the Geometry tab of the SOLID VOLUME SUPPORT dialog is the Avoid checkbox. By activating this checkbox, we can select one or more faces as input and prevent Fusion 360 from generating support structures on those surfaces. Figure 10.5 shows the creation of a second Solid Volume Support to support the entire body, while avoiding the generation of supports on the two arc faces highlighted in blue, which were previously used as inputs to create Solid Volume Support1.

![](img/baa027bed101c08b496f1f420cee8a5b_305_0.png)

Figure 10.5 – Selecting faces to avoid while generating a solid volume support

The outcome of the generation of this second solid volume support is the creation of a volume support for the entire body, while excluding the two faces we specified in the dialog to avoid. Using this technique, we can add unique support structures to certain faces while creating a different set of supports for the rest of the body.

The second tab within the SOLID VOLUME SUPPORT dialog is named General. Figure 10.6 shows the two input fields in the General tab. The first input field, named Top Distance to Part, controls the distance between the support structure we are generating and the down-skin of the part. The second input field, named Bottom Distance to Part, controls the distance between the support structure and the up-skin of the part if a given support structure were to be touching the up-skin of our part instead of the build plate.

The inputs for both of these fields are in units of length, and by default, the input is 0.3 millimeters. As shown in Figure 10.6, the layer height for the PLA (Direct Drive) print setting is 0.15 millimeters. Therefore, a support structure we generate using these settings will have a two-layer gap above the up-skin and below the down-skin of our part.

Based on my experience, when 3D printing with PLA, whenever we leave a two-layer gap between the part and the supports, the post-processing becomes easier, and we can remove the support structure from the part after printing without a lot of effort required.
