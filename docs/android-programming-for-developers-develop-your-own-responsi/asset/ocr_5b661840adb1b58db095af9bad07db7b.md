# # Creating a list item for ListView

Next, let's create a list item to be used within ListView. Follow the given steps for the same:

+   1. Right-click on the `layout` folder and navigate to **New | Layout resource file**. Enter `listitem` in the **Name** field and make the **Root element** field RelativeLayout.

+   2. Take a look at the next screenshot to see what we are trying to achieve:

![](bbox=[0.164, 0.287, 0.591, 0.349])

+   3. Drag three **ImageView** widgets onto the top-left corner of the layout.

+   4. It will make the rest of the layout easier to complete if we enter the src properties for each of these Imageview widgets. So, we might as well add id properties while we are doing so. Add the following properties from the table:

| Widget type | Property | Value to set to |
|---|---|---|
| ImageView (left) | id | imageViewImportant |
| ImageView (left) | src | @drawable/ic_warning_black_24dp |
| ImageView (center) | id | imageViewTodo |
| ImageView (center) | src | @drawable/ic_check_box_outline_blank_black_24dp |
| ImageView (right) | id | imageViewIdea |
| ImageView (right) | src | @drawable/ic_wb_incandescent_black_24dp |

+   5. Drag a **LargeText** widget immediately below **ImageViews**, again to the immediate left-hand side of the layout.

+   6. Now, drag a **Plain TextView** widget immediately below the LargeText