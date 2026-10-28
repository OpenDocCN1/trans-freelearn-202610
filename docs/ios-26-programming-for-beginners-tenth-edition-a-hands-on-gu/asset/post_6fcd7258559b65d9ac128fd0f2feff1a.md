
+   5. Build and run your app. It should work just as it did before.

You have created the Journal List and Map screens for your app! Well done!

## Summary

In this chapter, you learned some useful terms used in iOS app development. This will make it easier for you to understand the remainder of this book, as well as other books or online resources on the subject.

Then, you also learned about the different screens used in the JRNL app and how a user could use the app. As you recreate the app’s UI from scratch, you’re able to compare what you’re doing to what the actual app looks like.

After that, you learned how to use Interface Builder and storyboards to add a tab bar controller scene to your app and configure the button titles and icons. Finally, you added navigation controllers for the Journal List and Map screens. This will familiarize you with adding and configuring UI elements for your own apps.

In the next chapter, you will continue setting up your app’s UI and become familiar with more UI elements. You will also add and configure the remaining screens for your app.

## Join us on Discord

Read this book alongside other users, experts, and the author himself.

Ask questions, provide solutions to other readers, chat with the author via Ask Me Anything sessions, and much more. Scan the QR code or visit the link to join the community.

https://packt.link/LdIDT/

![](img/0972ba626734a3986e095c788777818e_191_0.png)

## 11

## Build the User Interface

In the previous chapter, you modified an existing Xcode project, added a tab bar to your app that allowed the user to select between the Journal List and Map screens, and configured the tab bar button titles and icons. When your app is launched, the Journal List screen is displayed, but it is currently blank.

As you saw in the app tour in *Chapter 10, Set Up the User Interface*, the Journal List screen should display a table view showing a list of journal entries in table view cells.

In this chapter, you will make the Journal List screen display a table view containing 10 empty table view cells, as well as a button that will display a view representing the Add New Journal Entry screen when tapped. You’ll also configure a Cancel button to dismiss this view and return you to the Journal List screen.

You’ll be adding a small amount of code to your app, but don’t worry too much about this—you’ll learn more about it in the next part of this book.

By the end of this chapter, you’ll have learned how to add view controllers to a storyboard scene, link outlets in view controllers to scenes, set up table view cells, and present a view controller modally. This will be very useful when you’re designing the user interface for your own apps.

The following topics will be covered in this chapter:

- Add a table view to the Journal List screen
- Connect storyboard elements to the view controller
- Configure data source methods for the table view
- Present a view modally

3. You will use the Auto Layout Add New Constraints button to bind the edges of the table view to the edges of its enclosing view. Make sure the Table View is selected and click the Auto Layout Add New Constraints button:

![](img/0972ba626734a3986e095c788777818e_197_0.png)

Figure 11.3: View controller scene with Table View selected

4. Type 0 in the top, left, right, and bottom edge constraint fields and click all the pale red struts. Make sure all the struts have turned bright red. Click the Add 4 Constraints button:

![](img/0972ba626734a3986e095c788777818e_198_0.png)

Figure 11.4: Auto Layout pop-up dialog box for adding new constraints

This sets the space between the edges of the table view and the edges of the enclosing view to 0, binding the table view’s edges to those of the enclosing view. Now, the table view will fill the available space, regardless of the device and orientation.

You have added a table view to the view of the view controller scene for the Journal List screen and used Auto Layout constraints to make it fill the space between the navigation and tab bars, but the Journal List screen will still be blank when you build and run your app.

In the next section, you will implement the code for the JournalListViewController class, and you’ll connect outlets in this class to the UI elements on the Journal List screen. This will enable an instance of the JournalListViewController class to control what is displayed on the Journal List screen.

## Connect storyboard elements to the view controller

You’ve added a table view to the Journal List screen, but it does not display anything yet. You’ll need to modify the existing view controller to manage the table view in the Journal List screen. The ViewController file was automatically created by Xcode when you created the JRNL project.

It contains the declaration and definition of a UIViewController subclass named ViewController, and this class is currently set as the view controller for the Journal List screen. You’ll change the name of the class in the ViewController file to JournalListViewController and create an outlet for the table view that you added to the view controller scene earlier. Follow these steps:

+   1. Click the ViewController file in the Project navigator. In the Editor area, right-click the class name (ViewController) and choose Refactor | Rename....

![](img/0972ba626734a3986e095c788777818e_200_0.png)

Figure 11.6: Editor area showing the pop-up menu with Rename... highlighted

2. Change the class name to JournalListViewController and click Rename:

![](img/0972ba626734a3986e095c788777818e_201_0.png)

Figure 11.7: Editor area showing the new name for the ViewController class

3. Verify that both the class name and the filename have been changed to JournalListViewController:

![](img/0972ba626734a3986e095c788777818e_201_1.png)

Figure 11.8: Filename and class name both changed to JournalListViewController

4. Click the **Main** storyboard file in the Project navigator and select the first **Journal Scene** (the one containing the table view) in the document outline.

![](img/0972ba626734a3986e095c788777818e_202_0.png)

Figure 11.9: Document outline showing the first Journal Scene option selected

5. Click on the Identity inspector button and verify that, under **Custom Class**, **Class** is set to **JournalListViewController**:

![](img/0972ba626734a3986e095c788777818e_202_1.png)

Figure 11.10: Identity inspector with Class set to JournalListViewController

This means that the content of the Journal List screen is being managed by an instance of the JournalListViewController class.

9. To connect the table view in the Journal scene to an outlet in the JournalListViewController class, Ctrl + drag from the table view to the JournalListViewController file, just below the class name declaration:

![](img/0972ba626734a3986e095c788777818e_204_0.png)

Figure 11.14: Editor area showing drag destination

> You can also drag from the table view in the document outline.

10. A small pop-up dialog box will appear. Type the name of the outlet, tableView, into the Name text field, set Storage to Strong, and click Connect:

```
5 //  Created by
    iOS26Programming on
    07/08/2025.
6 //
7
8 import UIKit
9
10 class
    JournalListViewController:
    UIViewController {
11
12 override func
    viewDidLoad() {
13 super.viewDidLoad()
    // Do any additional
        setup after
        loading the view.
15 }
16
```

![](img/0972ba626734a3986e095c788777818e_205_0.png)

Figure 11.15: Pop-up dialog box for outlet creation

11. Verify that the tableView outlet declaration has been added to the JournalListViewController class. After you have done so, click the X button to close the assistant editor window:

![](img/0972ba626734a3986e095c788777818e_205_1.png)

Figure 11.16: Editor area showing the tableView outlet

The JournalListViewController class now has an outlet, tableView, for the table view in the Journal List screen. This means a JournalListViewController instance can manage what the table view displays.
