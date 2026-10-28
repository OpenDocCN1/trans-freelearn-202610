

8. 点击第三个 Table View Cell，并从库中拖入一个 Label 对象到其中。在 Attributes 检查器中，在 Label 下，将 Style 设置为 Semibold，Alignment 设置为左对齐。

9. 点击 Add New Constraints 按钮，并将 Label 的 top、left 和 right 约束设置为 0。确保 Constrain to Margins 已勾选，然后点击 Add 3 Constraints 按钮。标签现在看起来如下所示：

![](img/0972ba626734a3986e095c788777818e_282_0.png)

Figure 13.50: Label with the constraints applied

10. 点击第四个 Table View Cell，并从库中拖入一个 Text View 对象到其中。在 Attributes 检查器中，在 Text View 下，取消选中 Editable 和 Selectable，因为在 Journal Entry Detail 屏幕中你不需要编辑或选择文本。你也可以根据需要更改默认文本。

11. 点击 Add New Constraints 按钮，并将 top、left、right 和 bottom 约束设置为 0。确保 Constrain to Margins 已勾选，然后点击 Add 4 Constraints 按钮。文本视图现在看起来如下所示：

![](img/0972ba626734a3986e095c788777818e_282_1.png)

Figure 13.51: Text View with the constraints applied

12. 点击第五个 Table View Cell，并从库中拖入一个 Image View 对象到其中。在 Attributes 检查器中，在 Image View 下，将 Image 设置为 face.smiling。

13. 在 Size 检查器中，在 View 下，将 Width 和 Height 设置为 300。

14. 点击 Add New Constraints 按钮并设置 Width 和 Height 约束（其值应已存在）。点击 Add 2 Constraints 按钮。

15. 点击 Align 按钮，并勾选 Horizontally in Container 和 Vertically in Container。点击 Add 2 Constraints 按钮。图像视图现在看起来如下所示：

![](img/0972ba626734a3986e095c788777818e_283_0.png)

Figure 13.52: Image View object with constraints applied

16. 点击第六个 Table View Cell，并从库中拖入一个 Image View 对象到其中。在 Attributes 检查器中，在 Image View 下，将 Image 设置为 map。

17. 对此图像视图重复 *steps 13 to 15*。它现在看起来如下所示：

![](img/0972ba626734a3986e095c788777818e_284_0.png)

Figure 13.53: Image View object with constraints applied

18. 所有必需的用户界面元素都已添加到 Entry Detail 场景中。构建并运行你的应用，并在 Journal List 屏幕上点击一行以导航到 Journal Entry Detail 屏幕：

19. 向下滚动以查看 Journal Entry Detail 屏幕的剩余部分。

![](img/0972ba626734a3986e095c788777818e_286_0.png)

Figure 13.55: Simulator showing the remainder of the Journal Entry Detail screen

Excellent! You have modified all the screens for your app, and you’re ready to add functionality to them in the next part of this book.

## Summary

In this chapter, you modified the Journal List, Add New Journal Entry, and Journal Entry Detail screens to match the design shown in the app tour. For the Journal List screen, you modified the journalCell table view cell by adding an image view and two labels to it. You modified the Add New Journal Entry screen by adding a custom view, a switch, a text field, a text view, and an image view to it. You also configured the image view to show a default image. For the Journal Entry Detail screen, you added a text view, labels, and image views to it, configuring the image views to show default images.

You now have more experience in how to use Interface Builder to add and configure multiple user interface elements, set their sizes and positions using the Size inspector, and apply the necessary constraints using the Add New Constraints and Align buttons. This will be useful to ensure compatibility with different screen sizes and orientations. You should also be able to easily prototype the appearance and flow of your apps.

You’re now finished with the storyboard and design setup. You can go through every screen that your app has and see what they look like, even though none of the screens have actual data in them. If this app were a house being built, it’s as though you’ve built all the walls and floors, and the house is now ready to have the interior done. Great job!

This concludes Part 2 of this book. In the next part, you’ll begin to type in all the code required for your app to work. In the next chapter, you’ll start by learning more about the Model-View-Controller design pattern. You’ll also learn how table views work, which is crucial for understanding how the Journal List screen works.

## Get This Book’s PDF Version and Exclusive Extras

Scan the QR code (or go to packtpub.com/unlock). Search for this book by name, confirm the edition, and then follow the steps on the page.

Note: Keep your invoice handy. Purchases made directly from Packt don’t require an invoice.

![](img/0972ba626734a3986e095c788777818e_287_0.png)

## Part 3
### Code

Welcome to Part 3 of this book. With your user interface complete, you will now add code to implement your app’s functionality. To display your data in a list, you will use a table view, and you will learn how to use an array as a data source. Next, you’ll learn how to pass data from a view controller to the array used as a data source, and pass data from one view controller to another. You will also look at how to determine the device location and how to display a map containing annotations. After that, you will learn how to persist app data using JSON files. Then, you’ll learn how to create custom views, use the device camera and photo library, and add search capability to your table view. Finally, you’ll replace the table view with a collection view to make your app suitable for larger screens, such as a Mac or iPad.

This part comprises the following chapters:

- Chapter 14, MVC and Table Views
- Chapter 15, Get Data into Table Views
- Chapter 16, Pass Data Between View Controllers
- Chapter 17, Core Location and MapKit
- Chapter 18, JSON Files
- Chapter 19, Custom Views
- Chapter 20, Camera and Photo Library
- Chapter 21, Search
- Chapter 22, Collection Views

By the end of this part, you’ll have completed the JRNL app. You’ll have the experience of building a complete app from scratch, which will be useful as you build your own apps. Let’s get started!

## 14 MVC and Table Views

In the previous chapter, you modified the Journal List screen, the Add New Journal Entry screen, and the Journal Entry Detail screen to match the app tour shown in *Chapter 10, Set Up the User Interface*. You have completed the initial UI for the JRNL app, and that concluded Part 2 of this book.

This chapter begins Part 3 of this book, where you will focus on the code that makes your app work. In this chapter, you will learn about the Model-View-Controller (MVC) design pattern and how the different parts of an app interact with one another. Then, you’ll implement a table view programmatically (which means implementing it using code instead of storyboards) using a playground, to understand how table views work. Finally, you’ll revisit the table view you implemented on the Journal List screen, so you can see the differences between implementing it in a storyboard and implementing it programmatically.

By the end of this chapter, you’ll understand the MVC design pattern, you’ll have learned how to create a table view controller programmatically, and you’ll know how to use table view delegate and data source protocols.

The following topics will be covered in this chapter:

- The MVC design pattern
- Table views
- Revisit the Journal List screen

### Technical requirements

The completed Xcode playground and project for this chapter are in the Chapter14 folder of the code bundle for this book, which can be downloaded here:

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

Check out the following video to see the code in action:

https://youtu.be/SHqmm6Ch57E

Create a new playground and call it TableViewBasics. You can use this playground to type in and run all the code in this chapter as you go along. Before you do, let’s look at the MVC design pattern, an approach commonly used when writing iOS apps.

### The MVC design pattern

The Model-View-Controller (MVC) design pattern is a common approach used when building iOS apps. MVC divides an app into three different parts:

- **Model:** This handles data storage and representation and data processing tasks
- **View:** This includes all the things that are on the screen that the user can interact with
- **Controller:** This manages the flow of information between the model and the view

One notable feature of MVC is that the view and model do not interact with one another; instead, all communication is managed by the controller.

For example, imagine you’re at a restaurant. You look at a menu and choose something you want. Then, a waiter comes, takes your order, and sends it to the cook. The cook prepares your order, and when it is done, the waiter takes the order and brings it out to you. In this scenario, the menu is the view, the waiter is the controller, and the cook is the model. Also, note that all interactions between you and the kitchen are only through the waiter; there is no interaction between you and the cook.

To find out more about MVC, visit https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html.

