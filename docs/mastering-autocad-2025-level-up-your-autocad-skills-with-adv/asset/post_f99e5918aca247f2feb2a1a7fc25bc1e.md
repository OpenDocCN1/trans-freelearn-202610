
15. You will now be prompted for two extension origin points. Use the **Endpoint** object snap and use the top-left and top-right points on the floor plan. You will then need to click a third time to place the dimension line. Figure 8.25 shows the two points to select on the plan and an approximate position of the dimension line. Your placed dimension should look similar to the one in Figure 8.25 when placed. After the third click, the **Linear** dimension command closes.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_222_0.png)

Figure 8.25: The placed Linear dimension type with placement points arrowed

16. Using the **Linear** dimension type, place a few more horizontal and vertical dimensions to get used to the dimension placement workflow. You should end up with a floor plan that looks something like Figure 8.26:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_222_1.png)

Figure 8.26: The floor plan with some more dimension annotation added

## Tips and tricks #27

When placing Linear dimensions, you can line them up accurately using Object Snaps (OSNAP). Simply use the Endpoint object snap of the point of the arrowhead you want to line up with when placing the dimension line – the third click when placing a Linear dimension – and you will find that the arrowheads then line up regularly, keeping the dimension lines all in line. This works for both horizontal and vertical dimension placement.

Object Snaps are the most effective method to use to dimensions accurately and precisely. Make sure that the OSNAP setting is always ON when placing dimension annotation.

## AutoCAD for Mac

Linear dimension placement works in the same way for AutoCAD for Mac. The only difference is that the dimension tools are in the vertical left-hand menu.

Notice that both the text annotation and the dimension annotation are displayed on the Model tab, using their appropriate annotation scales. Click on the ISO A3 layout tab to see the drawing displayed in Figure 8.27:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_223_0.png)

Figure 8.27: The ISO A3 tab with the drawing displayed

In *Figure 8.27*, the text annotation is highlighted, as it only appears in the **1:30** (lower) viewport and not the **1:50** (upper) viewport. This is because the text annotation only has a **1:30** annotation scale applied to it in the **Model** tab. In the same way, the dimension annotation is only set to display in the **1:50** (upper) viewport as it only has a **1:50** annotation scale applied to it in the **Model** tab. This is how annotative scaling works.

In this section, you learned how to use your annotative text and dimension styles with your scaled viewports. Both the text and dimensions placed used a single annotation scale in the **Model** tab to coincide with the scaled viewport where they were displayed in the drawing.

Annotation scales allow easy scaling of annotative objects, in this case, annotative dimensions. When using annotative dimensions, annotation scales can be applied to consistently scale dimension annotation in viewports of differing scales in your layout tabs.

In the next section of this chapter, you will learn how to add and delete annotation scales for annotative objects.

## Adding and deleting object annotation scales

In this section, you will learn that annotative objects can have more than one annotation scale applied to them, thus being able to be displayed in more than one scaled viewport.

You will still have the `Office Plan_GROUND FLOOR.dwg` file open. With it open, follow these steps:

- 1. Return to the **Model** tab in AutoCAD and zoom to the **Executive Office** area.
- 2. Hover over the multiline text object that says **Executive Office**. You can see the annotative symbol visible in *Figure 8.28*:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_224_0.png)

3. Select the annotative multiline text with a click. You will see the blue grips appear. Right-click to display the context-sensitive shortcut menu, as shown in Figure 8.29:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_225_0.png)

Figure 8.29: The shortcut menu with Annotative Object Scale - Add/Delete Scales... displayed

4. Select Annotative Object Scale from the shortcut menu, and then click on Add/Delete Scales... from the sub-menu.

5. The Annotation Object Scale dialog box is displayed, as shown in Figure 8.30. You will notice that the only annotation scale applied to the annotative text is the 1:30 scale. Click on the Add... button:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_225_1.png)

Figure 8.30: The Annotation Object Scale dialog box

8. Click on OK to close the dialog box. You will notice that the annotative text is now larger in the Model tab, as shown in Figure 8.33. This is because you now have two annotation scales applied, and the current annotation scale is 1:50, which defines its size in the drawing. Also note that when hovering over the annotative text object, two annotative symbols are displayed, indicating more than one annotation scale has been applied.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_227_0.png)

Figure 8.33: The annotative text object with two annotative symbols displayed

9. If you return to the ISO A3 layout tab, you will also see that the Executive Office text object is displayed in the 1:50 (upper) viewport. This is because it now has the 1:50 annotation scale applied to it in the Model tab. Figure 8.34 shows this, and you can see that using that text style with the 1:50 annotation scale in that viewport causes it to cross geometry and look a bit untidy.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_227_1.png)

Figure 8.34: The annotative text object in the 1:50 viewport

- 10. Return to the **Model** tab. Select the **Executive Office** text object, and the blue grips will appear.
- 11. Right-click to display the shortcut menu. Select **Annotative Object Scale**. The **Annotative Object Scale** dialog box will be displayed as it was previously in the section.
- 12. Select **Add/Delete Scales...** in the sub-menu.
- 13. Select the **1:50** scale in the list.
- 14. Click the **Delete** button, and the **1:50** scale will be deleted as an applied annotation scale for the text object.
- 15. Click on **OK** to close the dialog box, like before.
- 16. The annotative text object now only has the one annotation scale applied, as before.
- 17. If you return to the **ISO A3** layout tab, you will see that the **Executive Office** text object is now only displayed in the **1:30** (lower) viewport, as shown in Figure 8.35.
- 18. Make sure to save your drawing as `Office Plan_GROUND FLOOR-COMPLETE.dwg`. That way, you save all of the revisions you have made, and you can refer to the file for future reference.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_228_0.png)

Figure 8.35: The ISO A3 layout tab with the annotative text object displayed

You can now add and delete annotation scales to your annotative objects, including text and dimensions. Provided you place the objects in the drawing using an annotative style, such as text and dimension styles, any annotative object can have more than one annotation scale applied to it, allowing it to be displayed in multiple viewports at differing scales in the layout tabs in a drawing.

## Part 3:
Content Re-Use Including
Dynamic Blocks

Re-using design content in AutoCAD provides you with the flexibility to use similar content again and again. Working on a project that uses specific content that can be reused in another similar project saves valuable project time, reducing repetitive, onerous content creation. Re-using content from your DWG files also allows for the easy creation of content libraries using regular AutoCAD blocks and dynamic blocks that incorporate specific parameters and actions, allowing for easy manipulation of the dynamic blocks when working with them in your drawings.

This part has the following chapters:

- Chapter 9, Developing Block Libraries
- Chapter 10, Working with Block Attributes
- Chapter 11, Creating a Dynamic Block with Parameters and Actions
- Chapter 12, Creating a Dynamic Block with Visibility States
