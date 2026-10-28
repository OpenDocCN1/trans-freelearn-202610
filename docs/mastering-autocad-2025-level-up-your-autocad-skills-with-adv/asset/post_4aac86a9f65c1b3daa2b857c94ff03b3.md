
6. Select **PC_ID** in the attribute list. Use the **Move Up** button in the dialog box and move it to the top of the attribute list. Using **Move Up** and **Move Down** as you select an attribute, change the list order to as it is displayed in *Figure 10.23*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_282_0.png)

Figure 10.23: The reordered attribute list

7. Click on **Apply** and then on **OK**, making sure that the changes are applied. **Block Attribute Manager** will now close.

8. Using the skills learned in the previous section, insert another instance of the `PC_Attributes` block in the drawing. You can place it anywhere, as it is the attributes you need to look at. Repositioning can be sorted after the block is inserted using object snaps.

9. The **Edit Attributes** dialog box appears after clicking to position the block, as shown in *Figure 10.24*.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_283_0.png)

Figure 10.24: The Edit Attributes dialog box

10. The attribute order has changed in the Edit Attributes dialog box to match the display order of the attributes in the drawing, as per the changes made using Block Attribute Manager (BATTMAN). This is a great visual timesaver as you can now check and edit your attributes in the same order as they are displayed in the drawing, thus cutting down on errors.

### AutoCAD for Mac

The Block Attribute Manager (BATTMAN) palette works in the same way in AutoCAD for Mac.

You can now create and manage your own blocks with attributes, developing them from scratch, and saving them in your DWG files. You can also edit and manage your block attributes using Enhanced Attribute Editor and the Block Attribute Manager (BATTMAN).

As you build your AutoCAD knowledge, you can experiment with both tools to develop extensive block libraries, managing your libraries effectively using the skills learned in Chapter 9 by creating block libraries for your blocks with attributes.

You can learn more about AutoCAD block attributes on the Autodesk website:

https://help.autodesk.com/view/ACD/2023/ENU/?guid=GUID-67A2DDAD-2217-412F-8AEF-D4495192F45B

You now know how to work with, edit, and manage your attributes in your AutoCAD blocks, and using the knowledge gained in *Chapter 9*, these attributed blocks can then be added to a block library drawing that is referenced in the **Blocks** palette, or you could develop your own tool palettes.

## Summary

In this chapter, you learned about creating and editing attributes in an AutoCAD block and inserting the block into a DWG file.

You also learned how to edit individual attribute properties using **Enhanced Attribute Editor**. Also, using **Block Attribute Manager** (*BATTMAN*), you learned how to work with and manage block attributes.

Effective block attribute management is incredibly important when using blocks with attributes in your drawings. The information must be accurate and correct and displayed effectively in your drawings.

As you work with AutoCAD drawings on larger projects, the skills you learned in this chapter will give you a good grounding and understanding of how to start implementing blocks with attributes in your AutoCAD designs.

The tools used in this chapter demonstrate how important attributes can be to relay block information to the CAD team and project stakeholders using your drawings, saving valuable time. In the next chapter, we will look at dynamic blocks in more depth, so that you can learn how to create and manage dynamic blocks, adding parameters and actions.

## 11 Creating a Dynamic Block with Parameters and Actions

In the previous chapter, you learned how to create attributes in AutoCAD blocks. In this chapter, you will look at how those block attributes can be applied to the dynamic block you will create. However, you will concentrate on creating a dynamic block with parameters and actions.

As you know, blocks are objects made up of multiple AutoCAD objects for content reuse, such as a chair or a desk, to avoid having to rework that chair or desk repetitively in a drawing. Making a dynamic block of the chair or desk gives you even more versatility in the block itself, providing the ability to manipulate the block quickly and easily.

Dynamic blocks need both a parameter and an associated action, so if you were working with a block representing a door, you may need to add a parameter and action to control the swing direction of the door in the door opening in the drawing. Many parameters and actions are available in AutoCAD for dynamic blocks, and this chapter covers some of the methodologies you will need to start working with them in your drawings.

In this chapter, you will learn how adding dynamic parameters and actions to your blocks will make them more versatile and allow for much easier manipulation of the blocks in your AutoCAD drawings.

This chapter covers the following topics:

- Creating the regular door block
- Adding a vertical flip to the door
- Adding a horizontal flip to the door
- Testing the new dynamic block in the drawing

### Exercise file

You will need to use the exercise file, `New Office_Dynamic Blocks.dwg`, for this chapter.

### Creating the regular door block

In this section, we will be using AutoCAD objects to create a regular door block that can then be made dynamic. Open the exercise file in the **Model** tab and zoom into the door opening. Once zoomed into the door opening, you will see the objects that will be used to make the regular door block. There are two objects: a rectangular polyline and an arc. Both are on layer 0 (**zero**). When creating the regular door block, make sure to include both objects so that the block will be displayed in full visually when in use. You can see, in *Figure 11.1*, that both objects are selected, and the blue grips are displayed.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_286_0.png)

*Figure 11.1: The objects to be used to make the regular door block*

To create the door block, follow these steps:

1. Select the **Insert** tab on the ribbon with both objects selected. Then, click on **Create Block (BLOCK)** on the **Block Definition** panel, as shown in *Figure 11.2*.

   ![](img/e5541e806f2ef46a4958b6c3d0b346a9_286_1.png)

   *Figure 11.2: The Create Block (BLOCK) command on the Block Definition panel*

2. The **Block Definition** dialog box will appear. In the **Name** field, type in the block name, Door-DYN (*DYN* is short for dynamic).
3. Click on the **Pick point** icon in *Figure 11.3*.

   ![](img/e5541e806f2ef46a4958b6c3d0b346a9_287_0.png)

   Figure 11.3: The Pick point icon in the Block Definition dialog box

4. You will be taken into the drawing to select an insertion point for your block. Select the Endpoint object snap, as shown in Figure 11.4.

   ![](img/e5541e806f2ef46a4958b6c3d0b346a9_288_0.png)

   Figure 11.4: The Endpoint object snap to be used as the insertion point

5. Clicking on the Endpoint object snap will return you to the Block Definition dialog box. Note the exact precision of the X and Y coordinates in the dialog box due to using an object snap. You now need to set all the settings required to create your dynamic door block.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_290_0.png)

Figure 11.6: Block Editor open, ready to create the dynamic block

Your door block is now created and ready to start adding your dynamic parameters and actions. The next two sections will explain how to add parameters and actions to your dynamic door block while working in Block Editor in AutoCAD.

> Tips and tricks #31
> When selecting the objects to create an AutoCAD block, it is often more beneficial to select the objects that form the block before clicking on the Create Block (BLOCK) command. To make sure you can do this, you need Noun/verb selection checked in the Selection tab in the Options dialog box, as shown in Figure 11.7. You can open the Options dialog box from the Application menu, right-click the shortcut menu, or type OPTIONS and press Enter.
