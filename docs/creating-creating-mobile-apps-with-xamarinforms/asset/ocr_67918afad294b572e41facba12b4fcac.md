```
VerticalOptions = LayoutOptions.Center
},
new Label
{
    Text = String.Format("{0:X2}-{1:X2}-{2:X2}",
        (int)(255 * color.R),
        (int)(255 * color.G),
        (int)(255 * color.B)),
    VerticalOptions = LayoutOptions.Center,
    IsVisible = color != Color.Default
}
}
}
};
}
}
```

The `CreateColorView` method returns a `Frame` containing a horizontal `StackLayout` with a `BoxView` indicating the color, and two `Label` views for the name of the color and its RGB composition. This RGB display is meaningless for the `Color.Default` value, so the second `Label` has its `IsVisible` property set to `false` in that case. The `Label` still exists, but it’s treated as non-existent when the page is rendered.

Now this is a scrollable color list that’s beginning to be something we can take a little pride in:

![](bbox=[0.133, 0.477, 0.848, 0.812])