
- 3. Double-click the label and change the label text to Get Location:

![](img/0972ba626734a3986e095c788777818e_262_0.png)

Figure 13.25: Label text changed to Get Location

- 4. You’ll embed both the label and the switch in a horizontal stack view. In the document outline, hold down the Shift key, click Switch, and then click Get Location to select both the switch and the label:

![](img/0972ba626734a3986e095c788777818e_262_1.png)

Figure 13.26: Document outline showing both label and switch selected

- 5. Choose Embed In | Stack View from the Editor menu.

![](img/0972ba626734a3986e095c788777818e_263_0.png)

Figure 13.27: Editor menu with Embed In | Stack View selected

Both the switch and the label are now embedded in a stack view.

- 2. In the Attributes inspector, under Text Field, set Placeholder to Journal Title:

![](img/0972ba626734a3986e095c788777818e_266_0.png)

Figure 13.30: Attributes inspector with Placeholder set to Journal Title

- 3. To add a text view to the scene, click the Library button. Type text into the filter field. A Text View object will appear in the results. Drag it to the view of the New Entry scene under the text field you just added:

![](img/0972ba626734a3986e095c788777818e_266_1.png)

Figure 13.31: Library with Text Field object selected

## 开放性伤口处理

如果发生切口渗漏，则需要缝合水密伤口。更常见的情况是，不会形成完全水密愈合。大约70%的病例将拥有防水伤口，尽管未缝合的小切口白内障手术(SICS)可能存在轻微的渗漏途径，这可能导致角膜水肿、浅前房、脉络膜渗出或睫状体脱离。理想的切口应具有自密封性，不需要缝合。穹隆部为基底的结膜瓣通常用于青光眼手术，不用于白内障手术。

## 前房穿刺

用尖刀片（15度或20度刀片）在角膜上做前房穿刺，后续的步骤通过该穿刺进行。

## 伤口构建

根据医生的经验和制定的计划，按照以下任何一种方法进行。

## 囊膜切开（CCC）

在白内障手术中，使用连有粘弹剂的钝针或囊膜镊进行连续环形撕囊术（CCC）是不可缺少的步骤。应设计和创建规定尺寸和中心的CCC。首先，用Utrata镊子进行初始撕裂或穿刺，然后用镊子抓住产生的撕裂瓣，并在圆周方向上移动。

## Add an image view to the New Entry scene

As shown in the app tour, the user can use the device camera to take a photo for a journal entry. The selected photo will be shown in the Add New Journal Entry screen using an image view. In Chapter 11, Build the User Interface, you added an image view to the journalCell table view cell. Now, you will add an image view to the New Entry scene. Follow these steps:

- 1. To add an image view to the scene, click the Library button. Type imag into the filter field. An Image View object will appear in the results. Drag it to the view of the New Entry scene under the text view:

![](img/0972ba626734a3986e095c788777818e_268_0.png)

Note that the image view you just added appears in the document outline and is a subview of the view for New Entry Scene.

- 2. Click the Size inspector button. Under View, set both Width and Height to 300.

- 3. Click the Attributes inspector button. Under Image View, set Image to face.smiling.

- 4. You’ll use constraints to set the width and height for the image view. Click the Add New Constraints button and check the Width and Height constraints (their values should already be set to 300). After that, click the Add 2 Constraints button. The image view will now look like the following screenshot:

![](img/0972ba626734a3986e095c788777818e_269_0.png)

Figure 13.34: Image View with constraints applied

Note the red outlines around the image view, as its position relative to the enclosing view is ambiguous. Don’t worry about it now, as you will fix it later.

All the user interface elements for the New Entry scene have been added. In the next section, you’ll embed all of them in a vertical stack view to resolve the positioning issues.

## Embed user interface elements in a stack view

The New Entry scene now has all the required user interface elements, but the position of the elements relative to the enclosing view is ambiguous. You'll embed all the elements in a vertical stack view and use constraints to resolve the positioning issues. Follow these steps:

- 1. In the document outline, select all the user interface elements that you added earlier, as shown here:

![](img/0972ba626734a3986e095c788777818e_270_0.png)

Figure 13.35: Document outline showing selected elements

- 2. Choose Embed In | Stack View from the Editor menu.

- 3. Select Stack View in the document outline. In the Attributes inspector, under Stack View, set Alignment to Center and Spacing to 8.

- 4. With the stack view selected, click the Add New Constraints button. Enter the following values to set the constraints for the stack view:
  - Top: 20
  - Left: 20
  - Right: 20

- 7. Select Text View in the document outline and click the Add New Constraints button. Set the left and right constraints to 8, and click the Add 2 Constraints button. The text view will be extended to almost the full width of the stack view. Verify that the text field and text view appear as shown in the following screenshot:

![](img/0972ba626734a3986e095c788777818e_273_0.png)

Figure 13.38: Text field and text view in the stack view with a constraint applied

All the positioning issues have now been resolved.

As you can see, the Journal Entry Detail screen has the following elements:

- A label showing the date
- A custom view showing star ratings
- A label for the journal entry title
- A label for the journal entry body
- An image view for a photo that you will take with your phone’s camera
- An image view showing a map location

Also, you need to scroll to view the entire screen. You will now modify it to match the design shown in the app tour, beginning by setting the number and size of the table view cells in the next section.

## Configure the number and size of static table view cells

In Chapter 12, Complete the User Interface, you added a table view controller scene to the Main storyboard file. This will represent the Journal Entry Detail screen. Unlike the Journal List screen, the Journal Entry Detail screen will always show the same number of cells, with each cell containing a user interface element. You will now set the number and size of the cells to match the layout shown in the app tour. Follow these steps:

- 1. In the Main storyboard file, expand Table View Controller Scene in the document outline, and click Navigation Item:

![](img/0972ba626734a3986e095c788777818e_277_0.png)

Figure 13.42: Document outline with Table View Controller Scene selected

- 2. Click the Attributes inspector button, and under Navigation Item, set Title to Entry Detail.

- 3. Expand Table View and select Table View Section in the document outline:

![](img/0972ba626734a3986e095c788777818e_278_0.png)

Figure 13.43: Document outline with Table View Section selected

- 4. In the Attributes inspector under Table View Section, set Rows to 7.

![](img/0972ba626734a3986e095c788777818e_278_1.png)

Figure 13.44: Attributes inspector showing 7 rows

- 5. Click the second Table View Cell under Table View Section in the document outline:

![](img/0972ba626734a3986e095c788777818e_279_0.png)

Figure 13.45: Document outline showing the second Table View Cell

- 6. Click the Size inspector button. Under Table View Cell, set Row Height to 60:

![](img/0972ba626734a3986e095c788777818e_279_1.png)

Figure 13.46: Size inspector showing Row Height set to 60

- 7. Click the fourth Table View Cell under Table View Section in the document outline. In the Size inspector, under Table View Cell, set Row Height to 150.

- 8. Click the fifth Table View Cell under Table View Section in the document outline. In the Size inspector, under Table View Cell, set Row Height to 316.

- 9. Repeat the previous step with the sixth Table View Cell.

You have set the number and size of the cells to match the layout shown in the app tour. In the next section, you'll add the user interface elements to each cell.

## Add user interface elements to static table view cells

In the previous section, you used a stack view to help you manage multiple user interface elements in the Add New Journal Entry screen. Here, you'll use a table view with static table view cells instead. The advantage of using a static table view is that view scrolling is built in, so it can accommodate views that are taller than the device screen. Follow these steps:

- 1. In the document outline, click the first Table View Cell, and drag a Label object into it from the library. In the Attributes inspector, under Label, use the Font menu to set Style to Semibold and set Alignment to right.

- 2. Click the Add New Constraints button, and set the top, left, and right constraints of the label to 0. Make sure that Constrain to Margins is checked, and click the Add 3 Constraints button. The label will now look like the following screenshot:

![](img/0972ba626734a3986e095c788777818e_280_0.png)

Figure 13.47: Label with the constraints applied

- 3. In the document outline, click the second Table View Cell, and drag a Horizontal Stack View object into it from the library. In the Attributes inspector, under Stack View, set Spacing to 8. Under View, set Background to System Cyan Color.

- 4. Click the Size inspector. Under View, set Width to 252 and Height to 44.

- 5. Click the Add New Constraints button and set the Width and Height constraints (their values should already be present). Click the Add 2 Constraints button.
