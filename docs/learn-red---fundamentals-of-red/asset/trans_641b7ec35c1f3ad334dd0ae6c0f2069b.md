
正如其名称所示，这些面用于指示某个数量的增加或减少。进度条是被动的，仅显示状态。滑块可以被移动来指示进度或减少，这是一个 `on-change` 事件。两个面都将数量保存在 `/data` 中，作为 `percent!` 值（如 25 %）。如果将其指定为 0.0 到 1.0 之间的浮点数，则需要使用 `to-percent` 将该值转换。

以下是它们使用的一些示例：

![](assets/d9622a5b-2d3a-4bfe-8142-bc6009119011.png)

实现此功能的代码如下：

```
;-- see Chapter09/progress-slider.red:
view [
    across
    p1: progress 100x20 10%    ;-- horizontal progress bar:
    p2: progress 20x100 15%    ;-- vertical progress bar:
    ;-- bar following input:
    below
    text "Give the percentage (like 30%) and ENTER"
    field [p3/data: face/data]
    p3: progress
    ;-- slider showing status in text:
    t: text "25%"
    slider 100x20 data 25% [t/text: to-string face/data]
    return
    ;-- slider displayed in a progress bar:
    slider 100x20 data 10% [p4/data: face/data]
    p4: progress 100x20 10% 
]
```

进度条也可以像你所看到的那样垂直使用。第三个中，进度与字段的输入保持同步。滑块在被移动时有一个默认的 `change` 事件；在这里它将状态显示在文本字段中。在最右边的示例中，此事件使进度条与滑块保持同步。

# 样式

如果你的应用需要多个具有相同属性和行为器的面实例，例如棋盘游戏的方块或计算器的按钮，你可以将其定义为一个具有名称的 `style` 并使用该名称代替。以下来自 Nick Antoniacci 的方块游戏（仅 10 行！）对此进行了说明：

```
;-- see Chapter09/tile.red:
view [
    title "Tile Game"
    backdrop silver
    style tl: button 100x100 [
        temp: face/offset
        face/offset: empty/offset 
        empty/offset: temp
    ] 
    tl "8" tl "7" tl "6" return 
    tl "5" tl "4" tl "3" return 
    tl "2" tl "1" empty: base silver
]
```

我们为所需的全部八个按钮定义了一个样式 `tl`。点击一个方块按钮会将其移动到空位 `empty`，方法是将其 `offset` 设置为 `empty` 的 `offset`。然后 `empty` 通过将其 `offset` 设置为 `temp`（即该按钮的 `offset`）移动到方块的位置。它看起来像这样：

![](assets/81c94086-7eae-42a7-806e-3c14eb65a674.png)

因此，`style` 用于创建自定义面，且 `style` 的属性可以在具体控件中被覆盖。

# 事件和行为器

在之前的示例中，我们看到大多数面都可以拥有行为器，它们是事件处理程序，当该面发生特定事件时执行。例如，我们刚刚探索的代码中的滑块有一个默认的 `on-change` 行为器，即最后一个块中的代码：

```
slider 100x20 data 25% [t/text: to-string face/data]
```

并非所有面都能响应所有类型的事件。但很多面也可以响应多种事件。

Red 定义了一些 40 个事件，你可以在你的面上为其定义行为器。你可以在这里找到完整列表：[https://doc.red-lang.org/en/view.html#\_events](https://doc.red-lang.org/en/view.html#_events)。

行为器都有简短的名称，例如 `move`、`over` 或 `down`。如果我们将其分类，可以区分以下几种：

* 鼠标事件，例如 `down`、`up`、`drag`、`drop`、`click`、`dbl-click`、`over`、`wheel` 等，包括三个按钮、鼠标滚轮以及悬停在面上的事件
* 窗口事件，例如 `move` 和 `resize`
* 键盘事件，例如 `key`、`key-down`、`key-up` 和 `enter`
* 触摸事件，例如 `zoom`、`pan`、`rotate` 和 `tapping`
* 一个计时器事件（`time`）
* 通用事件，例如 `focus`、`select`、`change` 和 `menu`

要在面上实现一个行为器，请在其代码块前指定一个 `on-event` 名称。这仅对额外的行为器必要，而对默认行为器则不需要。在以下各节中，我们将查看一些典型示例并了解更多其内部原理。

## 多个事件

实现多个事件很容易。以下是一个为按钮定义了四个行为器的示例：

```
;-- see Chapter09/multiple-events.red:
view [
    below
    message: text 100x25 green "Nothing"
    button "Different things happen" 
        [message/text: "A click!"]               ; default event
        on-over [message/text: "Hovering!"]  ; hovering cursor over face
        on-dbl-click [message/text: "A double click!"]
        on-wheel [message/text: "Wheeling!"]     ; turning mouse wheel
]
```

显示如下：

![](assets/70e2d38d-f186-4cf4-bc31-08f151f803f2.png)

请注意，左鼠标按钮点击仅对按钮控件称为 `on-click`，对于所有其他面，它是 `on-down`，随后是 `on-up` 事件。

=> 现在回答 *Questions* 部分的第 8 个问题。

## 计时器事件

此类事件有一个 `on-time` 行为器。该行为器与面的 `rate` 方面一起工作，该方面设置事件发生的速率，可以表示为：

* 每秒速率，例如每秒一次：`rate 1`，因此 `rate` 50 比 `rate` 1 快 50 倍
* 作为一个 `time!` 值，例如 `rate` 0:00:03 表示每 3 秒一次

每次滴答时，`on-time` 行为器都会执行。以下是一些简单的示例。请注意，我们如何通过在 `on-time` 行为器中更改 `offset` 来实现一些原始的动画效果：

```
;-- see Chapter09/timers.red:
view [
    size 400x400
    b: button 80x30 green "Catch me" rate 50 
        on-time [b/offset: b/offset + 1x1]
        on-down [quit]
    t1: text txt rate 5
        on-time [either t1/text = "" [t1/text: txt][t1/text: ""]]
    t2: text txt rate 0:00:03
        on-time [either t2/text = "" [t2/text: txt][t2/text: ""]]
]
```

现在是时候让 *Container settings* 部分中的数字时钟工作了。为此，我们只需要添加一行（以粗体显示）来定义 `on-time` 行为器，该行为器每秒执行一次：

```
;-- see Chapter09/digital-clock2.red:
time-now: to string! now/time
view/flags [ 
    title "Clock"
    show-time: text font-size 13 font-color red time-now
    rate 1 on-time [show-time/text: to string! now/time]
]
[modal popup] 
```

## 行为器是对象

如果你解决了第 2 题，你会知道面是一个具有多个方面的复杂对象。它包含的行为器存储在一个方面中，该方面是一个对象，每个事件一个字段。假设我们有以下按钮代码：

```
;-- see Chapter09/faces-are-objects.red:
bt: button "Print me!" [print bt]
```

点击按钮，然后滚动到控制台输出的末尾，你会找到以下内容：

```
actors: make object! [
     on-click: func [face [object!] event [event! none!]][print bt]
]
```

我们将在接下来的两个部分中应用它。

## 检测键盘

行为器实际上是事件名称和一个函数的组合，该函数接受一些参数，主要是在其上发生事件的控件和事件本身。以下代码片段展示了如何检测何时按下某些键：

```
;-- see Chapter09/key-detecting.red:
view/options [     
    t: text "hit 'space', 'a', or '?' keys !"][
    actors: object [on-key: func [key event] [
            if event/key = #"a" [t/text: "'a' key pressed."]
            if event/key = #"?" [t/text: "'?' key pressed."]
            if event/key = #" " [t/text: "'space bar'pressed."] 
        ]
    ]
]
```

## 菜单

演员是对象的事实在我们看到 Red 中菜单事件是如何定义时变得非常明显。菜单的定义首先通过声明一个 `layout` 对象开始。然后，通过 `/menu` 字段，菜单标题和子菜单被定义为一个列表。每个子菜单在列表中都是更深的嵌套层级，例如：
