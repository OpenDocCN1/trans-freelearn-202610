

```csharp
};

// Get access to the text resource.
Assembly assembly = this.GetType().GetTypeInfo().Assembly;
string resource = "BlackCat.Texts.TheBlackCat.txt";

using (Stream stream = assembly.GetManifestResourceStream (resource))
{
    using (StreamReader reader = new StreamReader (stream))
    {
        bool gotTitle = false;
        string line;

        // Read in a line (which is actually a paragraph).
        while (null != (line = reader.ReadLine()))
        {
            Label label = new Label
            {
                Text = line,

                // Black text for ebooks!
                TextColor = Color.Black
            };

            if (!gotTitle)
            {
                // Add first label (the title) to mainStack.
                label.HorizontalOptions = LayoutOptions.Center;
                label.Font = Font.SystemFontOfSize(NamedSize.Medium,
                    FontAttributes.Bold);

                mainStack.Children.Add(label);
                gotTitle = true;
            }
            else
            {
                // Add subsequent labels to textStack.
                textStack.Children.Add(label);
            }
        }
    }
}

// Put the textStack in a ScrollView with FillAndExpand.
ScrollView scrollView = new ScrollView
{
    Content = textStack,
    VerticalOptions = LayoutOptions.FillAndExpand,
    Padding = new Thickness(5, 0)
};

// Add the ScrollView as a second child of mainStack.
mainStack.Children.Add(scrollView);

// Set page content to mainStack.
```

```csharp
Children =
{
    button,
    new ScrollView
    {
        VerticalOptions = LayoutOptions.FillAndExpand,
        Content = loggerLayout
    }
};
}

void OnButtonClicked(object sender, EventArgs args)
{
    // Add Label to scrollable StackLayout.
    loggerLayout.Children.Add(new Label
    {
        Text = "Button clicked at " + DateTime.Now.ToString("T")
    });
}
```

本书中的程序中，事件处理程序的名称以单词 On 开头，其后是触发事件的视图的某种标识，有时只是视图类型，然后是事件名称。在本例中得到的名称是 OnButtonClicked。

Clicked 处理程序在 Button 创建后立即附加到 Button：

```csharp
button.Clicked += OnButtonClicked;
```

然后组装页面。请注意，ScrollView 的 VerticalOptions 设置为 FillAndExpand，这样它就可以与 Button 共享 StackLayout，同时仍然可见且可滚动。

以下是几次点击后的显示：

```csharp
addButton = new Button
{
    Text = "Add",
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
addButton.Clicked += OnButtonClicked;

removeButton = new Button
{
    Text = "Remove",
    HorizontalOptions = LayoutOptions.CenterAndExpand,
    IsEnabled = false
};
removeButton.Clicked += OnButtonClicked;

this.Padding = new Thickness(5, Device.OnPlatform(20, 0, 0), 5, 0);

// Assemble the page.
this.Content = new StackLayout
{
    Children =
    {
        new StackLayout
        {
            Orientation = StackOrientation.Horizontal,
            Children =
            {
                addButton,
                removeButton
            }
        },

        new ScrollView
        {
            VerticalOptions = LayoutOptions.FillAndExpand,
            Content = loggerLayout
        }
    }
};
}

void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;

    if (button == addButton)
    {
        // Add Label to scrollable StackLayout.
        loggerLayout.Children.Add(new Label
        {
            Text = "Button clicked at " + DateTime.Now.ToString("T")
        });
    }
    else
    {
        // Remove topmost Label from StackLayout
        loggerLayout.Children.RemoveAt(0);
    }
    // Enable "Remove" button only if children are present.
    removeButton.IsEnabled = loggerLayout.Children.Count > 0;
}
```

两个按钮都设置了 HorizontalOptions 的值为 CenterAndExpand，这样它们就可以使用水平 StackLayout 在屏幕上很好地排列。

请注意，当 Clicked 处理程序检测到 removeButton 时，它只是对 Children 属性调用 RemoveAt 方法。如果没有子项会发生什么？

在该程序中不可能发生这种情况！应用程序开始时，removeButton 的 IsEnabled 属性被初始化为 false。当按钮以这种方式被禁用时，它看起来不可用，实际上也是不可用的。它不会触发 Clicked 事件。在 Clicked 处理程序的末尾，removeButton 的 IsEnabled 属性仅在 loggerLayout 至少有一个子项时才设置为 true。

这说明了一个很好的通用规则：如果你的代码需要确定按钮 Clicked 事件是否有效，那么最好通过将 IsEnabled 设置为 false 来完全防止无效的按钮点击。

## 匿名事件处理程序

如今许多 C# 程序员喜欢将小型事件处理程序定义为匿名 lambda 函数。这使得事件处理代码可以非常接近触发事件的对象的实例化和初始化，而不是文件中的其他位置。它还允许在事件处理程序中引用对象，而无需将这些对象存储为字段。

下面是一个名为 ButtonLambdas 的程序，它有一个显示数字的 Label 和两个按钮。一个按钮将数字加倍，另一个按钮将数字减半。通常数字和标签变量会保存为字段。但由于匿名事件处理程序是在构造函数中在这两个变量定义之后立即定义的，因此事件处理程序可以访问它们：

```csharp
class ButtonLambdasPage : ContentPage
{
    public ButtonLambdasPage()
    {
        // Number to manipulate.
        double number = 1;

        // Create the Label for display.
        Label label = new Label
        {
            Text = number.ToString(),
            Font = Font.SystemFontOfSize(NamedSize.Large),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Create the first Button and attach Clicked handler.
        Button timesButton = new Button
        {
            Text = "Double",
            Font = Font.SystemFontOfSize(NamedSize.Large),
            HorizontalOptions = LayoutOptions.CenterAndExpand
        };
        timesButton.Clicked += (sender, args) =>
        {
            number *= 2;
            label.Text = number.ToString();
        };

        // Create the second Button and attach Clicked handler.
        Button divideButton = new Button
        {
            Text = "Half",
            Font = Font.SystemFontOfSize(NamedSize.Large),
            HorizontalOptions = LayoutOptions.CenterAndExpand
        };
        divideButton.Clicked += (sender, args) =>
        {
            number /= 2;
            label.Text = number.ToString();
        };

        // Assemble the page.
        this.Content = new StackLayout
        {
            Children =
            {
                label,
                new StackLayout
                {
                    Orientation = StackOrientation.Horizontal,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    Children =
                    {
                        timesButton,
                        divideButton
                    }
                }
            }
        };
    }
}
```

与前一个程序一样，两个按钮共享一个水平 `StackLayout`：

![](img/c2d61d0459c51981af78607f1eef3d1c_85_0.png)

将事件处理程序定义为匿名 lambda 函数的缺点是它们不能在多个视图之间共享。

## 使用 ID 区分视图

在 **TwoButtons** 程序中，你看到了一种通过比较对象来区分视图并共享事件处理程序的技术。当需要区分的视图不是很多时，这种方法效果很好，但对于计算器程序来说将是糟糕的做法。

`Element` 类专门定义了类型为 `string` 的 `StyleId`，用于标识视图。将其设置为应用程序方便的值即可。你可以使用 `if` 和 `else` 语句、`switch` 和 `case` 来测试这些值，或者使用 `Parse` 方法将字符串转换为数字或枚举成员。

下面的程序不是计算器，但它是一个数字键盘，这肯定是计算器的一部分。该程序名为 **SimplestKeypad**，使用 `StackLayout` 来组织键的行和列。 （该程序的一个目的就是演示 `StackLayout` 并不是这项工作的合适工具！）

该程序创建了总共四个 `StackLayout` 实例。`mainStack` 为垂直方向；三个水平 `StackLayout` 对象排列九个非零数字按钮。为了简单起见，键盘采用电话排序而不是计算器排序：

```csharp
class SimplestKeypadPage : ContentPage
{
    Label displayLabel;
    Button backspaceButton;
```

