6. Click on the Main storyboard file in the Project navigator and click the Cancel button (which appears as X) in the New Entry scene. In the document outline, Ctrl + drag from the Cancel button to the scene exit icon and choose unwindNewEntryCancelWithSegue: from the pop-up menu:

![](bbox=[0.25, 0.19, 0.8, 0.5])

Figure 11.48: Document outline showing the Cancel button action being set

When your app is running, clicking the X button will remove the view controller from the navigation controller’s viewControllers array, dismiss the view that is presented modally, and execute the unwindNewEntryCancel(segue:) method. Note that this method doesn’t do anything at present.

7. Build and run your app, and click the + button in the navigation bar of the Journal List screen. The new view will appear. When you click the X button, the new view disappears:

![](bbox=[0.31, 0.7, 0.75, 0.86])

Figure 11.49: Simulator showing the X button