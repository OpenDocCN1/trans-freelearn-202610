+   - LayoutOptions.End
+   - LayoutOptions.Fill

The default—established by the View class—is LayoutOptions.Fill, which means that the view fills its container.

As you've seen, a VerticalOptions setting on a Label doesn't make a difference when the Label is a child of a vertical StackLayout. The StackLayout itself constrains the height of its children to only the height they require, so the child has no freedom to move vertically within that slot.

Be prepared for this rule to be slightly amended!

The LayoutOptions structure has four additional static fields not discussed yet:

+   - LayoutOptions.StartAndExpand
+   - LayoutOptions.CenterAndExpand
+   - LayoutOptions.EndAndExpand
+   - LayoutOptions.FillAndExpand

As you'll recall, LayoutOptions has two instance properties named Alignment and Expands. These four instances of LayoutOptions all have the Expands property set to true. This Expands property can be very useful for managing the layout of the page, but it can be confusing on first encounter. Here are the requirements for Expands to play a role in a vertical StackLayout:

+   - The vertical StackLayout must have a height that is less than the height of its container. In other words, some extra unused vertical space must exist in the StackLayout.
+   - That first requirement implies that the vertical StackLayout cannot have a VerticalOptions setting of Start, Center, or End because that would cause the StackLayout to have a height equal to the height of its children and it would have no extra space.
+   - At least one child of the StackLayout must have a VerticalOptions setting with the Expands property set to true.

If these conditions are satisfied, the StackLayout allocates the extra vertical space equally among all the children that have a VerticalOptions setting with Expands equal to true. How the child occupies that space depends on the Alignment setting: Start, Center, End, or Fill.

Here's a program named VerticalOptionsDemo that uses reflection to create Label objects with all the possible VerticalOptions settings in a vertical StackLayout. The background and foreground colors are alternated so that you can see exactly how much space each Label occupies. The program uses Language Integrated Query (LINQ) to sort the fields of the LayoutOptions structure in a visually more illuminating manner:

```
class VerticalOptionsDemoPage : ContentPage
{
```