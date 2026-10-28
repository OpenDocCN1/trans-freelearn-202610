
- 4. Click on **Visibility** in the **Block Authoring Palette**, as shown in *Figure 12.22*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_328_0.png)

Figure 12.22: The Block Authoring Palette with the Visibility parameter indicated

- 5. You will be prompted for the parameter's location. You need to select a point close to the regular blocks but not directly on them, as shown in *Figure 12.23*. This is because the **Visibility** parameter will be displayed on the dynamic block when it is used in a drawing.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_328_1.png)

Figure 12.23: The Visibility parameter shown placed in Block Editor

- 6. You can see the *yellow exclamation* mark like in *Chapter 11*, which means that the **Visibility** parameter needs more information to function. In this case, it needs objects (the regular blocks) to represent each visibility state in the dynamic block. You don't need to name the parameter, but it can be done by selecting it, right-clicking and selecting **Properties** on the shortcut menu, and renaming it in the **Properties** palette.
- 7. Look at the ribbon. You are in the blue contextual **Block Editor** ribbon tab. Look for the **Visibility** ribbon panel, as shown in *Figure 12.24*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_329_0.png)

Figure 12.24: The Visibility panel on the ribbon

In *Figure 12.24*, there are two icons arrowed. The right-hand arrow points to the icon that sets the AutoCAD BVMODE system variable, called **Visibility Mode**. Clicking on this icon turns BVMODE either on (**1**) or off (**0**). You will need it on, as any visibility states set to invisible will then appear grayed out, rather than invisible, which is much easier to work with. If BVMODE is set to zero (**0**), you only see the current visibility state. This will become clearer as you go through this workflow. Click on the icon and look at the command line, and if you see BVMODE set to **1**, as shown in *Figure 12.25*, you are all set. If it says zero (**0**), click the icon again to set it to **1**.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_329_1.png)

Figure 12.25: Setting the BVMODE system variable to 1

- 8. Click on the **Visibility States** icon shown in *Figure 12.24*.
- 9. The **Visibility States** dialog box will display a default visibility state called **VisibilityState0**. Click on **New**, and in the **New Visibility State** dialog box, set the new state name to **Washbasin-SIDE**. Set the visibility options to the third option, **Leave visibility of existing objects unchanged in new state**, as shown in *Figure 12.26*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_330_0.png)

*Figure 12.26:* The New Visibility State dialog box with settings arrowed

- 10. Click on **OK**. The **Visibility States** dialog box will then be shown, as in *Figure 12.27*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_330_1.png)

*Figure 12.27:* The Visibility States dialog box

- 11. The **Washbasin-SIDE** visibility state is current, as indicated by the check mark. You can now select the default state, **VisibilityState0**, and click **Delete** to remove it.

- 12. Click on **OK** to close the **Visibility States** dialog box. You are now in the **Washbasin-SIDE** visibility state and need to set which regular blocks are visible and invisible.

- 13. The ribbon's **Visibility** panel shows which visibility state you use. Select the regular block that displays the side view of the washbasin, as shown in *Figure 12.28*, and click on the green square icon, also shown in *Figure 12.28*. This sets that block as visible in the visibility state.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_331_0.png)

Figure 12.28: The selected side view block being made visible

The side view will be deselected and is now set as visible in the visibility state.

- 14. Select the two other blocks representing the top and front views, as shown in Figure 12.29. Then, click the green dashed icon, also shown in Figure 12.29. This will make them both to be invisible in the current visibility state.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_332_0.png)

Figure 12.29: The other two blocks selected to be made invisible

- 15. Once you click the icon, the two blocks will be deselected and displayed grayed out due to the BVMODE system variable being set to 1 earlier. This is shown in Figure 12.30.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_332_1.png)

Figure 12.30: The visibility state with the side view visible

You will note that you still have a yellow exclamation mark. This is because you only have one state set at present.

## Adding two more visibility states

The two remaining visibility states must be set up in the dynamic block. This section will repeat the creation of the first visibility state, but you will create two more visibility states, using the other two regular blocks:

- 1. Click on the Visibility States icon again to display the Visibility States dialog box. Click on New.
- 2. In the New Visibility State dialog box, set another state name called Washbasin-TOP, using the same options as Step 9 previously. Click on OK.
- 3. In the Visibility States dialog box, the new Washbasin-TOP state is now current (checked). Click on OK. You now need to repeat the visible/invisible process.
- 4. Check that the Washbasin-TOP state is current in the Visibility panel. Then, select the regular block showing the top view of the washbasin, as shown in Figure 12.31. Click on the green icon shown in Figure 12.31 to make it visible in the current state.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_333_0.png)

Figure 12.31: Setting the top view as visible

- 5. The top view block will be deselected automatically and is now set as visible in the **Washbasin-TOP** visibility state. Note that the yellow exclamation mark has gone because you now have more than one visibility state.
- 6. Like before, you must select the blocks representing the washbasin's side and front views and then click on the green dashed icon in the **Visibility** panel to mark them as invisible.
- 7. Again, they will automatically be deselected. They are now marked as invisible in the **Washbasin-TOP** visibility state and grayed out, as shown in *Figure 12.32*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_334_0.png)

Figure 12.32: The visibility state with the top view visible

- 8. Repeat the preceding workflow (*Steps 1-7*) one more time, creating a third visibility state called **Washbasin-FRONT**, where the block displaying the front view of the washbasin is visible.
- 9. The **Washbasin-FRONT** visibility state should look like *Figure 12.33*, where the block displaying the front view is visible and the other two side and top blocks are invisible (grayed out).

![](img/e5541e806f2ef46a4958b6c3d0b346a9_335_0.png)

Figure 12.33: The visibility state with the front view visible

To toggle between visibility states, you can set the current state using the drop-down menu on the **Visibility** panel, as shown in *Figure 12.34*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_335_1.png)

Figure 12.34: The visibility state drop-down menu

You have now set up all three required visibility states in the **Washbasin_DYN** dynamic block.

- 10. Click on **Close Block Editor** *(the green check mark on the ribbon)* to close **Block Editor** and return to the drawing.

- 11. Once you are back in the drawing after leaving **Block Editor**, a prompt will appear. Make sure to select **Save the changes to Washbasin_DYN**.

You are now back in the Washbasin-DYNAMIC.dwg file, and you can see the regular blocks you created previously that were used as visibility states in the new dynamic block.

Make sure to save the drawing so that your new dynamic block is saved in the DWG file.

**AutoCAD for Mac**

Adding visibility states in **Block Editor** follows the same workflow in AutoCAD for Mac.

You have now created your new **Washbasin_DYN** dynamic block with visibility states. In the next section of this chapter, you will bring the block into the drawing and use the visibility states to display different views of the washbasin.

## Using the new dynamic block with visibility states

In the previous section, you learned how to use regular blocks to create visibility states of different views of the washbasin in a new dynamic block. In this section, you will learn how to insert the new dynamic block into the drawing and utilize the visibility states.

Throughout this chapter, you have worked on the default layer, layer zero (0). This means your new dynamic block will assume the current layer when inserted in the drawing. The following workflow will allow you to fully test the dynamic washbasin block:

- 1. On the **Home** tab on the ribbon, check that the **Sanitary** layer is current on the **Layers** panel.
- 2. Using zoom and pan functions, ensure you work in a space away from the existing blocks.
- 3. Click on the **Insert** tab on the ribbon, then click on the **Insert** dropdown at the left-hand end. *Figure 12.35 shows the **Washbasin_DYN** block shown in the dropdown.*

![](img/e5541e806f2ef46a4958b6c3d0b346a9_336_0.png)

Figure 12.35: The Washbasin_DYN dynamic block on the Insert dropdown
