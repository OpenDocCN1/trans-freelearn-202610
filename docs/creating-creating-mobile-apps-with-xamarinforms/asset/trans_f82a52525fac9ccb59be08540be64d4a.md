
```
Device.StartTimer(TimeSpan.FromSeconds(1), OnTimerTick);
}

void OnPageSizeChanged(object sender, EventArgs args)
{
    // Scale the font size to the page width
    //      (based on 11 characters in the displayed string).
    if (this.Width > 0)
        clockLabel.Font = Font.SystemFontOfSize(this.Width / 6);
}

bool OnTimerTick()
{
    // Set the Text property of the Label.
    clockLabel.Text = DateTime.Now.ToString("h:mm:ss tt");
    return true;
}
}
```

程序创建一个用于显示时间的 Label，然后设置两个事件：页面的 SizeChanged 事件用于更改字体大小，Device.StartTimer 事件用于按一秒的间隔触发。两个事件处理程序都只是更改 Label 的属性。Device.StartTimer 事件必须返回 true 以继续计时，返回 false 以停止计时。

OnTimerTick 方法为 DateTime 指定了一个自定义格式字符串，该字符串产生 10 或 11 个字符，但其中两个是大写字母，它们比平均字符宽度更宽。SizeChanged 处理程序隐式假设显示 12 个字符，将字体大小设置为页面宽度的 1/6：

![](img/c2d61d0459c51981af78607f1eef3d1c_214_0.png)

当然，在横屏模式下文本要大得多。不幸的是，本书的截图仅针对竖屏模式设计，因此你需要将本书侧过来才能看到程序在横屏模式下的样子：

![](img/c2d61d0459c51981af78607f1eef3d1c_215_0.png)

当然，这个一秒钟的计时器并不会在每秒开始时精确触发，因此显示的时间可能与该设备上的其他时间显示不完全一致。你可以通过设置更快的计时器触发频率来提高精度。性能不会受到太大影响，因为显示仍然每秒只更新一次，并且在此期间不需要新的布局周期。

## 图像与位图

就呈现而言，文本是必不可少的，但图片嘛……嗯，图片通常也是必不可少的。

名为 `Image` 的视图之于位图，正如 `Label` 之于文本。`Image` 显示的位图可以来自网站（或通过 URL 访问），也可以作为资源绑定到程序的 PCL 中，还可以是各个平台项目或 SAP 的内容，或者可以通过 .NET `Stream` 对象进行访问。

`Image` 定义了一个 `Source` 属性，你可以将其设置为 `ImageSource` 类型的对象，后者定义了四个用于引用位图的静态方法，分别是：

- `ImageSource.FromUri` 用于通过网络访问位图。
- `ImageSource.FromResource` 用于访问作为资源存储在应用程序 PCL 中的位图。
- `ImageSource.FromFile` 用于访问作为内容存储在平台项目或 SAP 中的位图。

![](img/c2d61d0459c51981af78607f1eef3d1c_217_0.png)

无论位图尺寸小于还是大于页面尺寸，这种精确适配都会发生。这个位图恰好是 4096 像素见方，但你可以像下面这样指定 uri 变量来下载一个更小的文件：

```
string uri = "http://developer.xamarin.com/demo/IMG_1415.JPG?width=100";
```

图像仍然会被拉伸以适应该区域。

因此，当你在竖屏和横屏模式之间旋转手机或模拟器时，渲染的位图可能会改变大小，并且你会在位图未覆盖的顶部和底部或左侧和右侧看到一些空白区域。你可以使用 Image 从 VisualElement 继承的 BackgroundColor 属性为该区域着色。

Image 定义了一个控制位图渲染方式的 Aspect 属性。默认设置是枚举成员 `Aspect.AspectFit`，这意味着位图在保持自身纵横比的同时适合其父级边界。

试试这样做：

```
class BitmapFromWebsitePage : ContentPage
{
    public BitmapFromWebsitePage()
    {
        string uri = "http://developer.xamarin.com/demo/IMG_1415.JPG";

        this.Content = new Image
        {
            Source = ImageSource.FromUri(new Uri(uri)),
            Aspect = Aspect.Fill
        };
    }
}
```

- iOS：**BundleResource**
- Android：**AndroidResource**
- Windows Phone：**Content**

## 绝对布局与附加可绑定属性

下面是 Xamarin.Forms 类层次结构，显示了从 `Layout` 派生的所有类：

```
System.Object
    BindableObject
        Element
            VisualElement
                View
                    Layout
                        ContentView
                            Frame
                            ScrollView
                        Layout<T>
                            AbsoluteLayout
                            Grid
                            RelativeLayout
                            StackLayout
```

你已经见过 `ContentView`、`Frame` 和 `ScrollView`（它们都有一个 `Content` 属性，你可以将其设置为单个子元素）以及 `StackLayout`（它从 `Layout<T>` 继承了 `Children` 属性）。`Grid` 和 `RelativeLayout` 实现了更复杂的布局模型；`Grid` 将在下一章中探讨，而 `RelativeLayout` 将在本预览版未包含的后续章节中介绍。

乍一看，`AbsoluteLayout` 类实现了一种相当原始的布局模型——一种可回溯到早期图形用户界面那段并不那么美好的旧时光的模型，那时程序员必须逐个调整屏幕上每个元素的大小和位置。

使用 `AbsoluteLayout` 时，到目前为止你学到的大部分布局规则都不再适用。当 `View` 是 `ContentPage` 或 `StackLayout` 的子元素时，`HorizontalOptions` 和 `VerticalOptions` 属性非常重要；但当 `View` 是 `AbsoluteLayout` 的子元素时，它们*完全没有效果*。相反，程序必须为 `AbsoluteLayout` 的每个子元素指定一个特定位置。子元素也可以被赋予特定大小，或者允许其自行确定大小。

你可以像使用 `StackLayout` 一样，将子视图添加到 `AbsoluteLayout` 的 `Children` 集合中：

```
absoluteLayout.Children.Add(child);
```

附加可绑定属性是一种通用机制，允许一个类定义的属性存储在另一个类的实例中，但附加可绑定属性主要用于布局类。正如你将在下一章中看到的，`Grid` 定义了附加可绑定属性来指定每个子元素的行和列。

程序可能很难为 `AbsoluteLayout` 的子元素确定合适的宽度和高度，但通常你并不需要这样做。如果你在传递给 `AbsoluteLayout.SetLayoutBounds` 的 `Rectangle` 中，为宽度和高度参数指定静态只读属性 `AbsoluteLayout.AutoSize`，那么子元素将根据它自行计算的宽度和高度来确定大小。

`AbsoluteLayout` 重新定义了其 `Children` 属性，以提供一些快捷方式。你可以在将子视图添加到 `AbsoluteLayout` 时指定初始位置和大小：

```
absoluteLayout.Children.Add(child, new Rectangle(x, y, width, height));
```

或者，如果你希望视图自动调整大小：

```
absoluteLayout.Children.Add(child, new Point(x, y));
```

但是，如果在子元素已添加到 `Children` 集合后需要更改其位置和大小，则必须使用静态 `AbsoluteLayout.SetLayoutBounds` 方法。

可以推测，使用 `AbsoluteLayout` 比使用 `StackLayout` 更困难。通常，让 Xamarin.Forms 和其他 `Layout` 类为你处理大部分布局复杂性要容易得多。但对于某些特殊应用来说，`AbsoluteLayout` 是理想的选择。

其中一个特殊应用是 **DotMatrixClock**，它使用模拟的 5 × 7 点阵显示屏显示当前时间的数字。每个点都是一个 `BoxView`，在屏幕上单独调整大小和位置，并根据该点的亮灭状态着色为红色或浅灰色。可以设想，这个时钟的点可以组织在嵌套的 `StackLayout` 元素或 `Grid`（下一章介绍）中，但无论如何，每个 `BoxView` 都需要被赋予一个大小。这些视图数量庞大且排列规整，这表明与必须以更通用方式执行位置计算的布局相比，程序员更清楚如何在屏幕上安排它们。

`numberPatterns` 和 `colonPatterns` 字段定义了 10 个数字和冒号分隔符的点阵模式：

```
class DotMatrixClockPage : ContentPage
{
    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0 }, { 1, 0, 0, 0, 1 }, { 1, 0, 0, 1, 1 }, { 1, 0, 1, 0, 1 },
            { 1, 1, 0, 0, 1 }, { 1, 0, 0, 0, 1 }, { 0, 1, 1, 1, 0 }
        },
        {
            { 0, 0, 1, 0, 0 }, { 0, 1, 1, 0, 0 }, { 0, 0, 1, 0, 0 }, { 0, 0, 1, 0, 0 },
```
