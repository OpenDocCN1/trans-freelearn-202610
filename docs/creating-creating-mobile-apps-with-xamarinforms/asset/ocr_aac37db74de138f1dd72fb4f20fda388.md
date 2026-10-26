Although this is certainly one of the simplest `IValueConverter` implementations imaginable, they often aren’t very elaborate. In `Convert`, the `value` argument is the value from the source, and the return value goes to the target. You can customize the method by checking the `targetType` argument, which is the type of the target property. You can also pass a `parameter` from the `Binding` constructor or `Binding.Create` method. You’ll see more examples in future chapters.

The `SetSlidersAndBindings` method instantiates `DoubleToIntConverter` when it’s required for the bindings. In a real program, you only need one instance of `DoubleToIntConverter` for the lifetime of the program, and you can instantiate it as a field.

```
class ColorScrollPage : ContentPage
{
    …

    void SetSlidersAndBindings()
    {
        // Don't handle ValueChange events from Sliders!
        ignoreSliderValueChanges = true;

        double sliderMax = 1;
        string[] format = null;
        IValueConverter valueConverter = null;

        // Set local variables for color mode.
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

        // Bind the labels to the sliders.
        for (int i = 0; i < 3; i++)
        {
            sliders[i].Maximum = sliderMax;
            labels[i].BindingContext = sliders[i];
            labels[i].SetBinding(Label.TextProperty,
                new Binding("Value", BindingMode.OneWay, valueConverter, null, format[i]));
        }

        // Now set the slider values.
        Color currentColor = this.currentColor;
```