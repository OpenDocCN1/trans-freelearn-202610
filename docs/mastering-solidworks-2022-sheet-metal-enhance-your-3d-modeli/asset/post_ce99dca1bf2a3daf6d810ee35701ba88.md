
4. We can now select the corners that we wish to break. The corners can be left-clicked directly, or the entire face can be selected and all relevant corners will be broken. In this way, the Break-Corner/Corner-Trim tool is much more useful than the Fillet or Chamfer tools, because we don’t have to zoom in a lot to select the very short edges of the flanges; we can simply select the entire face.

Select both of the Edge Flanges, and the Base Flange, as shown in Figure 9.9:

![](img/4e9c736de6bac4449096a7b92c28af4c_186_0.png)

Figure 9.9: Adding the Break Corner feature

5. Press the green OK check mark to add the Break-Corner/Corner-Trim feature.

So far, we have just created external broken corners, but the tool can also be used with internal corners, as shown in the next section.

## Using the Break-Corner/Corner-Trim tool with internal corners

Follow these steps to use this tool with internal corners:

- 1. Add a Tab to the Base Flange, by sketching a rectangle on the upper face, then selecting the Base Flange/Tab tool (Figure 9.10):

![](img/4e9c736de6bac4449096a7b92c28af4c_187_0.png)

Figure 9.10: Adding a Tab to the Base Flange

- 2. Select the Break-Corner/Corner-Trim tool again.

3. Using the same settings as before (**Fillet**, 5mm size), select the **Base Flange** face again. Note how just the *external* corners are selected (Figure 9.11):

![](img/4e9c736de6bac4449096a7b92c28af4c_188_0.png)

Figure 9.11: Selecting the Base Flange face

4. Zoom in and manually select the two *internal* corners (Figure 9.12). Creating internal broken corners adds material, whereas creating external broken corners removes material.

![](img/4e9c736de6bac4449096a7b92c28af4c_188_1.png)

Figure 9.12: Manually selecting internal corners

5. Press the green **OK** check mark to add the new broken corners.

As well as using the **Break-Corner/Corner-Trim** tool on folded parts, it can also be used on flattened models to quickly select all corners together.

## Using the Break-Corner/Corner-Trim tool on flattened models

There may be many cases where you create an entire Sheet Metal model without adding any trimmed corners but wish to add them all at the end of your modeling, perhaps to aid with manufacturing.

In these cases, the **Break-Corner/Corner-Trim** tool can be used after flattening the part:

- 1. Continuing from the previous example, delete any current **Break-Corner/Corner-Trim** features so that you just have the **Base Flange**, **Tab**, and two **Edge Flange** features.
- 2. Flatten the model by clicking the **Flatten** button.
- 3. Now expand the drop-down menu for the **Corner** tools and you'll see that only one option is available, **Corner-Trim**. This is very similar to the previous tool that we used in the last section but specialized for flattened models. Select the **Corner-Trim** tool (Figure 9.13):

![](img/4e9c736de6bac4449096a7b92c28af4c_189_0.png)

Figure 9.13: Using the Corner-Trim tool with a flattened part

- 4. We now have the ability to add **Relief Options** and to add **Break Corner Options**. We will cover the **Relief Options** in the next section and just look at the **Break Corner Options** for now.

As before, we can set the **Break type** and **Size**.

Set **Filletted**, 5mm size.

- 5. Next, select the corners to be broken. When using this tool in the flattened state, we have two new options. Clicking the **Collect all corners** button will automatically select all relevant corners in the model. We can also check the **Internal corners only** box to just select internal corners (external corners have to be selected manually).

Click **Collect all corners** to select the bends in the model.

> **Using the Collect all corners Button versus Selecting the Face**
> When selecting the corners to break, I recommend using the **Collect all corners** button, instead of just selecting the face. This is because sometimes selecting the face will also mistakenly select corners within bends, which shouldn't be broken.

- 6. Press the green **OK** check mark to break the selected corners. Note that the **Corner-Trim** feature now appears after the **Flat-Pattern** folder in the FeatureManager Design Tree.

- 7. Unflatten the part by selecting the **Flatten** button again. Note that the trimmed corners no longer appear in the model and the **Corner-Trim** feature is suppressed (turned off). This is because the **Corner-Trim** feature is a *child feature* of the **Flat-Pattern** feature. Therefore, if the **Flat-Pattern** feature is suppressed (such as when the model is unflattened) then the **Corner-Trim** feature is also suppressed.

Using the **Corner-Trim** tool after flattening in this way is an example of one of the few times where you might add features to the model after the **Flat-Pattern** folder at the end of your modeling.

If you require the corners to be trimmed in the unflattened state, then you can use the **Corner/Corner-Trim** tool as described earlier in this chapter, or you can use the **Unfold** feature to flatten your part, then manually trim all of the corners, then use the **Fold** feature to fold the part back up.

In the next section, we will take a look at the final corner tool, **Corner Relief**.

## Preparing your model for manufacturing using Corner Relief

The **Corner Relief** tool allows us to add relief cuts to the corners in our model. This can be useful for avoiding manufacturing issues in tight corners where two or more bends meet.

The tool can be used in the unflattened or flattened state and we will look at the unflattened version first:

- 1. Continuing from the previous example, delete any current **Break-Corner/Corner-Trim** features so that you just have the **Base Flange**, **Tab**, and two **Edge Flange** features. Ensure that the model is in the Unflattened (that is, folded-up) state.
- 2. Select the **Corner Relief** tool from the drop-down menu underneath the **Corner** tools. We can now set some options in the Property Manager (*Figure 9.14*):

![](img/4e9c736de6bac4449096a7b92c28af4c_191_0.png)

- 3. The **Scope** section simply indicates which sheet we are working with. This is usually automatically prefilled but if working in a multi-sheet part, then you can select the appropriate sheet.

- 4. Next, we need to select either a **2 Bend Corner** or a **3 Bend Corner**. In our current model, we only have a 2 Bend Corner. This means that the corner has two bends that are next to each other (*Figure 9.15*). In the next section, we will look at 3 Bend Corners.

![](img/4e9c736de6bac4449096a7b92c28af4c_192_0.png)

Figure 9.15: A 2 Bend Corner

Select **2 Bend Corner** then click the **Collect all corners** button in the **Corners** section.

Notice how the **Define Corner** box has now been filled with the two faces from the bend.

5. We can now set the **Relief Options**. There are five main options, each of which produces a slightly different style of cut. Each version also has a number of sub-options:

- **Rectangular**: This will cut out a rectangular area. The cut size can either be determined by a numerical value in the **Slot length** box or by checking the **Ratio to thickness** box, which calculates the size according to the sheet thickness.

The **Centered on bend lines** option centers the cut relative to the bend lines, and the **Tangent to bend** option makes the cut tangent to the bend lines (Figure 9.16).

The **Add filleted corners** button simply rounds off the sharp corners of the relief cut.

![](img/4e9c736de6bac4449096a7b92c28af4c_193_0.png)

Figure 9.16: The Centered on bend and Tangent to bend options

- **Circular**: Produces a circular cut out.

- **Tear**: Creates a torn relief; there are no sub-options available.

- **Obround**: This creates an obround (slot-shaped) cut.

- **Constant Width**: This makes a linear and continuous cut with a constant width and it has no sub-options.

Experiment with the different types of **Corner Relief** cut to see the differences between them on the preview in the Graphics Area.

6. Press the green **OK** check mark to create the **Corner Relief** cuts.

As well as 2 Bend Corners, the **Corner Relief** tool can also be used with more unusual 3 Bend Corners.
