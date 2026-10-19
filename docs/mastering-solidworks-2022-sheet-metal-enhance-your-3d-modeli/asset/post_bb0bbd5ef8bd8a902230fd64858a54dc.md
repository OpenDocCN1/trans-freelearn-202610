
To make a Custom Forming Tool, we first create a shape as a normal *part* document, and then we convert it to a Forming Tool and save it as the appropriate file type, as follows:

1. Start a new *part* document and sketch a base on the **Top Plane**. This base is only used to help with tool setup and will be cut away from the model before use, so the exact size isn’t critical, but it should always be larger than the main tool shape in both directions.

In this case, sketch a **Center Rectangle** that is 50x50mm in size and fixed to the Origin. Then Extrude it 10mm, as illustrated in *Figure 10.13*.

![](img/4e9c736de6bac4449096a7b92c28af4c_210_0.png)

Figure 10.13: Extruding a 50x50mm base, fixed to the Origin

2. Next, we need to create a forming shape for the tool. That is the shape that will actually be pressed into the metal sheet when the tool is used.

3. Start a sketch on the upper face of the base extrude and use the **Polygon** tool to sketch a hexagon (six-sided polygon) that is 30mm in size across the flat edges. This should be centered on the Origin.

Extrude the hexagon 10mm upward, as illustrated in *Figure 10.14*.

![](img/4e9c736de6bac4449096a7b92c28af4c_211_0.png)

Figure 10.14: Extruding a 30mm hexagon, 10mm high

4. Generally, when making Forming Tools (and working with sheet metal), it is desirable to avoid sharp edges. With Forming Tools in particular, sharp edges can cause manufacturing issues.

Therefore, use the **Fillet** tool to first add six 5mm Fillets to the six upright edges of the hexagon.

5. Next, add a 2mm Fillet to both the top and bottom edges of the hexagon, as illustrated in Figure 10.15. Note that creating an initial base that was larger than the tool shape now allows us to easily add the lower Fillet. Without the base, it would be much more difficult to create the lower, filleted edge:

![](img/4e9c736de6bac4449096a7b92c28af4c_212_0.png)

Figure 10.15: Adding Fillets to remove sharp edges

Now that we have a filleted lower edge, we no longer need the initial base feature, and so we can cut this away.

6. Expand the first Boss-Extrude feature and select the sketch underneath. Then, use this sketch to create an Extruded Cut that is the same depth as the first Boss-Extrude feature (10mm), completely cutting away the first Boss-Extrude feature, as illustrated in Figure 10.16.

![](img/4e9c736de6bac4449096a7b92c28af4c_212_1.png)

Figure 10.16: Cutting away the initial base, using the first sketch and a Cut Extrude

The geometry of our tool is now complete, but before converting it into a Forming Tool, we can add an *Orientation Sketch*. This sketch can be used in **Flat Pattern** versions of models and can also be helpful when positioning a *part*.

7. Start a sketch on the underside face of the tool. Then, left-click to select the entire face and press the **Convert Entities** tool (on the **Sketch** tab). This will convert the outline of the face into new sketch entities, giving an outline of the tool.
8. Next, select the **Point** tool and add a *point* at the center of the tool (lined up with Origin). This can be used to help position the tool.

Then, exit the sketch.

You can see an outline of the tool in Figure 10.17):

![](img/4e9c736de6bac4449096a7b92c28af4c_213_0.png)

Figure 10.17: Adding an Orientation Sketch

9. We are now ready to convert the *part* to a Forming Tool.
Select **Insert | Sheet Metal | Forming Tool**, and the **Form Tool Property Manager** will open.

10. Select the **Stopping Face** field.

This is the face of the tool that will stop at the face of the sheet where the tool is inserted. In other words, it is the face of the tool that will be coplanar (lined up) with the metal sheet when the tool is at its deepest point. Figure 10.18 shows an example of this on a different tool:

![](img/4e9c736de6bac4449096a7b92c28af4c_214_0.png)

Figure 10.18: The Stopping Face

Select the underside face of the tool (the same face where the Orientation Sketch was drawn). Note that it turns cyan.

11. Next, the **Faces to Remove** field can be set.

This selection is optional; it's not necessary to remove any faces, but any that are selected here will turn red and will be cut out of the sheet when the Forming Tool is used.

Select the upper face of the tool, as illustrated in Figure 10.19.

![](img/4e9c736de6bac4449096a7b92c28af4c_215_0.png)

Figure 10.19: Selecting the upper face as the Face to Remove

Note that faces can also be split into smaller faces (by drawing sketch entities, such as lines and circles, on the areas to be split up and then using the Split Line tool) if you only want to cut away certain sections of a flat face, rather than the entire face.

12. Finally, we can set the Insertion Point field by selecting that tab in the Property Manager, and then selecting the point from the Orientation Sketch (or the Origin; both should be in the same place).

13. Press the green OK check mark to create a Form Tool feature (Figure 10.20). Note that the feature appears in the FeatureManager Design Tree and the tool in the Graphics Area is now color-coded. As previously mentioned, yellow indicates bend faces, red indicates faces that will be removed, and cyan (underneath) shows the Stopping Face:

![](img/4e9c736de6bac4449096a7b92c28af4c_216_0.png)

Figure 10.20: The created Form Tool

14. Before use, the Forming Tool has to be saved. There are two main ways to do this, as outlined here:

- Save as a part document: The document can be saved as a standard part file (with a *.SLDPRT file extension). If the part is saved within the forming tools folder, then SolidWorks will recognize it as a Forming Tool, even if it is saved as a standard part file. If the file is saved in a different location, then SolidWorks will not recognize the part as a Forming Tool.
- Save as a Forming Tool document: Alternatively, the model can be saved as a specific Forming Tool file (with a *.SLDFTP file extension). In this case, the file can be saved in any location and SolidWorks will still recognize it as a Forming Tool.

## Forming Tool or Part Document?

Forming Tools can be saved as either file type, as detailed previously. The difference between the two is mostly just a matter of file organization. If you prefer to keep all files from a specific project in the same location, then it may be best to save the tool as a Forming Tool document (*.SLDFTP), but if the Forming Tool is a generic tool that will be used on multiple projects, then you may prefer to save it directly into the Design Library as a normal part file.

Save the tool file by going to File | Save As....

The type of document can then be set by selecting from the Save as type drop-down menu, directly underneath the filename.

In this case, save the tool as a standard SolidWorks part, within the forming tools folder. (The default location is C:\ProgramData\SolidWorks\SolidWorks 20xx\Design Library\Forming Tools.)

15. After saving, open the Design Library within the Task Pane on the right-hand side of the screen and check that your new tool appears with the other Forming Tools. It can now be dragged into models and used in the same way as the standard Forming Tools.

If the tool does not appear, you may need to add it to the Design Library using the Add to library button, as described in the information box at the end of the Modifying existing forming tools section.

## Forming Tool Not Working Correctly?

If you find that your new Forming Tool does not work correctly, then try checking that the corners are rounded. Sharp corners can sometimes cause errors with Forming Tools.

This section showed how to create entirely Custom Forming Tools instead of just modifying existing models. This greatly expands the range of indentations and cuts that you can use.

## Summary

In this chapter, we looked at how to use the Forming Tools from the **Design Library** to add 3D detail to your Sheet Metal models. We also explored modifying existing tools to suit your own needs, as well as creating completely Custom Forming Tools. This allows a virtually unlimited variety of Forming Tool shapes and sizes to be used.

In the next chapter, we will learn how existing 3D models can be converted into Sheet Metal models using the **Insert Bends** tool.
