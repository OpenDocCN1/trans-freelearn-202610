
5. The width of the **Base Flange** can be set using the **Direction 1** (and **Direction 2**, if required) options in the **Property Manager** on the left of the screen. The **Sheet Metal** properties can also be set at this stage (see Figure 1.20).

![](img/4e9c736de6bac4449096a7b92c28af4c_44_0.png)

Figure 1.20: Creating a Base Flange

6. Press **OK** to create a **Base Flange**. It can be seen that this creates a simple, rectangular **Base Flange** similar to the **Single Closed Profile** example from the first section.

In this way, the **Single Open Contour Base Flange** can be thought of as similar to the **Thin Feature Extrude** option from normal solid modeling.

## Creating more Base Flange complex shapes

The real beauty of using the Single Open Contour option is that it allows you to easily create more complex, bent shapes in one feature.

- 1. Start a new part document and start a sketch on the appropriate Plane, such as the Front Plane.
- 2. Sketch a more complex shape using the Line tool. An example Z or jog shape can be seen in Figure 1.21.

![](img/4e9c736de6bac4449096a7b92c28af4c_45_0.png)

Figure 1.21: A more complex Single Open Contour sketch

- 3. Select Base Flange/Tab from the Sheet Metal tools and set your desired options. Example settings could be:
  - Width: 10mm, Direction 1
  - Thickness: 2mm
  - Bend Radius: 1mm

4. Press **OK** to create a **Base Flange** (*Figure 1.22*). It can be seen that although the underlying sketch had sharp corners, the actual **Base Flange** feature has automatically been created with the bent corners using the **Bend Radius** that was specified in the **Sheet Metal** properties:

![](img/4e9c736de6bac4449096a7b92c28af4c_46_0.png)

Figure 1.22: Single Open Contour Base Flange

**Using Single Open Contours** to create **Base Flanges** can be a good way to simplify the production of parts such as simple brackets.

Note that these types of sketches don’t just need to use straight lines—they can also use arcs and splines if required.

## Flattening parts

Now that we have created a *part* with bends in it, we can explore how 3D Sheet Metal *parts* can be flattened. Flattening is one of the features of the SolidWorks Sheet Metal module that really does add huge value to the design process because it allows *parts* to be accurately converted from finished *parts* into a state that can be easily created from sheets of flat stock material.

To flatten Sheet Metal parts:

- 1. From within a Sheet Metal part, select the Flatten option from the Sheet Metal tab or toolbar (labeled i in Figure 1.23).
- 2. The part will be unfolded into a flat sheet. Note the dotted outline, which indicates the bounding box of the flattened part, and the sketch lines, which show the location of Bend Lines (labeled ii in Figure 1.23).
- 3. If your Bend Lines aren't visible, try going to View | Hide/Show | Sketches.
- 4. Note also that the Flat-Pattern folder at the end of the FeatureManager Design Tree is now unsuppressed and so is no longer grayed out (labeled iii in Figure 1.23).

Using the Flatten option essentially unsuppresses (or switches on) the Flat-Pattern folder, which flattens the part. If you expand the Flat-Pattern folder, then the Flat Pattern feature will be seen, and this contains the bend details of the part.

- 5. To unflatten—or fold up—the part, simply click the Flatten button on the Sheet Metal tab or toolbar again or click the Flatten button that can be seen in the top right of the graphics area (labeled iv in Figure 1.23). Note that the part folds up and the Flat-Pattern folder is now suppressed (or switched off) again.

![](img/4e9c736de6bac4449096a7b92c28af4c_47_0.png)

Figure 1.23: Flatten option

The Flatten option is a vital aspect of SolidWorks Sheet Metal that allows parts to be unfolded. As we progress through the book and learn about the Sheet Metal tools, we will see this option in use in a more practical setting.

## A recap of Base Flange options and flattening parts

There are three types of Base Flange (See Fig. 1.24):

- 1. **Single Closed Profile**: This is one of the most basic types of Base Flange and uses one single profile that is fully closed, such as a simple rectangle or square. Although it is simple, this type of Base Flange is extremely useful and is likely to make up the majority of Base Flanges for Sheet Metal parts.
- 2. **Multiple Contained Closed**: Closed profiles can also contain smaller, closed profiles such as cutouts and holes. While these types of Base Flanges can be useful in certain situations, it is generally better practice to create simple Base Flanges using a Single Closed Profile and then add any extra holes, cuts, or details as separate features. This makes modification of these features at a later stage much easier.
- 3. **Single Open Contour**: Base Flanges can also be created using a collection of single Sketch Entities such as lines, arcs, and splines. This type of Base Flange can be useful when creating items such as brackets.

![](img/4e9c736de6bac4449096a7b92c28af4c_48_0.png)

Figure 1.24: Types of Base Flange; left to right: Single Closed Profile, Multiple Contained Closed, Single Open Contour

Flattening is a vital aspect of SolidWorks Sheet Metal and parts with bends can be flattened by simply selecting the Flatten option from the Sheet Metal tools. This will unsuppress the Flat-Pattern folder and unfold any bends in the part.

So far in this chapter, we have learned how to start Sheet Metal parts and how to create the first feature of most of these parts—the Base Flange. We have also looked at how to edit the various Sheet Metal properties that are important to those parts. So, now that we have a reasonable idea of how to start virtual Sheet Metal parts, we will jump briefly into a real-world aspect of sheet metal and take a look at the various sheet metal materials available.

## Considerations when selecting sheet metal materials

There are dozens of types of metal that could be used to create sheet metal parts and these all vary in use, physical properties, cost, and availability. The following pages will give you an idea of which might be best for your project.

This section is not an exhaustive list of materials; rather, it is just an overview of some of the most common ones that you are likely to encounter. Within these materials, there are also many specific grades (sub-types) of metal, each with slightly different properties. If in doubt, discuss your needs and budget with your sheet metal manufacturer and your material supplier to find the most suitable choice for your needs.

## Steel and aluminum

The vast majority of sheet metal parts around us are made from either steel or aluminum, and of those, steel is probably the most common material.

## Advantages and disadvantages of steel

Steel is an excellent all-round material, and if you're unsure which material to use, then steel is usually a solid choice. Steel is so popular and widely used for a number of reasons. It is very strong, easy to weld, and ductile, which means it is easy to bend and form compared to more exotic metals. Steel is also fairly inexpensive—it can be up to three times cheaper than aluminum for the same weight—and is widely available.

Material choice is always about compromise, so every material will have disadvantages. One of the main downsides of steel is that it is quite heavy—around two and a half times denser than aluminum, depending on the exact grades used. This means that if you have two pieces that are the exact same size and shape, then the steel part will be significantly heavier than the aluminum part.

Steel also has poor corrosion protection, meaning that it often goes rusty. You will have seen rusty steel items such as car bodywork. This rust is corrosion that is caused by a reaction between the metal and elements in the environment such as water and oxygen. The rust weakens and eventually destroys the metal. Steel can be treated to prevent or slow corrosion, but this will add to the material cost, and some other metals have much more natural resistance to corrosion.
