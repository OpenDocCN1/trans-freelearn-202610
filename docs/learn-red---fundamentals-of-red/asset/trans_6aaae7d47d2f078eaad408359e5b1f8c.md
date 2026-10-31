
用户界面屏幕及其包含的所有内容在属性更改时会自动实时更新。面板和方面之间存在 *双向绑定*。当其中一方发生变化时，另一方会相应跟随。我们在示例中不会使用它，但如果你作为开发者想要控制屏幕更新的时机，需要执行以下操作：

*   关闭 `auto-sync` 选项—`system/view/auto-sync?: off`
*   使用 `show` 函数，例如 `show face` 来更新控件

\=> 在 *问题* 部分的第 3 个问题中了解如何编写代码以关闭窗口的按钮。

# 容器设置 – view

这些是描述容器方面（窗口）外观和感觉的设置。我们已经使用了 `title`。以下是一些可以按任意顺序组合的常见设置（将代码粘贴到 Red 控制台中，并尝试使用）：

*   `view [ size 350x125]` 允许具有不同尺寸（宽 x 高）的窗口
*   `view [ backdrop blue]` 创建一个具有蓝色背景的窗口

# 更改应用程序图标

默认的 Red 窗口 *图标*（显示汉诺塔）可以用你自己的 `.ico` 文件替换，如下所示：

```
;-- see Chapter09/icon.red:
Red [
    needs: 'view
    icon: %gaming.ico
]
view []
```

该窗口显示如下：

![](assets/b2a0b307-c367-46f4-b945-6bcf9bebc576.png)

但要看到这个新图标，应用程序必须编译为 `red -r icon.red`。这也适用于在应用托盘中显示的非 GUI 应用程序的图标。

# View 的细化

`view` 还可以有一个或多个细化。它们的细节在主 `view` 块之后出现的块中描述，如下一般 `view` 块的模式所示：

```
view/refine1/refine2 [
    container settings
    layout commands
    face1 facets [action facet]
    face2 facets [action facet]
    ...
][details refine1][details refine2]
```

要确定窗口在屏幕上出现的位置，请使用带有提供 `offset` 值的块的 `options` 细化，如下所示：

```
;-- see Chapter09/container-layout.red:
view/options [
][offset: 400x100]
```

你可能已经注意到标准窗口不可调整大小或最大化。要实现这一点，我们需要带有 `resize` 词的 `flags` 细化：

```
view/flags [
][resize]
```

这些标志可以像本示例中那样组合，其中窗口可调整大小且为模态：

```
view/flags/options [
][modal resize][offset: 400x100]
```

模态意味着窗口显示在父窗口之上，只要模态窗口显示，父窗口就被禁用。换句话说，模态窗口需要你的注意。

还有其他标志，如 `no-title`、`no-border`、`no-min`、`no-max` 和 `no-buttons`。它们的名字说明了它们的作用。适当时将它们组合以创建最小化窗口。

以下是这些标志的使用示例：

```
;-- see Chapter09/digital-clock.red:
time-now: to string! now/time
view/flags [ 
    title "Red Time"
    show-time: text font-size 14 font-color red time-now
]
[modal popup]
```

这显示了以下窗口：

![](assets/e79da340-8a86-40bd-902f-effa00bcb241.png)

它还展示了 `font-size` 和 `font-color` 对 `text` 的使用。要使时钟运行，我们只需要一行代码，但为此我们需要一个事件，请参阅后面的 *事件和参与者* 和 *计时器事件* 部分。

# 在 view 中使用 do

在 `view` 代码块内部，像 `n: 2 ** 3` 和 `print n` 这样的普通绑定语句 *必须* 放在 `do` 块中，如下所示：

```
;-- see Chapter09/container-layout.red:
view [
    text "Use of do block"
    do [n: 2 ** 3 print n]
]
```

`do` 块可用于在布局显示之前评估代码并初始化或更改 GUI。

所有其他代码必须出现在 action 方面代码块中，例如我们的按钮点击代码 `[download-rates]`。

# 布局命令

Red 使用正常的 *坐标系统*，原点 `0x0` 在左上角：

![](assets/8eb09781-0076-48a6-ba5c-e5b8917de5ea.png)

坐标为 (`n`, `m`) 的点记为 `nxm`。这样的 `pair!` 值也可以用于 `face/offset` 属性，该属性描述了面相对于原点的左上角位置，在本例中为左侧 `n` 像素，向下 `m` 像素。它也可以用于将面移动到新位置：

```
;-- see Chapter09/offset.red:
t: text "I will not stay here"
button "Move text" [t/offset: 20x30] 
```

默认情况下，Red 使用一种用 `across` 词表示的流式布局。面从左到右放置，直到屏幕行填满，然后填充下一行，依此类推。

除了 `across` 和 `below` 之外，还有一些词可以微调此过程：

*   `return` 在 `across` 模式下将下一个面水平放置在下一行，或在 `below` 模式下垂直放置在其旁边
*   `at nxm` 将下一个面（仅此一个）的左上角放置在绝对位置 (`n`, `m`)
*   `origin nxm` 从位置 (`n`, `m`) 而不是 (0, 0) 开始窗口布局
*   `pad wxh`，其中 `w` 为宽度，`h` 为高度，指定下一个面从右侧开始 `w` 像素，向下 `h` 像素
*   `space wxh`，其中 `w` 为宽度，`h` 为高度，指定下一个面之间在宽度和高度上的新间距（换句话说，就是偏移量）

为了了解这些命令的工作原理，请在 *问题* 部分的第 4 个问题中尝试使用它们。

# 面板概览

VID 方言包含约 20 种不同的面板（图形控件），每个面板都有其特定的属性（*方面*）。该值的类型指定了方面的作用。例如，字符串将用作面板的显示文本，或一对值将确定其大小。大多数面板还具有默认的 *动作方面*，即在特定事件（如点击按钮或从列表中选择）触发时执行的命令块。

我们不可能详细讨论所有内容，相反我们将对可用的面板类型进行概览，说明你可以做什么，并突出一些重要属性。鼓励你在阅读过程中（与问题一起）尝试这些内容。

# Base、box 和 image

我们从最基本的类型开始：

![](assets/06c162b0-70c9-4b36-9d06-ef0109868d77.png)

这是 VID 代码：

```
;-- see Chapter09/base-box-image.red:
view [
    base
    box red
    image 140x140 %heads.jpg
    below
    text "I am a button:"
    button %heads.jpg [quit]
]
```

`Base` 是其中最原始的。它显示一个灰色矩形，但可以响应点击（`on-down`）事件；它可以显示图像，并且可以在其内部绘制。`box` 类似于 base，但默认情况下是透明的，因此我们为其指定颜色以使其显示。

`image` 是一个默认大小为 100 x 100 的 base 面板，用于显示图片。每当一个面板的方面为图像文件时，它将被用作该面板的背景图像。参见按钮示例。`camera` 方面也可用，当硬件可用时（如移动设备）可捕获视频流。

# 文本面板 – h1、text、field 和 area

这是一个使用文本面板的窗口：

![](assets/a9ff03ce-e707-4dd0-b476-b969776615f9.png)

这是生成该窗口的代码（请注意，在真正的应用程序中，我们必须为控件提供有意义的名称，而不是 `t1` 和 `f1`！）：

```
; see Chapter09/text-faces.red:
view [
    title "Text faces"
    below
    h3 "Heading 3"
    t1: text 300x30 bold italic blue font-name "algerian" 
        font-color ivory font-size 14 "Text with properties"
    text "Click me!" [face/color: orange face/text: "FIRE!"]
    f1: field 300 "Type something here" [ print [f1/data] ]
    a: area 300x100 [print a/text]
]

