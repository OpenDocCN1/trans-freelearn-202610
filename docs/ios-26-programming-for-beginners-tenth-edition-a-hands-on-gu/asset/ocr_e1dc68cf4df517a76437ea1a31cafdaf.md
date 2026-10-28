Apple provides an extensive library of user interface elements that you can use in your own apps. This helps to give all iOS apps a consistent look and feel. As you can see, the Add New Journal Entry screen has the following elements:

+   - A custom view showing star ratings
+   - A switch that allows you to get your current location
+   - A text field for the journal entry title
+   - A text view for the journal entry body
+   - An image view for a photo that you will take with your phone’s camera

You will now modify the screen to match the design shown in the app tour, beginning in the next section by adding a custom view that allows a user to set star ratings.

# Add a custom view to the New Entry scene

As you have seen in the app tour, the Add New Journal Entry screen has a custom view showing star ratings. This custom view is a subclass of a horizontal stack view. You’ll add the horizontal stack view in this chapter and complete the implementation of the custom view in Chapter 19, Custom Views.

A stack view is an instance of the UIStackView class. It allows you to easily lay out a collection of views either in a column or a row. To add a stack view to the Add New Journal Entry screen, follow these steps:

+   1. In the Main storyboard file, click New Entry Scene in the document outline:

![](bbox=[0.19, 0.605, 0.865, 0.805])

Figure 13.17: Editor area showing New Entry Scene