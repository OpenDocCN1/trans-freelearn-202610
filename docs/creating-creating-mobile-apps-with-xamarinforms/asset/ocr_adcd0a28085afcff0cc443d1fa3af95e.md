Attached bindable properties are a general-purpose mechanism for properties defined by one class to be stored in instances of another class, but attached bindable properties are used mostly with layout classes. As you’ll see in the next chapter, `Grid` defines attached bindable properties to specify the row and column of each child.

It can be difficult for a program to determine a good width and height to assign to a child of an `AbsoluteLayout`, but often you don’t need to. If you specify the static read-only property `AbsoluteLayout.AutoSize` for the width and height arguments in the `Rectangle` you pass to `AbsoluteLayout.SetLayoutBounds`, the child will be sized based on a width and height it calculates for itself.

`AbsoluteLayout` redefines its `Children` property to provide a couple of shortcuts. You can specify an initial position and size when you add a child view to the `AbsoluteLayout`:

```
absoluteLayout.Children.Add(child, new Rectangle(x, y, width, height));
```

Or, if you want the view automatically sized:

```
absoluteLayout.Children.Add(child, new Point(x, y));
```

However, if you need to change the position and size after the child has been added to the `Children` collection, you need to use the static `AbsoluteLayout.SetLayoutBounds` method.

As you can surmise, using `AbsoluteLayout` is more difficult than using `StackLayout`. In general it’s much easier to let Xamarin.Forms and the other `Layout` classes handle much of the complexity of layout for you. But for some special applications, `AbsoluteLayout` is ideal.

One such special application is **DotMatrixClock**, which displays the digits of the current time using a simulated 5 × 7 dot matrix display. Each dot is a `BoxView`, individually sized and positioned on the screen and colored either red or light gray depending on whether the dot is on or off. Conceivably, the dots of this clock could be organized in nested `StackLayout` elements or a `Grid` (covered in the next chapter) but each `BoxView` needs to be given a size anyway. The sheer quantity and regularity of these views suggests that the programmer knows better how to arrange them on the screen than a layout that needs to perform the location calculations in a more generalized manner.

The `numberPatterns` and `colonPatterns` fields define the dot matrix patterns for the 10 digits and a colon separator:

```
class DotMatrixClockPage : ContentPage
{
    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0 }, { 1, 0, 0, 0, 1 }, { 1, 0, 0, 1, 1 }, { 1, 0, 1, 0, 1 },
            { 1, 1, 0, 0, 1 }, { 1, 0, 0, 0, 1 }, { 0, 1, 1, 1, 0 }
        },
        {
            { 0, 0, 1, 0, 0 }, { 0, 1, 1, 0, 0 }, { 0, 0, 1, 0, 0 }, { 0, 0, 1, 0, 0 },
```