```
SetSlidersAndBindings ();
    }
  }
}
...
}
```

The `SetSlidersAndBindings` method has the responsibility of setting all the `Slider` properties in accordance with the new color mode. For example, for the hexadecimal RGB mode, the Maximum property of each `Slider` must be set to 255. Based on the color mode, the `Slider` values themselves must be set based on the R, G, and B, or the Hue, Saturation, and Luminosity properties of the current color.

`SetSlidersAndBindings` is also responsible for setting bindings from the `Value` properties of the three `Slider` views to target the three `Label` views that identify these sliders. These bindings use the groups of formatting strings defined as field arrays. For the floating point RGB or HSL mode, the formatting specification is “F2”, and for the hexadecimal RGB mode, the formatting specification is “X2.”

And that is a problem. The `Value` property of the `Slider` is of type `double`, and if you attempt to format a `double` with “X2”, an exception will be raised.

In code, converting from a `double` to an `int` is a trivial cast. But when defining a data binding, you don’t have direct access to the data passing from the source to the target. So how can this be done?

Fortunately, the `Binding` constructor and the `Binding.Create` method have an argument of type `IValueConverter` specifically for this purpose. `IValueConverter` is an interface that defines two methods named `Convert` and `ConvertBack`. You supply a class that implements this interface to perform the conversion you need. The `Convert` method is called when the property value passes from the source to the target. The `ConvertBack` method only plays a role in `TwoWay` or `OneWayToSource` bindings to convert the target type to the source type.

Here is an implementation of `IValueConverter` to convert from double to int:

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