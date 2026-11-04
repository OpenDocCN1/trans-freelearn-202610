
lattice, you will also need access to the Product Design extension (https://www.autodesk.com/products/fusion-360/product-design-extension).

Autodesk offers a 14-day trial for the Product Design extension. You can activate your trial within the extensions dialog of Fusion 360.

Product Design extension access is not available for personal/hobby licenses of Fusion 360.

The lesson files for this chapter can be found here: https://github.com/PacktPublishing/3D-Printing-with-Fusion-360.

## Hollowing parts

In previous chapters, we covered how to import and edit existing designs made up of solid and mesh bodies. We also utilized shape optimization and automated modeling to have Fusion 360 generate lightweight design alternatives. Then, we simulated those geometries to see whether the generated models can withstand the operating loads. During all these activities, our main objective was to create models that can be 3D printed.

We can use 3D printing to manufacture end-use parts, as well as quickly create prototypes before mass production using other manufacturing methods. Regardless of why we choose 3D printing as our method of manufacturing, we all want to produce our physical models quickly and efficiently.

As outlined in Chapter 2, there are multiple 3D printing technologies available. Each 3D printing technology has unique design requirements to produce physical models efficiently. When you are printing parts that do not require structural strength, hollowing your model can be a great way to save time and material.

When manufacturing large-volume components using fused filament fabrication (FFF) 3D printers, it is common practice to utilize multiple contour lines to make the part strong. In FFF, it is also common to utilize an infill pattern with a density that could range from 15 to 30%, to keep the parts lightweight. This same slicing technique does not directly work for other 3D printing technologies, such as stereolithography (SLA), Multi Jet Fusion, or selective laser sintering (SLS). When creating parts to prototype using those technologies, designers must proactively think about how to lightweight parts to reduce material and energy usage.

In this section, we will focus on how to hollow parts for 3D printing technologies such as stereolithography, Multi Jet Fusion, and selective laser sintering. In most cases, hollowing out parts will save a significant amount of material instead of printing the entire part as a 100% solid object. Hollowing a part can also improve our printing success in resin-based printers (stereolithography and Digital Light Processing (DLP)) as it reduces the weight of the parts being printed and decreases the stresses on the build plate so that parts do not detach from the build plate during the printing process.

While hollowing parts, we also have to make sure our wall thickness is large enough for our chosen printing process so that our parts can be manufactured successfully and do not break during their intended use.

When working with solid models, Fusion 360 offers two methods for hollowing parts. Both methods can be accessed with the same SHELL command. The Shell command is located within the DESIGN workspace | the SOLID tab | the MODIFY panel, as can be seen in Figure 4.1.

![](img/baa027bed101c08b496f1f420cee8a5b_131_0.png)

Figure 4.1 – Accessing the shell command within the SOLID tab

Fusion 360’s SHELL functionality is very versatile. It allows you to select faces or bodies as input. If you select a face and then specify the thickness, the selected face will be removed, and you will end up with a hollow version of that body, as can be seen in Figure 4.2. The SHELL dialog also allows you to select multiple faces. By pressing down the Shift key, you can select multiple faces, and the shell command will remove those faces similarly.

![](img/baa027bed101c08b496f1f420cee8a5b_132_0.png)

Figure 4.2 – Selecting a single face to turn a solid body into a hollowed version

Another way to use the SHELL command is to select a body instead of a face(s). After accessing the SHELL dialog, you can simply select the body to shell from the browser. Alternatively, you can select the body to shell from the canvas. If you want to utilize the canvas, you have several options to select a body. One option is to change Selection Priority to Select Body Priority. After making that change, you can simply point and select the body you wish to shell, as shown in Figure 4.3. Another option is to access the Selection Filters pull-out menu and make sure Bodies is the only item that is checked, which will allow limiting your Shell selection method to a Body.

![](img/baa027bed101c08b496f1f420cee8a5b_133_0.png)

Figure 4.3 – Using the canvas to select a body to shell

Once you execute the shell command on the body, you will typically not see any visual differences in how your model looks. There are several ways you may want to inspect your model to get a better understanding of what the shelled outcome looks like. The first method is to simply change your visual style to Shaded with Hidden Edges or Wire Frame with Hidden Edges, as shown in Figure 4.4. These visual styles will both give you an indication that the model has been shelled by displaying additional edges on the inside of the model.

![](img/baa027bed101c08b496f1f420cee8a5b_133_1.png)

Figure 4.4 – Changing the visual style

If you do not want to change your visual style from the default Shaded with Visible Edges Only, you may want to utilize Section Analysis. You can access the section analysis from the SOLID tab | the INSPECT panel. After selecting a cut plane, the section analysis command allows you to translate that plane along its normal direction to show you a slice view of the component. You can also flip the cut direction so that you can look through the part as needed. Figure 4.5 shows a Section Analysis view for this part. You will also notice that the section analysis adds a line item called Section1 to the browser within the Analysis folder. We can control the visibility of all section analyses, as well as other analyses we have added to our model, by toggling the visibility icon.

![](img/baa027bed101c08b496f1f420cee8a5b_134_0.png)

Figure 4.5 – Applying a section analysis to visualize the outcome of the shell command

If we are working with a mesh object, we also can create a hollowed version. The shell command within the MESH tab | the MODIFY panel allows us to select the body, enter a thickness, and create a hollowed version of that object. Unlike the SHELL command, which is available within the SOLID tab, the SHELL command within the MESH tab does not allow the selection of faces. This means that we cannot create the same shapes using the SHELL command within the MESH tab. However, in situations where the shell command within the SOLID tab fails to create the desired outcome, you can tessellate your solid body and create a mesh version of it. After that, you can try the shell command within the MESH tab, as shown in Figure 4.6.

![](img/baa027bed101c08b496f1f420cee8a5b_135_0.png)

Figure 4.6 – Using the shell command within the MESH tab to hollow a body

Once we have the hollowed version of the mesh body, we can review the outcome by changing our visual style to shaded with hidden edges or by applying a section analysis similar to how we added a section analysis to a solid body in Figure 4.5. When we add a section analysis to a mesh body, we will not see the hatch view. Instead, we will see the triangles of the mesh body sectioned along the cut plane, as shown in Figure 4.7.

![](img/baa027bed101c08b496f1f420cee8a5b_135_1.png)

Figure 4.7 – Section analysis view of hollowed-out mesh body

In this section, we focused on how to use the shell command on both solid and mesh bodies. We covered how to shell a solid body either by removing one or more of its faces, as well as keeping the body intact and simply hollowing out the inside volume of the model. Using a mesh-specific shell command, we repeated the process on a mesh body. After each shell operation, we learned how to visualize the resulting shelled body by changing the visualization settings, as well as adding a section analysis to see the interior surfaces and hatching on the cut plane where appropriate.

In the next section, we will go over how and where to add drain holes to hollowed-out models in order to remove any raw material that would be trapped inside during and after 3D printing such models.

## Creating drainage holes

After 3D printing hollowed-out parts, we will need to drain or remove all trapped powder/resin during the postprocessing phase. That's why we need to include drainage holes in our design phase. The diameter of the drainage holes depends on the 3D printing technology we are utilizing as well as the viscosity of the material we are 3D printing with. For example, if you are using a laser-based resin printer (SLA), your resins are generally more viscous, compared to resins for DLP printer resins. Certain resins can also be used for both DLP and SLA printers. The more viscous a resin is, the larger the diameter of the drainage hole needs to be.

Table 4.1 shows two resins from different sources with varying viscosity. According to the manufacturer, the second resin is compatible with both SLA and DLP printers:

| Resin | Viscosity @ 25 deg. C |
|---|---|
| Prusa Orange Tough: https://www.prusa3d.com/product/prusament-resin-tough-prusa-orange-1kg-2/#downloads | 180-280 centipoise |
| Liqcreate Strong-X: https://www.liqcreate.com/product/strong-x/#technical | 550 centipoise |

Table 4.1 – Resins and their viscosities for SLA and DLP printing

The smallest-hole diameter the technology can create using SLA 3D printing is 0.5 millimeters, as stated in the design rules for 3D printing on Hubs.com, as shown in Figure 2.25 of Chapter 2. The same chart also shows that the minimum diameter of a hole for a SLS process is 1.5 millimeters. However, it is common practice to introduce a minimum of two 2-millimeter diameter drainage holes for DLP printers, and three or more 3.5-millimeter diameter drainage holes for SLA printers. Similarly, for SLS printers, a minimum of two 3.5-millimeter drain holes is recommended for the best results. Remember that adding more and larger drain holes always makes it easier to remove un-sintered powder as well as uncured resin from internal cavities.

The placement of drain holes also requires consideration. Another reason we need drain holes, especially for resin-based printers, is to eliminate the cupping effect. The cupping effect occurs when we 3D print a layer and the print bed moves up and down within the resin, building up pressure
