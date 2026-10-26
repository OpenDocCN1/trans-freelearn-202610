

```csharp
Task<Page> PopModalAsync();
}
```

这些都是异步方法，这意味着它们会立即返回，而导航操作则继续进行。

正如你可能从这些方法名称中推断出的那样，Xamarin.Forms 中的页面导航的工作方式类似于熟悉的后进先出栈：每次 `PushAsync` 调用都会将当前页面压入栈并导航到指定的页面。每次调用 `PopAsync` 都会将前一个页面从栈中弹出并导航回该弹出页面。这种架构用户非常熟悉，因为 Web 浏览器的工作方式类似。

显式调用 `PopAsync` 并非总是必需的，因为用户通常可以使用 Android 或 Windows Phone 上的硬件返回按钮，或由 iOS 和 Android 作为页面导航用户界面的一部分自动提供的返回按钮来向后导航。`PopToRootAsync` 调用本质上会清空栈并返回到主页。未来的章节（本预览版中未包含）将更详细地讨论页面和导航，包括用于弹出页面导航的 Modal 方法。

`INavigation` 接口由 Xamarin.Forms 内部的一个类实现，但该类可通过 `VisualElement` 定义的 `Navigation` 属性返回的对象访问。非常经常，使用页面本身的 `Navigation` 属性来导航到另一个页面会很方便。例如：

```csharp
this.Navigation.PushAsync(new AnotherPage());
```

然而，如果更方便，你也可以使用页面上任何视图的 `Navigation` 属性。

要在 Xamarin.Forms 应用程序中启用页面导航，`App.GetMainPage` 方法应返回 `NavigationPage` 的实例而不是 `ContentPage`。幸运的是，此更改非常容易。如果 `HomePage` 派生自 `ContentPage`（在 NoteTaker7 应用程序中就是这种情况），那么 `App.GetMainPage` 只需将该页面的新实例传递给 `NavigationPage` 构造函数并返回结果即可。以下是 NoteTaker7 中的 App 类：

```csharp
namespace NoteTaker7
{
    public class App
    {
        public static Page GetMainPage()
        {
            return new NavigationPage(new HomePage());
        }
    }
}
```

这会将一个 `ContentPage` 对象转换为一个 `NavigationPage` 对象。

严格来说，将 `ContentPage` 包装在 `NavigationPage` 中对于 Windows Phone 并不是必需的，但你需要它用于 iOS 和 Android，因此最简单的方法是为三个平台都使用相同的代码。

对于 iOS 和 Android，此更改对页面的视觉外观有明显影响。页面顶部会预留一个区域用于页面标题，你可以使用 `Page` 定义的 `Title` 属性来设置。你在启用页面导航时应设置此 `Title` 属性。

**NoteTaker7** 中的 `HomePage` 类派生自 `ContentPage`，构造函数设置了该 `Title` 属性。构造函数还创建了一个居中的 `Button`，其文本为“Add New Note”，并有一个 `Clicked` 处理程序，该处理程序根据当前日期和时间生成唯一的文件名并导航到 `NotePage`：

```csharp
namespace NoteTaker7
{
    class HomePage : ContentPage
    {
        public HomePage()
        {
            this.Title = "Note Taker 7";

            Button button = new Button
            {
                Text = "Add New Note",
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.Center
            };

            button.Clicked += (sender, args) =>
            {
                // Create unique filename.
                DateTime datetime = DateTime.UtcNow;
                string filename = datetime.ToString("yyyyMMddHHmmssfff") + ".note";

                // Navigate to new NotePage.
                this.Navigation.PushAsync(new NotePage(filename));
            };

            this.Content = button;
        }
    }
}
```

以下是 **NoteTaker7** 在三个平台上运行时此页面的外观：

![](img/c2d61d0459c51981af78607f1eef3d1c_154_0.png)

注意 iPhone 和 Android 上的标题。使用 iPhone 上的 `NavigationPage` 时，你不再需要担心页面上的任何内容占用与状态栏相同空间，无论是在主页上还是从该页面导航到的任何页面上。

NoteTaker7 中的 Note、IFileHelper 和 FileHelper 类与 NoteTaker6 中的相同。

`NotePage` 类与程序早期版本中的页面类相似。它派生自 `ContentPage`，但定义了一个带有一个字符串参数的构造函数，指示笔记的文件名。此构造函数将文件名保存为私有字段，设置 `Title` 属性，并构建页面的其余部分：

```csharp
namespace NoteTaker7
{
    class NotePage : ContentPage
    {
        string filename;
        Note note = new Note();

        public NotePage(string filename)
        {
            this.filename = filename;
            Title = "New Note";

            // Create Entry and Editor views.
            Entry entry = new Entry
            {
                Placeholder = "Title (optional)"
            };
```

![](img/c2d61d0459c51981af78607f1eef3d1c_156_0.png)

再次注意，此页面的 `Title` 属性显示在 iOS 和 Android 屏幕顶部，并且左侧的角度括号提示点击此处可导航回主页。此外，Android 和 Windows Phone 设备底部最左侧的按钮也会导航回主页。

那么加载和保存按钮在哪里呢？嗯，加载按钮在这里没有意义，因为在这个版本的程序中 `NotePage` 仅在具有新文件名时显示。文件尚不存在。然而，`NotePage` 应该有一些保存笔记的功能。

它确实有。在使用页面导航时，利用 `Page` 类定义的两个受保护的虚拟方法 `OnAppearing` 和 `OnDisappearing` 通常很方便。`OnAppearing` 方法在导航到页面时被调用，而 `OnDisappearing` 方法在从页面导航离开时被调用。在 `NotePage` 类中，`OnAppearing` 在这里没做什么（只是为了对称性而显示），但 `OnDisappearing` 重写是存储 `Note` 对象的完美时机：

```csharp
namespace NoteTaker7
{
    class NotePage : ContentPage
    {
        ...
        protected override void OnAppearing()
        {
            base.OnAppearing();
        }
        protected override void OnDisappearing()
        {
            // Only save it if there's some text somewhere.
            if (!String.IsNullOrWhiteSpace(note.Title) ||
                !String.IsNullOrWhiteSpace(note.Text))
            {
                note.SaveAsync(filename);
            }
            base.OnDisappearing();
        }
    }
}
```

这意味着用户无需费心显式保存笔记。在导航回主页时会自动完成。

你可能会想：如果用户在导航返回之前终止应用程序怎么办？或者让应用程序在 `NotePage` 仍处于活动状态时运行，然后关闭手机？是的，这确实是个问题。但这实际上是涉及 *应用程序生命周期* 的主题问题，而在这一章结束之前将会讨论这个问题。

## Version 8. Notes in the ListView

随着 **NoteTaker** 程序增强为显示多个笔记，一些重构可能很有必要。目前，`Note` 对象和与该 `Note` 对象关联的文件名并不是紧密关联的。程序有可能使用与 `LoadAsync` 方法不同的文件名调用 `Note` 的 `SaveAsync` 方法。

让我们修复这个问题。在 **NoteTaker8** 中，让我们将文件名传递给 `Note` 构造函数，该构造函数将其保存为名为 `Filename` 的公共只读属性。最终你会看到使此属性公开的原因。然而，由于此 `Filename` 属性在构造函数中设置且永不更改，因此该属性没有理由触发 `PropertyChanged` 事件。这允许 `LoadAsync` 和 `SaveAsync` 方法被重新定义为无参数。以下是新版本的相关部分：

```csharp
namespace NoteTaker8
{
    class Note : INotifyPropertyChanged
    {
        string title, text;

        public event PropertyChangedEventHandler PropertyChanged;

        public Note(string filename)
        {
            this.Filename = filename;
        }

        public string Filename { private set; get; }
```

