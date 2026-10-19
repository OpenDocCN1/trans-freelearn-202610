8. The exact position of the flange can be specified using the three **Flange Position** buttons. The **Trim Side Bends** and **Custom Bend Allowance** options can also be used, and the start and end of the **Miter Flange** can be offset.

9. Press **OK** to create a **Miter Flange**.

# Adding multiple edges

One of the great benefits of using **Miter Flanges** instead of **Edge Flanges** is that corners between multiple edges can be mitered. In this context, this means that they join correctly at the corners with a specific gap.

To add multiple edges and demonstrate the mitered corners, proceed as follows:

+   1. Edit the **Miter Flange** feature in the FeatureManager Design Tree.
+   2. Ensure you are in the **Along Edges** selection box (this should be blue). If not, then left-click in the box to select it.
+   3. Left-click on more edges of the **Base Flange** to select them, and you should see the yellow preview now adds new flanges to the edges selected.
+   4. The **Gap distance** option in the **Property Manager** allows the gap between the adjacent flanges to be adjusted.
+   5. Select all four of the **Base Flange** edges and press **OK** to create a **Miter Flange** feature.

Note that the *edges selected have to be adjacent to each other* and have to connect back to the original edge/face where the initial sketch was drawn.

As mentioned, one of the key benefits of using the **Miter Flange** instead of the **Edge Flange** is that it allows corners to be properly mitered:

+   1. Expand the **Miter Flange** feature in the FeatureManager Design Tree and edit the sketch. We can now change the profile of the flange.
+   2. Select the vertical line by left-clicking on it, and delete the Vertical Relation. This can be done either by selecting and deleting the small **Vertical Relation** symbol next to the line in the **Graphics Area** or by deleting the Vertical Relation in the **Existing Relations** box at the top of the **Property Manager** on the left.

When the Vertical Relation is deleted, the line should turn blue, which indicates that it is now Under Defined.