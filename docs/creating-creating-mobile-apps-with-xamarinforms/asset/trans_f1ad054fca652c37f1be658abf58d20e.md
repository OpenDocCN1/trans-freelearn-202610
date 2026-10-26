

```csharp
BindableProperty.Create<RadioButton, Font>
    (radio => radio.Font,                 // property
        Font.SystemFontOfSize(NamedSize.Large));   // initial value

public Font Font
{
    set { SetValue(FontProperty, value); }
    get { return (Font)GetValue(FontProperty); }
}
…
}
```

Create 方法使用反射来获取 Font 属性名称的字符串表示。Font 属性被初始化为 Large 大小，因为 Medium 大小无法提供足够的触控目标。

现在 RadioButton 具有 Text 和 Font 属性，但这些属性的值必须设置在 RadioButton 在其构造函数中创建的两个 Label 视图上。这可以通过为刚刚定义的 Text 和 Font 属性设置属性更改处理程序来实现，但数据绑定提供了一种更简单的方法。在以下在 RadioButton 构造函数中定义的数据绑定中，目标是 Label 视图，源是 RadioButton 本身：

```csharp
class RadioButton : ContentView
{
    …
    public RadioButton()
    {
        …
        // Set the bindings.
        checkLabel.BindingContext = this;
        checkLabel.SetBinding(Label.FontProperty, "Font");

        textLabel.BindingContext = this;
        textLabel.SetBinding(Label.TextProperty, "Text");
        textLabel.SetBinding(Label.FontProperty, "Font");
        …
    }
    …
}
```

RadioButton 还需要一个 Toggled 事件和一个由 IsToggledProperty 可绑定属性支持的 IsToggled 属性。此 BindableProperty 对象的定义包含对属性更改时调用的方法的引用：

```csharp
class RadioButton : ContentView
{
    …
    // Define the Toggled event, IsToggled bindable property and property.
    public event EventHandler<ToggledEventArgs> Toggled;

    public static readonly BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled",
            typeof(bool),      // property type
            typeof(RadioButton),  // this type
            false,                // initial value
            BindingMode.TwoWay,   // default binding
            null,                 // validation
            OnIsToggledPropertyChanged);

    public bool IsToggled
    {
        get { return (bool)GetValue(IsToggledProperty); }
        set { SetValue(IsToggledProperty, value); }
    }
    ...
}
```

在静态 BindableProperty.Create 方法中引用的 OnIsToggledPropertyChanged 方法会在 IsToggled 属性更改时被调用。然而，由于该方法在静态字段定义中被引用，因此该方法本身也必须是静态的。它的签名如下：

```csharp
static void OnIsToggledPropertyChanged(BindableObject sender,
    object oldValue, object newValue)
{
    ...
}
```

每当 RadioButton 实例的 IsToggled 属性更改时，就会调用此方法。然而，此方法时静态的！如果存在多个 RadioButton 实例（这是很正常的），此方法如何知道哪个 RadioButton 被切换了？

很简单：第一个参数是其 IsToggled 属性已更改的实际 RadioButton 实例，而第二个和第三个参数表示旧值和新值。这意味着你可以安全地将第一个参数强制转换为 RadioButton 类型对象：

```csharp
static void OnIsToggledPropertyChanged(BindableObject sender,
    object oldValue, object newValue)
{
    // Get the object whose property is changing.
    RadioButton radioButton = (RadioButton)sender;
    ...
}
```

然后你可以继续进行。

但通常很方便让此静态方法调用实例方法，并让实例方法完成所有工作。这就是 RadioButton 中所做的方式。此方法基本上必须执行三项工作：

- 设置适当的字符以指示选中或未选中状态。
- 触发 IsToggled 事件。
- 如果 IsToggled 为 true，则将所有 RadioButton 兄弟的 IsToggled 设置为 false。

以下是 IsToggled 属性更改处理程序的静态和实例版本：

```csharp
class RadioButton : ContentView
{
    // Property changed handler for the IsToggled property.
    static void OnIsToggledPropertyChanged(BindableObject sender,
                                            object oldValue, object newValue)
    {
        // Get the object whose property is changing.
        RadioButton radioButton = (RadioButton)sender;
        radioButton.OnIsToggledPropertyChanged((bool)oldValue, (bool)newValue);
    }

    void OnIsToggledPropertyChanged(bool oldValue, bool newValue)
    {
        this.checkLabel.Text = newValue ? checkOn : checkOff;

        // Fire the event.
        if (this.Toggled != null)
        {
            this.Toggled(this, new ToggledEventArgs(this.IsToggled));
        }

        // If toggled on, toggle off all siblings.
        if (this.IsToggled)
        {
            Layout<View> parent = this.ParentView as Layout<View>;

            if (parent != null)
            {
                foreach (View view in parent.Children)
                {
                    if (view is RadioButton && view != this)
                    {
                        ((RadioButton)view).IsToggled = false;
                    }
                }
            }
        }
    }
}
```

只剩下一件缺失的事：RadioButton 尚未响应触摸。每当 RadioButton 被轻触时，它必须将其 IsToggled 属性设置为 true。

Xamarin.Forms 视图可以通过 GestureRecognizer 对象获取触摸输入。对于轻触，你创建一个 TapGestureRecognizer 类型的对象，并提供一个在轻触发生时调用的回调方法。（这是目前唯一可用的 GestureRecognizer 类型。）然后将此 TapGestureRecognizer 添加到视图的 GestureRecognizers 集合中。

```csharp
};
radioButton.Toggled += OnRadioButtonToggled;
radioStack.Children.Add(radioButton);

radioButton = new RadioButton
{
    Text = "Blue",
    StyleId = "#0000FF"
};
radioButton.Toggled += OnRadioButtonToggled;
radioStack.Children.Add(radioButton);

// A BoxView to display the selected color.
boxView = new BoxView
{
    VerticalOptions = LayoutOptions.FillAndExpand
};

this.Padding = new Thickness(0, Device.OnPlatform(20, 0, 0), 0, 0);

// Build the page.
this.Content = new StackLayout
{
    Children =
    {
        new Frame
        {
            HorizontalOptions = LayoutOptions.Center,
            Content = radioStack
        },
        boxView
    }
};

// Initialize.
((RadioButton)radioStack.Children[0]).IsToggled = true;
}

void OnRadioButtonToggled(object sender, ToggledEventArgs args)
{
    if (args.Value)
    {
        // Set the color from the StyleId string.
        boxView.Color = Color.FromHex(((RadioButton)sender).StyleId);
    }
}
}
```

请注意，每个 RadioButton 的 StyleId 属性都被设置为颜色的十六进制字符串表示形式。OnRadioButtonToggled 事件处理程序只需将其传递给 Color.FromHex 方法以设置 BoxView 的新颜色。（这是一种相当不寻常的 StyleId 用法。如果想将 Color 值等信息与视图一起存储，最好从视图派生并添加一个简单类型的读写属性。）

```csharp
public static void SetRadioState(this View view, bool isToggled)
{
    // Check if the property is actually changing.
    if (instances[view].toggledState != isToggled)
    {
        // Set the new value.
        instances[view].toggledState = isToggled;

        // Fire the handler.
        instances[view].toggledHandler(view);

        // If being toggled, untoggle all the siblings.
        if (isToggled)
        {
            Layout<View> parent = view.ParentView as Layout<View>;

            if (parent != null)
            {
                foreach (View sibling in parent.Children)
                {
                    if (sibling != view && instances.ContainsKey(sibling))
                    {
                        sibling.SetRadioState(false);
                    }
                }
            }
        }
    }
}

public static bool GetRadioState(this View view)
{
    return instances[view].toggledState;
}
```

你可以在任何 View 派生类上调用 AttachRadioToggler 并提供一个更改处理程序。SetRadioState 和 GetRadioState 扩展方法是必要的，因为不存在扩展属性。

作为对 toggledHandler 方法的调用的响应，程序可以以任何想要的方式更改 View。例如，如果你使用 Label 作为项目，可以更改字体或不透明度。你将在即将到来的 ColorScroll 程序中看到一个示例。

## Mastering the Grid

Grid 是一种强大的布局机制，它将子元素组织到行和列的单元格中。起初 Grid 看起来很像 HTML 表格，但表格用于展示目的，而 Grid 仅用于布局。例如，Grid 中没有标题的概念。

