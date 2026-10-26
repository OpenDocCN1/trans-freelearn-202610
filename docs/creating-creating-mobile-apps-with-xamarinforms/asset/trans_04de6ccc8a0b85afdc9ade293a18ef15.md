

controllersStack.Children.Add(radioGrid);

// 为三个颜色分量创建标签和滑块。
for (int component = 0; component < 3; component++)
{
    labels[component] = new Label
    {
        XAlign = TextAlignment.Center
    };
    controllersStack.Children.Add(labels[component]);

    // 为所有滑块设置相同的 ValueChanged 处理程序。
    sliders[component] = new Slider();
    sliders[component].ValueChanged += OnSliderValueChanged;
    controllersStack.Children.Add(sliders[component]);
}

// 构建页面。
this.Padding = new Thickness(0, Device.OnPlatform(20, 0, 0), 0, 0);
this.Content = mainGrid;

// 设置 SizeChanged 处理程序。
SizeChanged += OnPageSizeChanged;

// 初始化滑块值。
SetSlidersAndBindings();
}
...
}

页面的 SizeChanged 处理程序负责根据宽高比调整 mainGrid 的行和列定义，并将 BoxView 和包含控制面板的 StackLayout 放置在适当的单元格中。尽管其中一些设置无论方向如何都相同——并且列和行默认为零——但将它们全部定义和重新定义在一个地方可以使意图更清晰：

```
class ColorScrollPage : ContentPage
{
    ...
    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // 竖屏模式。
        if (this.Height > this.Width)
        {
            // 调整列和行定义。
            mainGrid.ColumnDefinitions[0].Width = new GridLength(1, GridUnitType.Star);
            mainGrid.ColumnDefinitions[1].Width = new GridLength(0);
            mainGrid.RowDefinitions[0].Height = new GridLength(1, GridUnitType.Star);
            mainGrid.RowDefinitions[1].Height = GridLength.Auto;

```
```
// 设置 BoxView 和 StackLayout 的行和列。
Grid.SetColumn(boxView, 0);
Grid.SetRow(boxView, 0);

Grid.SetColumn(controllersStack, 0);
Grid.SetRow(controllersStack, 1);
}
// 横屏模式。
else
{
    // 调整列和行定义。
    mainGrid.ColumnDefinitions[0].Width = new GridLength(1, GridUnitType.Star);
    mainGrid.ColumnDefinitions[1].Width = new GridLength(1, GridUnitType.Star);
    mainGrid.RowDefinitions[0].Height = new GridLength(1, GridUnitType.Star);
    mainGrid.RowDefinitions[1].Height = new GridLength(0);

    // 设置 BoxView 和 StackLayout 的行和列。
    Grid.SetColumn(boxView, 0);
    Grid.SetRow(boxView, 0);

    Grid.SetColumn(controllersStack, 1);
    Grid.SetRow(controllersStack, 0);
}
}

...

}
```
```

充当单选按钮的三个 `Label` 视图的事件处理程序有两个任务：设置 `Label` 的 `Opacity` 以指示哪个被选中，以及设置新的颜色模式。然而，实际对滑块和标签进行所有更改的工作被委派给一个单独的方法：

```
```csharp
class ColorScrollPage : ContentPage
{
    ...

    void OnRadioLabelToggled(View view)
    {
        // 设置 Opacity 以指示切换状态。
        view.Opacity = view.GetRadioState() ? 1.0 : 0.5;

        // 可能更改当前颜色模式。
        if (view.GetRadioState())
        {
            ColorMode newColorMode =
                (ColorMode) Enum.Parse(typeof(ColorMode), view.StyleId);

            if (currentColorMode != newColorMode)
            {
                currentColorMode = newColorMode;
```
```
```
SetSlidersAndBindings ();
    }
  }
}
...
}
```

`SetSlidersAndBindings` 方法负责根据新的颜色模式设置所有 `Slider` 属性。例如，对于十六进制 RGB 模式，每个 `Slider` 的 Maximum 属性必须设置为 255。根据颜色模式，`Slider` 值本身必须基于当前颜色的 R、G、B 或 Hue、Saturation 和 Luminosity 属性来设置。

`SetSlidersAndBindings` 还负责从三个 `Slider` 视图的 `Value` 属性设置到标识这些滑块的三个 `Label` 视图的绑定。这些绑定使用定义为字段数组的格式字符串组。对于浮点 RGB 或 HSL 模式，格式规范为“F2”，对于十六进制 RGB 模式，格式规范为“X2。”

这就是问题所在。`Slider` 的 `Value` 属性的类型为 `double`，如果你尝试用“X2”格式化一个 `double`，将会引发异常。

在代码中，从 `double` 转换为 `int` 只是一个简单的强制转换。但在定义数据绑定时，你无法直接访问从源到目标传递的数据。那么该如何实现呢？

幸运的是，`Binding` 构造函数和 `Binding.Create` 方法都有一个类型为 `IValueConverter` 的参数，专门用于此目的。`IValueConverter` 是一个定义了 `Convert` 和 `ConvertBack` 两个方法的接口。你提供一个实现此接口的类来执行所需的转换。`Convert` 方法在属性值从源传递到目标时被调用。`ConvertBack` 方法仅在 `TwoWay` 或 `OneWayToSource` 绑定中起作用，用于将目标类型转换为源类型。

以下是将 double 转换为 int 的 `IValueConverter` 实现：

```
public class DoubleToIntegerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
                          object parameter, CultureInfo culture)
    {
        return (int)(double)value;
    }

    public object ConvertBack(object value, Type targetType,
                              object parameter, CultureInfo culture)
    {
        return (double)(int)value;
    }
}
```

尽管这无疑是最简单的 `IValueConverter` 实现之一，但它们通常并不复杂。在 `Convert` 中，`value` 参数是源的值，返回值传递给目标。你可以通过检查 `targetType` 参数来自定义该方法，该参数是目标属性的类型。你还可以从 `Binding` 构造函数或 `Binding.Create` 方法传递一个 `parameter`。你将在后续章节中看到更多示例。

`SetSlidersAndBindings` 方法在需要为绑定时实例化 `DoubleToIntConverter`。在实际程序中，程序生命周期内只需要一个 `DoubleToIntConverter` 实例，你可以将其作为字段实例化。

```
class ColorScrollPage : ContentPage
{
    …

    void SetSlidersAndBindings()
    {
        // 不要处理滑块的 ValueChange 事件！
        ignoreSliderValueChanges = true;

        double sliderMax = 1;
        string[] format = null;
        IValueConverter valueConverter = null;

        // 为颜色模式设置局部变量。
        switch (currentColorMode)
        {
            case ColorMode.RgbHex:
                sliderMax = 255;
                format = rgbHexFormat;
                valueConverter = new DoubleToIntegerConverter();
                break;

            case ColorMode.RgbFloat:
                format = rgbFloatFormat;
                break;

            case ColorMode.Hsl:
                format = hslFormat;
                break;
        }

        // 将标签绑定到滑块。
        for (int i = 0; i < 3; i++)
        {
            sliders[i].Maximum = sliderMax;
            labels[i].BindingContext = sliders[i];
            labels[i].SetBinding(Label.TextProperty,
                new Binding("Value", BindingMode.OneWay, valueConverter, null, format[i]));
        }

        // 现在设置滑块值。
        Color currentColor = this.currentColor;
```

## Free ebooks

从技术概述到特定主题的深入分析，从 Microsoft Press 获取 *免费* 电子书：

**www.microsoftvirtualacademy.com/ebooks**

以 PDF、EPUB 和/或 Kindle 的 Mobi 格式下载你的免费电子书。

在 Microsoft Virtual Academy 寻找其他优秀资源，在那里你可以学习新技能并借助专家提供的免费 Microsoft 培训推动职业发展。

