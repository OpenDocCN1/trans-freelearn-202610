The `Entry` and `Editor` define `Focused` and `Unfocused` events that signal gaining and losing input focus. The `IsFocused` property indicates if a particular view currently has input focus. A program can attempt to set focus to a particular view in code by calling the `Focus` method on the view. The method returns `true` if the focus change was successful. These focus-related members are defined by `Visual-Element`, the base class for all UI elements, but they are really only relevant for the `Entry` and `Editor`.

Both `Entry` and `Editor` define `Text` properties that expose the current text displayed by the view. A program can initialize this `Text` property and then allow the user to edit that text.

Both `Entry` and `Editor` define a `Completed` event that is fired when the user has signaled that editing is completed. This is an excellent opportunity for programs to access the `Text` property and save the results.

Both `Entry` and `Editor` also define a `TextChanged` event that is fired when the `Text` property changes. (Keep in mind that .NET `string` objects are immutable. A `string` object can’t itself change after it’s been created. Every time the contents of the `Entry` or `Editor` change, it’s a whole new `string` object rather than a modified `string` object.) The `TextChanged` event can be a valuable means for the application to monitor the text input on a character-by-character basis and respond to changes before the user signals that the typing is complete.

You might assume that `Entry` is somewhat simpler than `Editor` because it handles only a single line of text. However, `Entry` has three additional properties that `Editor` doesn’t have:

+   - `TextColor` — a `Color` value
+   - `IsPassword` — a Boolean that causes characters to be masked
+   - `Placeholder` — light colored text that appears in the `Entry` but disappears as soon as the user begins typing.

The **NoteTaker** program requires a page that contains an `Entry` for the user to type a title for the note, and an `Editor` for typing the note itself. For the first version of this program—called **NoteTaker1**—let’s not do much more beyond getting these two views on the page with a couple of `Label` views for identification.

The `Editor` allows an indefinite amount of text to be entered and internally implements scrolling. As you discovered with the `ScrollView` in the previous chapter, sharing a scrollable view with other views on the page requires that the `VerticalOptions` property be set to `LayoutOptions.FillAndExpand`.

Here’s the `NoteTaker1Page` class:

```
class NoteTaker1Page : ContentPage
{
    public NoteTaker1Page()
    {
        this.Padding = new Thickness(10, Device.OnPlatform(20, 0, 0), 10, 0);
```