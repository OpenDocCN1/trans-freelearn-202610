

```csharp
public SimplestKeypadPage()
{
    // Create a vertical stack for the entire keypad.
    StackLayout mainStack = new StackLayout
    {
        VerticalOptions = LayoutOptions.Center,
        HorizontalOptions = LayoutOptions.Center
    };

    // First row is the Label.
    displayLabel = new Label
    {
        Font = Font.SystemFontOfSize(NamedSize.Large),
        VerticalOptions = LayoutOptions.Center,
        XAlign = TextAlignment.End
    };
    mainStack.Children.Add(displayLabel);

    // Second row is the backspace Button.
    backspaceButton = new Button
    {
        Text = "\\u21E6",
        Font = Font.SystemFontOfSize(NamedSize.Large),
        IsEnabled = false
    };
    backspaceButton.Clicked += OnBackspaceButtonClicked;
    mainStack.Children.Add(backspaceButton);

    // Now do the 10 number keys.
    StackLayout rowStack = null;

    for (int num = 1; num <= 10; num++)
    {
        if ((num - 1) % 3 == 0)
        {
            rowStack = new StackLayout
            {
                Orientation = StackOrientation.Horizontal
            };
            mainStack.Children.Add(rowStack);
        }

        Button digitButton = new Button
        {
            Text = (num % 10).ToString(),
            Font = Font.SystemFontOfSize(NamedSize.Large),
            StyleId = (num % 10).ToString()
        };
        digitButton.Clicked += OnDigitButtonClicked;

        // For the zero button, expand to fill horizontally.
        if (num == 10)
        {
            digitButton.HorizontalOptions = LayoutOptions.FillAndExpand;
        }
}
rowStack.Children.Add(digitButton);
}

this.Content = mainStack;
}

void OnDigitButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    displayLabel.Text += (string)button.StyleId;
    backspaceButton.IsEnabled = true;
}

void OnBackspaceButtonClicked(object sender, EventArgs args)
{
    string text = displayLabel.Text;
    displayLabel.Text = text.Substring(0, text.Length - 1);
    backspaceButton.IsEnabled = displayLabel.Text.Length > 0;
}
}
```

十个数字键共享同一个 `Clicked` 处理程序。`StyleId` 属性表示与该键关联的数字，因此它可以附加到 `Label` 显示的字符串上。`StyleId` 恰好与 `Button` 的 `Text` 属性相同，并且也可以使用 `Text` 属性，但通常情况下并没有那么方便。

退格 `Button` 在功能上足够不同，以至于需要它自己的 `Clicked` 处理程序，尽管肯定可以将这两种方法合并为一种，以利用它们共有的代码。

为了使键盘的尺寸稍大一些，所有文本都使用 `NamedSize.Large` 创建的 `Font` 对象。以下是 **KeypadStack** 程序的三种渲染效果：

## CHAPTER 3
### Building an app: Infrastructure

有时像这样的编程教程可能会在直接解决实际应用需求之前涵盖大量内容。本章和下一章试图避免这种常见问题。

这两个章节介绍了一个从头开始构建的实际应用程序。这不会是一个非常复杂的应用程序，但它可用且有用。该应用程序的名称是 **NoteTaker**，它允许你做笔记并保存它们。

每个笔记都是一些文本——任意多的文本——以及一个可选的标题。如果不指定标题，将从笔记的前几个词为你生成一个标题。

当程序在下一章末尾最终完成时，它将具有以下主屏幕：

![](img/c2d61d0459c51981af78607f1eef3d1c_89_0.png)

那是已创建笔记标题的可滚动列表。**New** 按钮由加号符号表示，作为顶部或底部的工具栏项出现。点击该按钮，程序将导航到输入新笔记的页面：

我们通过无知而斗争来获取知识和技能所带来的问题。

在本章中，**NoteTaker** 程序经历了六个不同的版本。它们编号为 1 到 6，但版本号更像是 0.1 到 0.6。在通过这六个版本增强此程序的过程中，会遇到以下主题：

- 用于编辑文本的 `Entry` 和 `Editor` 视图
- 移动环境中的文件输入/输出（I/O）
- 异步操作
- 属性更改通知
- 数据绑定

下一章将深入多页面架构、页面导航、用于显示项目集合的强大的 `ListView`，以及处理应用程序生命周期问题。

### Version 1. The Entry and Editor views

**NoteTaker** 应用绝对需要一些文本输入。一些手机有物理键盘，但绝大多数仅限于虚拟屏幕键盘。当然，这些键盘因平台而异，并且通常也因文本输入类型而异。

Xamarin.Forms 为获取文本输入定义了两个视图：

- `Entry` 用于单行文本
- `Editor` 用于多行文本

`Entry` 和 `Editor` 都派生自 `InputView`，后者派生自 `View`。

所有三个平台都有适用于不同类型文本输入的各种虚拟键盘样式。例如，用于输入 URL 的键盘应与用于输入电话号码的键盘不同。因此，`InputView` 定义了一个名为 `Keyboard` 的属性，其类型为 `Keyboard`，该类定义了七个适用于不同键盘用途的静态只读 `Keyboard` 类型属性：

- `Default`
- `Text`
- `Chat`
- `Url`
- `Email`

`Entry` 和 `Editor` 定义了 `Focused` 和 `Unfocused` 事件，用于发出获得和失去输入焦点的信号。`IsFocused` 属性指示特定视图当前是否具有输入焦点。程序可以通过在视图上调用 `Focus` 方法来尝试将焦点设置到特定视图。方法返回 `true` 如果焦点更改成功。这些与焦点相关的成员由 `Visual-Element` 定义，它是所有 UI 元素的基类，但它们实际上仅与 `Entry` 和 `Editor` 相关。

`Entry` 和 `Editor` 都定义了 `Text` 属性，公开视图当前显示的文本。程序可以初始化此 `Text` 属性，然后允许用户编辑该文本。

`Entry` 和 `Editor` 都定义了 `Completed` 事件，当用户发出编辑完成的信号时触发。这是程序访问 `Text` 属性并保存结果的绝佳机会。

`Entry` 和 `Editor` 还定义了 `TextChanged` 事件，当 `Text` 属性更改时触发。记住 .NET `string` 对象是不可变的。`string` 对象在创建后本身不能更改。每当 `Entry` 或 `Editor` 的内容更改时，它都是一个全新的 `string` 对象，而不是一个修改过的 `string` 对象。`TextChanged` 事件可以成为应用程序按字符监控文本输入并在用户发出输入完成信号之前响应更改的宝贵手段。

你可能会认为 `Entry` 比 `Editor` 简单一些，因为它只处理单行文本。然而，`Entry` 有三个 `Editor` 没有的附加属性：

- `TextColor` — 一个 `Color` 值
- `IsPassword` — 一个布尔值，导致字符被屏蔽
- `Placeholder` — 在 `Entry` 中出现的浅色文本，一旦用户开始输入就会消失。

**NoteTaker** 程序需要一个页面，其中包含一个 `Entry` 用于用户输入笔记的标题，以及一个 `Editor` 用于输入笔记本身。对于此程序的第一个版本——称为 **NoteTaker1**——让我们不要做太多超出在页面上放置这两个视图以及用于标识的几个 `Label` 视图的事情。

`Editor` 允许输入无限量的文本，并在内部实现滚动。正如你在上一章中用 `ScrollView` 发现的那样，与页面上的其他视图共享可滚动视图需要将 `VerticalOptions` 属性设置为 `LayoutOptions.FillAndExpand`。

以下是 `NoteTaker1Page` 类：

```
class NoteTaker1Page : ContentPage
{
    public NoteTaker1Page()
    {
        this.Padding = new Thickness(10, Device.OnPlatform(20, 0, 0), 10, 0);
```

