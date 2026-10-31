
然后，我们带你参观了可用的面孔，例如 `text`、`field`、`button`、`check`、`drop-list`、`panel`、`tab-panel`、`slider`、`menus` 等。要真正构建动态屏幕，你必须在这些不同的面孔上使用 `actors`。你学会了如何编写它们，在一个面孔上组合多个 actors，检测按键，并创建定时事件。我们还举例说明了如何开始构建更复杂的维护应用程序。最后，你对功能非常强大的 `draw` 方言进行了介绍，它使你能够绘制 2D 形状和动画。

## 问题
1. 重写 `currency-rates-gui.red` 中的 `download-rates`，使其接受代码和日期作为参数。
2. 将 `button` 对象的内部结构打印到 `Red` 控制台。注意 `button` 有一个 `window` 父级面孔。
3. 你可以通过在其 `action` `facet` 中使用 `quit` 或 `unview` 词来编写按钮以关闭其窗口。两者都试一下。注意当有多个窗口需要关闭时使用 `unview/all`。
4. 创建一个窗口，显示具有不同颜色的方形 `base` 面孔，例如 `base yellow 25x25`。尝试几种布局命令。
5. 创建一个包含按钮和复选框的窗口。通过点击按钮更改复选框的状态和文本。点击三次后禁用该按钮（提示：使用 `face/enabled?`）。
6. 创建一个窗口，显示按 `text-list` 排序的系列的数据值。你可以通过两种方式实现——使用按钮 `action`，或使用 `do` 块。
7. 从系列 `\["Dave" 1804 "Peter" 9439 "Viviane" 2386 "Marcus" 9423``\]` 中提取名称并在 `text-list` 中显示。当选择一个名称时，在 `text` 字段中显示对应的数字。此外，将该名称在原始列表中的索引打印到控制台。
8. 创建一个窗口，显示文本，当鼠标光标悬停其上时背景颜色会改变。还定义一个字符串列表，例如城市。定义一个 `on-wheel` actor，按顺序显示每个城市。
9. 在宽度为 500、高度为 300、背景为白色的矩形 `base` 上，在矩形内绘制红色对角线和蓝色垂直线。

## 进一步阅读
你可以在这里找到有关 `VID` 的一些额外信息：[http://www.red-by-example.org/vid.html](http://www.red-by-example.org/vid.html)。
有关绘图方言的更多信息可以在这里找到：[http://www.red-by-example.org/draw.html](http://www.red-by-example.org/draw.html)。
目前关于 GUI 系统的官方文档比较正式，可以在这里找到：[https://doc.red-lang.org/en/gui.html](https://doc.red-lang.org/en/gui.html)。它包含有关 `View` 引擎、`VID` 和 `draw` 方言的深入信息。

# 高级 Red
在本最终章节中，我们将探索语言的更多高级方面，并展望 Red 在不久的将来的发展方向。响应式编程是编写更具响应性（GUI）应用程序的绝佳方式。我们在 [第1章](#b2834a1f-ebba-49f2-9d61-4bdfdf8a5702.xhtml)，*Red 的使命*，和 [第2章](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml)，*开发环境设置* 中遇到了 `Red/System` 作为 Red 的底层基础。在这里，我们将看到你如何在 Red 应用程序中或独立使用它。

本章将涵盖以下主题：
* 响应式编程
* `Red/System`
* 与操作系统交互
* 用于性能的数据类型
* 嵌入式 Red 和宏
* Red 用于智能合约编程
* Red 的路线图
* 一些有用的参考

## 技术要求
你可以在 [https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter10](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter10) 找到本章的代码。如果你已按照 [第2章](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml)，*开发环境设置* 中所述安装了 Red，你就可以开始了。你可以在 Windows、OS X 或 Linux 上工作。你可以在 `Red` 控制台中输入或粘贴任何代码以查看其结果，或者你可以使用 Visual Studio Code 等编辑器开始使用。

## 响应式编程
在 [第9章](#97dee83c-19eb-4b99-8e08-720cb23a61ad.xhtml)，*构建视觉界面* 中，我们看到了通过附加 actor 代码块来处理小部件上的事件是多么容易。我们遇到了几个例子，其中一个面孔中的更改或操作会影响另一个面孔——例如，点击按钮会更改字段中的文本。文本字段对按钮做出了反应，因为我们明确地在按钮上编写了 actor。

请看以下代码片段：
```
;-- see Chapter10/reactive1.red:
point: [x: 3 y: 5]
distance: square-root (point/x ** 2) + (point/y ** 2)
print distance    ;== 5.8309518948453
point/x: 2
print distance    ;== 5.8309518948453    <-- doesn't change!
distance: square-root (point/x ** 2) + (point/y ** 2)
print distance    ;== 5.385164807134504  <-- changed
```

`distance` 词给出了 `point` 到原点的距离，但当点的坐标发生变化时，`distance` 并不会随之自动改变。只有在更改后要求 `distance` 再次计算自己时，我们才会得到更新的结果。

你可以想象，让词在更改发生时自动更新其值会很有用——例如，在跟踪 running total 时，或在电子表格单元格中的计算。Red 已将其构建到其响应式框架中。以下是如何使用它使 `distance` 跟随 `point` 的更改：
```
point: make reactor! [x: 3 y: 5]
distance: is [square-root (point/x ** 2) + (point/y ** 2)]
print distance    ;== 5.8309518948453
point/x: 2
print distance    ;== 5.385164807134504
```

`point` 词是一个所谓的 `reactor`，即更改的原因或 `source`；这用 `make reactor!` 表示。`reactor` 字段中的更改会自动（即无需调用函数）触发其他 `target` 对象的更改，例如本例中的 `distance`。这些目标通过使用 `is` 词定义为 `reactive expression`。更改从 `source` 传播到 `target`：
`reactor` (`source`) ----> `target(s)`.

reactor 甚至可以更新自身，这需要的代码更少，如下所示：
```
point: make reactor! [
    x: 3 
    y: 5 
    distance: is [square-root (x ** 2) + (y ** 2)]
]
point ;== make object! [x: 3 y: 5 distance: 5.8309518948453]
point/x: 2
point ;== make object! [x: 2 y: 5 distance: 5.385164807134504]
```

不要让更改导致自身更改，引发无休止的循环！

更改的来源必须是具有字段的对象，例如示例中的 `point/x` 和 `point/y`。如果目标引用多于一个嵌套级别（例如 `shape/point/x`），请改用 `make deep-reactor!` 而不是 `make reactor!`。

这种响应性在编写图形界面时也可以使用——而不是编写 actor，请在目标侧使用 `react` 词。以下示例比较了以两种方式完成的滑块和进度条之间的同步。在 `Tab 1` 中，使用了响应式编程（进度条是目标），而在 `Tab 2` 中，我们采用了上一章的 actor 代码：
```
;-- see Chapter10/reactive-gui.red
view [
    origin 0x0 space 0x0
    tab-panel 500x500 [
        "Tab 1 " [
            progress 100x20 data 20% react [face/data: s/data]
            s: slider 100x20 data 20%
        ]
        "Tab 2 " [
            p: progress 100x20 20%
            slider 100x20 data 20% [p/data: face/data]
        ]
    ]
]
```

响应式框架只有大约 250 行 Red 代码，并且非常通用。一个对象可以是许多源对象的目标，你也可以创建反应对象的链。此框架不会让你做以前做不到的事情，但它会让你以另一种方式思考问题，你的代码将更紧凑、可读且声明式。
