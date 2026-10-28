
## Exercise file

For this chapter, you will need to use the exercise file titled *Washbasin-DYNAMIC.dwg*.

## Creating the washbasin block drawing

In this section, we will be using AutoCAD objects to create the various washbasin blocks for use in the dynamic block. To begin, open the file in the **Model** tab and zoom in to see the three views of the washbasin. You will see the objects that will be used to make the regular washbasin blocks. From left to right, they are front, top, and side. Each view comprises AutoCAD objects on layer **0** (zero), as displayed in *Figure 12.1*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_314_0.png)

Figure 12.1: The objects to be used to make the washbasin blocks

## Adding the front block

There are three visibility states in the dynamic block that need to be created. You will start with the front block first.

Follow these steps to create the front block:

- 1. Initially, you will create a regular block of the objects forming the front view on the left. With all the objects selected (as shown with all the grips displayed in *Figure 12.2*), select the **Insert** tab on the ribbon.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_314_1.png)

Figure 12.2: The objects for the front view selected, with grips displayed

2. Click on **Create Block** (*BLOCK*) on the **Block Definition** panel, as shown in Figure 12.3.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_315_0.png)

Figure 12.3: The Create Block (BLOCK) command on the Block Definition panel

3. The **Block Definition** dialog box will appear. In the **Name** field, type in the block name Washbasin_FRONT.

4. Click on the **Pick point** icon, as shown in Figure 12.4.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_315_1.png)

Figure 12.4: The Pick point icon in the Block Definition dialog box

5. You will be taken into the drawing to select an insertion point for your block. Select the Midpoint object snap shown in Figure 12.5.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_316_0.png)

Figure 12.5: The Midpoint object snap to be used as the insertion point

6. Clicking on the Midpoint object snap will return you to the Block Definition dialog box. Note the exact precision of the X and Y coordinates in the dialog box due to using an object snap. You must now set all the settings required to create your dynamic door block.

7. Refer to Figure 12.6 and set all the necessary settings in the Block Definition dialog box as shown.

Figure 12.6: The Block Definition dialog box with all required settings

- 8. Once you have all the necessary settings in the dialog box, click OK.

- 9. The dialog box will close, and your objects will now be a block reference in the drawing. To check that the objects have been converted to a block, hover over the washbasin front view and AutoCAD will provide a tooltip, as shown in Figure 12.7. If that tooltip reads Block Reference, you know that the objects have been converted to a block.

Figure 12.7: The Washbasin_FRONT block indicated by the tooltip

10. Press *Esc* several times to ensure you have deselected the block.

You must now perform the same workflow (*Steps 1–10*) to create the `Washbasin_TOP` and `Washbasin_SIDE` blocks.

## Adding the top and side blocks

You now need to repeat the workflow to add the top and side blocks. Each of these blocks, and the front block, will form a visibility state in the dynamic block. Follow these steps:

- 1. Repeating the preceding workflow from *Step 1* for the `Washbasin_TOP` block; make sure you select the objects that form the top view of the washbasin before clicking on **Create Block** (*BLOCK*).

- 2. Enter the name `Washbasin_TOP` in the **Name** field.

- 3. When using the **Pick point** tool in the **Block Definition** dialog box, you must select the **Midpoint** object snap shown in *Figure 12.8*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_318_0.png)

Figure 12.8: The Midpoint object snap for the Washbasin_TOP block

- 4. Check that all the other settings in the **Block Definition** dialog box match what is shown in *Figure 12.6* and click on **OK**.

- 5. Hover over the new block to check that the tooltip displays **Block Reference**, as shown in *Figure 12.9*. If it does, you have created your Washbasin_TOP regular block.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_319_0.png)

Figure 12.9: The finished Washbasin_TOP block

- 6. Again, repeating the block creation workflow from *Step 1*, you can now start to create the Washbasin_SIDE block.
- 7. Make sure you select the objects that form the side view of the washbasin before clicking on **Create Block** (*BLOCK*).
- 8. Enter the name `Washbasin_SIDE` in the **Name** field.
- 9. This block is a little more detailed, and you need to take into account the curved corner of the washbasin when using the **Pick point** tool in the **Block Definition** dialog box. You need to select an **Object Tracking** point, due to the curve on the corner of the washbasin.
- 10. You must ensure that **Polar Tracking** (*POLAR*), **Object Snap Tracking** (*AUTOSNAP*), and **Object Snap** (*OSNAP*) are active on the status bar, as shown in *Figure 12.10*. If they are active, they will be shown in pale blue. You can then use these drafting settings to select your **Object Tracking** point.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_319_1.png)

Figure 12.10: The settings required on the status bar

11. Zoom in to the upper-right corner of the washbasin side view objects, as shown in *Figure 12.11*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_320_0.png)

12. Click on **Pick point** in the **Block Definition** dialog box.

13. Hover over the two **Endpoint** object snaps shown in *Figure 12.12*, and *do not* click on them. Hovering over the object snaps uses them as tracking points and not object snaps, allowing you to locate the **Object Tracking** point you need.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_320_1.png)

## AutoCAD for Mac

AutoCAD for Mac uses a very similar method for creating dynamic blocks with visibility states. The Create Block (BLOCK) command is on the left-hand menu.

## Adding the necessary blocks for each view

In the previous section of this chapter, you learned how to make the regular blocks that form each view of the washbasin. This section will bring these blocks into the new dynamic block for the washbasin, ready to form the relevant visibility states.

Staying in the Washbasin-DYNAMIC.dwg file, you now need to work through the following steps to start developing your washbasin dynamic block:

- 1. To create a new block in Block Editor, you can directly use the Block Editor icon in the Block Definition panel on the Insert ribbon tab, as shown in Figure 12.15.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_323_0.png)

Figure 12.15: The Block Editor icon on the Block Definition panel

- 2. You will be prompted to edit a block definition with the Edit Block Definition dialog box, where all the regular blocks you created previously will be listed, as shown in Figure 12.16.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_323_1.png)

Figure 12.16: The Edit Block Definition dialog box

![](img/e5541e806f2ef46a4958b6c3d0b346a9_325_0.png)

Figure 12.18: The Midpoint object snap on the Washbasin_TOP block

8. Repeat the process from the Insert drop-down menu, but this time, insert the Washbasin_FRONT block. Again, at the Specify insertion point prompt, use the Midpoint object snap on the Washbasin_TOP block to place the block as shown in Figure 12.19.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_325_1.png)

Figure 12.19: Reusing the Midpoint object snap on the Washbasin_TOP block

9. You now have a somewhat jumbled view in Block Editor of the three regular blocks, as shown in Figure 12.20.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_326_0.png)

Figure 12.20: The three regular blocks placed in Block Editor

You have now placed the three blocks, and you need to start adding visibility states.

> **AutoCAD for Mac**
> Adding regular blocks to Block Editor follows the same workflow in AutoCAD for Mac.

At this point, stay in Block Editor and don’t save anything because, in the next section, we begin the second part of the workflow, and you will start adding the visibility states for the Washbasin_DYN dynamic block.

## Adding visibility states in a dynamic block

In the previous section, you learned how to insert the regular blocks into Block Editor, working toward creating the new Washbasin_DYN dynamic block. In this section, you will develop the relevant visibility state for each regular block, providing a view for each block in the Washbasin_DYN dynamic block. The visibility states are all stored in one dynamic block, and when that dynamic block is inserted into the drawing, you can select which visibility state is displayed. This saves time and avoids the need for three regular blocks, one for each washbasin view.
