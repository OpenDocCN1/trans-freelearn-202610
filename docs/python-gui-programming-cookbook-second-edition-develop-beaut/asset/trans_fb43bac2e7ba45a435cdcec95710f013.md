

## 如何操作...

为了向我们的 GUI 添加一个 Label 控件，我们将从 tkinter 导入 ttk 模块。请注意这两个 import 语句。将以下代码添加到 `win.mainloop()` 之前，该语句位于第一和第二份食谱的底部：

GUI_add_label.py

```
6 #===================
7 # imports
8 #===================
9 import tkinter as tk
10 from tkinter import ttk
11
12 # Create instance
13 win = tk.Tk()
14
15 # Add a title
16 win.title("Python GUI")
17
18 # Adding a Label
19 ttk.Label(win, text="A Label").grid(column=0, row=0)
20
21 #===================
22 # Start GUI
23 #===================
24 win.mainloop()
```

运行代码后，我们的 GUI 中会添加一个标签：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_37_0.png)

## 其工作原理...

在前述代码的第 10 行，我们从 tkinter 包中导入了一个单独的模块。ttk 模块有一些高级控件，能让我们的 GUI 看起来很棒。从某种意义上说，ttk 是 tkinter 包中的一个扩展。

我们仍然需要导入 tkinter 包本身，但我们必须指定现在还想使用 tkinter 包中的 ttk。

ttk 代表 themed tk。它改善了我们 GUI 的外观和感觉。

第 19 行在我们调用 mainloop 之前将标签添加到 GUI。

我们将窗口实例传递给 ttk.Label 构造函数并设置 text 属性。这将成为我们的 Label 显示的文本。

我们还使用了网格布局管理器，我们将在第 2 章《布局管理》中更深入地探讨它。

注意我们的 GUI 突然比之前的食谱小了很多。

它变得这么小的原因是我们向表单添加了一个控件。没有控件时，tkinter 包会使用默认大小。添加控件会导致优化，这通常意味着使用尽可能少的空间来显示控件。

如果我们使标签的文本更长，GUI 会自动扩展。我们将在第 2 章《布局管理》中的后续食谱中介绍这种自动表单大小调整。

## 还有更多...

尝试调整此带标签的 GUI 的大小并最大化它，看看会发生什么。

## 创建按钮并更改其文本属性

在本食谱中，我们将添加一个按钮控件，并使用此按钮来更改我们 GUI 的另一个控件的属性。这让我们了解了 Python GUI 环境中的回调函数和事件处理。

### 准备工作

本食谱扩展了前一个食谱 *向 GUI 表单添加标签*。你可以从 `https://github.com/PacktPublishing/Python-GUI-Programming-Cookbook-Second-Edition/` 下载全部代码。

### 如何操作…

我们添加一个按钮，点击时执行操作。在本食谱中，我们将更新我们在上一食谱中添加的标签以及按钮的文本属性：

`GUI_create_button_change_property.py`

```
18  # Adding a Label that will get modified
19  a_label = ttk.Label(win, text="A Label")
20  a_label.grid(column=0, row=0)
21
22  # Button Click Event Function
23  def click_me():
24      action.configure(text='*** I have been Clicked! ***')
25      a_label.configure(foreground='red')
26      a_label.configure(text='A Red Label')
27
28  # Adding a Button
29  action = ttk.Button(win, text='Click me!', command=click_me)
30  action.grid(column=1, row=0)
31
32  #====================
33  # Start GUI
34  #====================
35  win.mainloop()
```

以下屏幕截图显示了我们点击按钮之前 GUI 的外观：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_39_0.png)

点击按钮后，标签的颜色发生了变化，按钮的文本也随之改变，如下所示：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_40_0.png)

### 其工作原理…

在第 19 行，我们将标签分配给一个变量，并在第 20 行使用此变量在表单内定位标签。我们需要此变量，以便在 `click_me()` 函数中更改其属性。默认情况下，这是一个模块级变量，因此只要我们在调用它的函数之上声明该变量，我们就可以在函数内部访问它。

第 23 行是事件处理程序，当按钮被点击时会被调用。

在第 29 行，我们创建按钮并将 command 绑定到 `click_me()` 函数。

> GUI 是事件驱动的。点击按钮会产生一个事件。我们使用 `ttk.Button` 控件的 `command` 属性在回调函数中绑定事件发生时要执行的操作。注意我们没有使用括号，只使用了名称 `click_me`。

我们还将标签的文本更改为包含 `red`，因为在印刷版书籍中，否则可能不明显。当你运行代码时，你可以看到颜色确实发生了变化。

第 20 行和第 30 行都使用了网格布局管理器，这将在下一章中讨论。这使标签和按钮对齐。

### 还有更多…

我们将继续向我们的 GUI 添加更多控件，并在本书的其他食谱中利用许多内置属性。

## 文本框控件

在 tkinter 中，典型的单行文本框控件称为 Entry。在本食谱中，我们将向我们的 GUI 添加这样的 Entry 控件。我们将使标签更有用，通过描述 Entry 控件为用户做了什么。

### 准备工作

本食谱基于 创建按钮并更改其文本属性 食谱。

### 如何操作...

查看以下代码：

GUI_textbox_widget.py

```
22 # Modified Button Click Function
23 def click_me():
24     action.configure(text='Hello ' + name.get())
25
26 # Changing our Label
27 ttk.Label(win, text="Enter a name:").grid(column=0, row=0)
28
29 # Adding a Text box Entry widget
30 name = tk.StringVar()
31 name_entered = ttk.Entry(win, width=12, textvariable=name)
32 name_entered.grid(column=0, row=1)
```

现在，我们的 GUI 看起来像这样：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_41_0.png)

输入一些文本并点击按钮后，GUI 发生了以下变化：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_42_0.png)

### 其工作原理...

在第 24 行，我们获取 Entry 控件的值。我们还没有使用 OOP，那么我们怎么能访问一个甚至还没有声明的变量的值呢？

不使用 OOP 类，在 Python 的过程式编码中，我们必须在尝试使用该名称的语句之前物理地放置一个名称。那么为什么这会起作用呢？它确实起作用。

答案是按钮点击事件是一个回调函数，到用户点击按钮时为止，此函数中引用的变量是已知的并且确实存在。

生活很美好。

第 27 行为我们的标签赋予了更有意义的名称；目前，它描述了它下面的文本框。我们将按钮向下移动到标签旁边，以便在视觉上将两者关联起来。我们仍然在使用网格布局管理器，这将在第 2 章《布局管理》中更详细地解释。

第 30 行创建了一个变量 name。此变量绑定到 Entry 控件，在我们的 click_me() 函数中，我们能够通过在此变量上调用 get() 来检索 Entry 控件的值。这工作得很好。

现在我们看到，虽然按钮显示了我们输入的全部文本（以及更多），但文本框 Entry 控件并未扩展。原因是在第 31 行我们将其硬编码为宽度 12。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_43_0.png)

- Python 是一种动态类型语言，会根据赋值推断类型。这意味着如果我们将字符串赋给 `name` 变量，它将是 `string` 类型，如果我们将整数赋给 `name`，其类型将是整数。
- 使用 `tkinter` 时，我们必须在成功使用之前将 `name` 变量声明为 `tk.StringVar()` 类型。原因是 tkinter 不是 Python。我们可以从 Python 使用它，但它不是同一种语言。

## 设置控件焦点并禁用控件

虽然我们的 GUI 正在变得越来越好，但如果 GUI 出现时光标就出现在 Entry 控件中会更方便、更有用。在这里我们学习如何做到这一点。

### 准备工作

本食谱扩展了前一个食谱，文本框控件。

### 如何操作...

Python 真的很棒。我们要做的就是在 GUI 出现时将焦点设置到特定控件上，只需在我们之前创建的 `tkinter` 控件实例上调用 `focus()` 方法即可。在我们当前的 GUI 示例中，我们将 `ttk.Entry` 类实例分配给一个名为 `name_entered` 的变量。现在，我们可以让它获得焦点。

