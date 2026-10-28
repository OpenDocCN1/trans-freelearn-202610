
# Get This Book’s PDF Version and Exclusive Extras

Scan the QR code (or go to packtpub.com/unlock). Search for this book by name, confirm the edition, and then follow the steps on the page.

> *Note:* Keep your invoice handy. Purchases made directly from Packt don’t require an invoice.

![](img/0972ba626734a3986e095c788777818e_470_0.png)

## Technical requirements

You will continue working on the JRNL project that you modified in the previous chapter.

The resource files and completed Xcode project for this chapter are in the Chapter 22 folder of the code bundle for this book, which can be downloaded here:

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

Check out the following video to see the code in action:

https://youtu.be/AcqGtM4hQL4

Let’s start by learning about collection views in the next section.

## Collection views

A collection view is an instance of the UICollectionView class. It manages an ordered collection of elements and presents them using customizable layouts.

To learn more about collection views, visit https://developer.apple.com/documentation/uikit/uicollectionview.

The data displayed by a collection view is usually provided by a view controller. A view controller providing data for a collection view must adopt the UICollectionViewDataSource protocol. This protocol declares a list of methods that tells the collection view how many cells to display and what to display in each cell.

To learn more about the UICollectionViewDataSource protocol, visit https://developer.apple.com/documentation/uikit/uicollectionviewdatasource.

## Modify the Journal List screen to use a collection view

At present, the Journal List screen in the JRNL app uses a table view. A table view presents table view cells using rows arranged in a single column. This works great on an iPhone, but if you were to run the app on an iPad, you’d see there is a lot of wasted screen space on the Journal List screen, as shown in the following figure:

![](img/0972ba626734a3986e095c788777818e_474_0.png)

Figure 22.1: Simulator showing Journal List screen containing a table view on an iPad

To address this, you’ll replace the table view with a collection view, which will allow you to more effectively use the available screen space, as shown in the following figure:

![](img/0972ba626734a3986e095c788777818e_474_1.png)

Figure 22.2: Simulator showing Journal List screen containing a collection view on an iPad

2. Build and run your app, and note how it appears on the iPad’s screen:

![](img/0972ba626734a3986e095c788777818e_476_0.png)

Figure 22.4: Simulator showing iPad screen

Although the app works as it should, note that a lot of space is wasted on the right side of the Journal List screen.

3. Click the Stop button. Click the Main storyboard file in the Project navigator. In the document outline, click the table view under Journal Scene. Press the Delete key to remove it:

![](img/0972ba626734a3986e095c788777818e_476_1.png)

Figure 22.5: Editor area showing table view selected in the document outline

6. Type 0 in the top, left, right, and bottom edge constraint fields and click all the pale red struts. Make sure all the struts have turned bright red and Constrain to margins is not checked. Then, click the Add 4 Constraints button:

![](img/0972ba626734a3986e095c788777818e_479_0.png)

Figure 22.8: Auto Layout – Add New Constraints pop-up dialog box

This sets the space between the edges of the collection view and the edges of the enclosing view to 0, binding the collection view’s edges to those of the enclosing view.

8. With the collection view still selected, click the Size inspector button. Under Collection View, set Estimate Size to None.

Figure 22.10: Size inspector with Estimate Size highlighted

You’ll be adding code to determine the collection view size dynamically later.

9. You’ll need to re-establish the segue between the Journal List screen and the Journal Entry Detail screen. Ctrl + Drag from Collection View Cell in the document outline to the Entry Detail scene in the Editor area and choose Show from the pop-up menu.

Figure 22.11: Editor area showing drag destination

10. Click the newly added storyboard segue and click the Attributes inspector button. Under Storyboard Segue, set Identifier to entryDetail.

![](img/0972ba626734a3986e095c788777818e_482_0.png)

Figure 22.12: Attributes inspector with Identifier set to entryDetail

You have added a collection view to the Journal Scene and used Auto Layout constraints to make it fill the screen, but the prototype collection view cell is currently empty. You'll add UI elements to the collection view cell in the next section.

## Add UI elements to the collection view cell

You have replaced the table view inside the Journal Scene with a collection view, but the prototype collection view cell inside the collection view is empty. You'll need to add an image view and two labels to the prototype collection view cell and set up the constraints for them. This will make it match the table view cell used previously. Follow these steps:

- 1. Select Collection View Cell for the Journal Scene in the document outline. Drag the right edge of the collection view cell to the right until it reaches the right side of the screen:

![](img/0972ba626734a3986e095c788777818e_482_1.png)

Figure 22.13: Editor area showing Collection View Cell

Constrain to margins should not be checked. When done, click the Add 4 Constraints button:

![](img/0972ba626734a3986e095c788777818e_484_0.png)

Figure 22.15: Constraints for image view

5. Click the Attributes inspector button. Under **Image View**, set **Image** to `face.smiling`:

![](img/0972ba626734a3986e095c788777818e_485_0.png)

*Figure 22.16: Image View with Image set to face.smiling*

6. Next, you’ll add a label to display the journal entry date. Click the Library button. Type label into the filter field. A **Label** object will appear in the results. Drag it to the space between the image view you just added and the right side of the cell:

![](img/0972ba626734a3986e095c788777818e_485_1.png)

*Figure 22.17: Prototype cell with label added*

7. In the Attributes inspector, under Label, set Font to Title 1 using the Font menu:

![](img/0972ba626734a3986e095c788777818e_486_0.png)

Figure 22.18: Attributes inspector for Label

8. Click the Add New Constraints button and enter the following values to set the constraints for the label:

- Top: 0
- Left: 8
- Right: 0

Constrain to margins should be checked, which sets a standard margin of 8 points. When done, click the Add 3 Constraints button.

![](img/0972ba626734a3986e095c788777818e_487_0.png)

Figure 22.19: Constraints for label

9. Finally, you'll add a label to display the journal entry title. Click the Library button. Type label into the filter field. A Label object will appear in the results. Drag it to the space between the label you just added and the bottom of the cell:

![](img/0972ba626734a3986e095c788777818e_487_1.png)

Figure 22.20: Prototype cell with second label added

10. In the **Attributes** inspector, under **Label**, set **Font** to **Body** using the **Font** menu, and set **Lines** to 2:

![](img/0972ba626734a3986e095c788777818e_488_0.png)

Figure 22.21: Attributes inspector for Label

11. Click the Add New Constraints button and enter the following values to set the constraints for the label:

- Top: 0
- Left: 8
- Right: 0

**Constrain to margins** should be checked, which sets a standard margin of 8 points. When done, click the **Add 3 Constraints** button.

![](img/0972ba626734a3986e095c788777818e_489_0.png)

Figure 22.22: Constraints for second label

The prototype collection view cell now has an image view and two labels, and all the necessary constraints have been added. Fantastic! In the next section, you'll modify the JournalListTableViewCell class to manage the collection view cell's contents.

## Modify the JournalListTableViewCell class

Originally, the JournalListTableViewCell class was used to manage the table view instance's table view cells in the Journal List screen. Since you have replaced the table view with a collection view, all the connections between the JournalListTableViewCell class will need to be re-established to work with the UI elements that you just added to the collection view cell.
