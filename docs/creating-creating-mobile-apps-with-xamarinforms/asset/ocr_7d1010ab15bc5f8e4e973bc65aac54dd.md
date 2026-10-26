```
{
    this.Content = new BoxView
    {
        Color = Color.Accent,
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    }
}
```

Now the BoxView will assume its default dimensions, which are 40 units square. The BoxView is that size because the BoxView initializes its WidthRequest and HeightRequest properties to 40.

These WidthRequest and HeightRequest properties require a little explanation:

VisualElement defines Width and Height properties, but these properties are read-only. VisualElement also defines WidthRequest and HeightRequest properties that are both settable and gettable. Normally, all these properties are initialized to -1 (which effectively means they are undefined), but some View derivatives, such as BoxView, set the WidthRequest and HeightRequest properties to specific values.

Following the layout of a page, the Width and Height properties indicate actual dimensions of the view—the area that the view occupies on the screen. Because Width and Height are read-only, they are for informational purposes only.

If you want a View to be a specific size you can set the WidthRequest and HeightRequest properties. But these properties indicate (as the named suggests) a requested size, or a preferred size. If the view is allowed to fill its container, these properties will be ignored. But if not, these properties will govern the view’s size.

BoxView sets its own WidthRequest and HeightRequest properties to 40. You can think of these settings as a size that BoxView would like to be if nobody else has any opinions in the matter. You’ve already seen that WidthRequest and HeightRequest are ignored when the BoxView is allowed to fill the page. The WidthRequest kicks in if the HorizontalOptions is set to LayoutOptions.Left, Center, or Right, or if the BoxView is a child of a horizontal StackLayout. The HeightRequest behaves similarly.

Try this:

```
class GreetingsPage : ContentPage
{
    public GreetingsPage()
    {
        this.Content = new BoxView
        {
            Color = Color.Accent,
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center,
            WidthRequest = 200,
            HeightRequest = 100
```