

# 准备工作

本食谱扩展了之前的食谱，使用不同初始状态创建复选按钮。

# 如何做...

我们在之前的食谱中添加以下代码：

GUI_radiobutton_widget.py

```
74  # Radiobutton Globals
75  COLOR1 = "Blue"
76  COLOR2 = "Gold"
77  COLOR3 = "Red"
78
79  # Radiobutton Callback
80  def radCall():
81      radSel=radVar.get()
82      if  radSel == 1: win.configure(background=COLOR1)
83      elif radSel == 2: win.configure(background=COLOR2)
84      elif radSel == 3: win.configure(background=COLOR3)
85
86  # create three Radiobuttons using one variable
87  radVar = tk.IntVar()
88
89  rad1 = tk.Radiobutton(win, text=COLOR1, variable=radVar, value=1, command=radCall)
90  rad1.grid(column=0, row=5, sticky=tk.W, columnspan=3)
91
92  rad2 = tk.Radiobutton(win, text=COLOR2, variable=radVar, value=2, command=radCall)
93  rad2.grid(column=1, row=5, sticky=tk.W, columnspan=3)
94
95  rad3 = tk.Radiobutton(win, text=COLOR3, variable=radVar, value=3, command=radCall)
96  rad3.grid(column=2, row=5, sticky=tk.W, columnspan=3)
97
98  name_entered.focus()      # Place cursor into name Entry
99  ====================
100 # Start GUI
101 ====================
102 win.mainloop()
```

运行此代码并选择名为 Gold 的单选按钮会创建以下窗口：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_52_0.png)

# 它是如何工作的...

在第 75-77 行，我们创建了一些模块级全局变量，我们将在创建每个单选按钮以及创建更改主窗体背景颜色的操作的回调函数中使用它们（使用实例变量 win）。

我们使用全局变量是为了使更改代码更容易。通过将颜色名称分配给变量并在多个地方使用该变量，我们可以轻松尝试不同的颜色。无需对硬编码字符串进行全局搜索和替换（容易出错），我们只需更改一行代码，其余部分就会正常工作。这被称为 DRY 原则，即 Don't Repeat Yourself。这是我们在本书后面的食谱中会使用的 OOP 概念。

我们分配给变量的颜色名称（COLOR1、COLOR2 ...）是 tkinter 关键字（从技术上讲，它们是符号名称）。如果我们使用不是 tkinter 颜色关键字的名称，则代码将无法工作。

第 80 行是根据用户选择更改主窗体（win）背景的回调函数。

在第 87 行我们创建一个 tk.IntVar 变量。重要的是，我们只创建一个变量供所有三个单选按钮使用。正如屏幕截图所示，无论我们选择哪个单选按钮，其他单选按钮都会自动为我们取消选择。

第 89 到 96 行创建三个单选按钮，将它们分配给主窗体，并传入将在创建更改主窗口背景的操作的回调函数中使用的变量。

虽然这是第一个更改小部件颜色的食谱，但说实话，它看起来有点难看。本书中很大一部分后续食谱解释了如何让我们的 GUI 看起来真正令人惊艳。

# 还有更多...

以下是可在官方 tcl 手册页 http://www.tcl.tk/man/tcl8.5/TkCmd/colors.htm 上查找的可用符号颜色名称的小样本。

| Name | Red | Green | Blue |
|---|---|---|---|
| alice blue | 240 | 248 | 255 |
| AliceBlue | 240 | 248 | 255 |
| Blue | 0 | 0 | 255 |
| Gold | 255 | 215 | 0 |
| Red | 255 | 0 | 0 |

其中一些名称创建相同的颜色，因此 alice blue 创建的颜色与 AliceBlue 相同。在本食谱中，我们使用了符号名称 Blue、Gold 和 Red。

# 使用滚动文本小部件

ScrolledText 小部件比简单的 Entry 小部件大得多，并且跨越多行。它们是像记事本一样的控件，会自动换行，并在文本大于 ScrolledText 小部件高度时自动启用垂直滚动条。

# 准备工作

本食谱扩展了之前的食谱，使用单选按钮小部件。您可以从 https://github.com/PacktPublishing/Python-GUI-Programming-Cookbook-Second-Edition/ 下载本书每个章节的代码。

# 如何做...

通过添加以下代码行，我们创建了一个 ScrolledText 小部件：

GUI_scrolledtext_widget.py

```
6 #====================
7 # imports
8 #====================
9 import tkinter as tk
10 from tkinter import ttk
11 from tkinter import scrolledtext

99 # Using a scrolled Text control
100 scrol_w = 30
101 scrol_h = 3
102 scr = scrolledtext.ScrolledText(win, width=scrol_w, height=scrol_h, wrap=tk.WORD)
103 scr.grid(column=0, columnspan=3)
104
105 name_entered.focus()      # Place cursor into name Entry
106 #====================
107 # Start GUI
108 #====================
109 win.mainloop()
```

我们实际上可以输入我们的控件，如果我们输入足够的单词，行会自动换行：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_54_0.png)

一旦我们输入的单词超过控件可以显示的高度，垂直滚动条就会启用。所有这些开箱即用，无需我们编写更多代码即可实现：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_55_0.png)

# 它是如何工作的...

在第 11 行，我们导入包含 ScrolledText 小部件类的模块。将此添加到模块顶部，位于另外两个 import 语句下方。

第 100 行和第 101 行定义了我们即将创建的 ScrolledText 小部件的宽度和高度。这些是我们在第 102 行传递给 ScrolledText 小部件构造函数的硬编码值。

这些值是通过实验得出的魔数，可以很好地工作。你可以尝试将 scol_w 从 30 改为 50 并观察效果！

在第 102 行，我们还通过传入 wrap=tk.WORD 在小部件上设置了一个属性。

通过将 wrap 属性设置为 tk.WORD，我们告诉 ScrolledText 小部件按单词换行，这样我们就不会在单词中间换行。默认选项是 tk.CHAR，它会在单词中间时换行任何字符。

第二个屏幕截图显示垂直滚动条向下移动，因为我们正在阅读更长的文本，该文本完全不适合我们创建的 ScrolledText 控件的 x、y 尺寸。

将 ScrolledText 小部件的 grid 小部件的 columnspan 属性设置为 3 使此小部件跨越全部三个列。如果我们不设置此属性，我们的 ScrolledText 小部件将仅位于第一列，这不是我们想要的。

# 在循环中添加多个小部件

到目前为止，我们已经通过基本上复制和粘贴相同的代码然后修改变化（例如列号）创建了多个相同类型的小部件（例如 Radiobutton）。在本食谱中，我们开始重构代码以使其不那么冗余。

# 准备工作

我们正在重构上一食谱 Using scrolled text widgets 的某些部分代码，因此您需要该代码才能将本食谱应用到其中。

# 如何做...

以下是我们重构代码的方式：

GUI_adding_widgets_in_loop.py

```
76 # First, we change our Radiobutton global variables into a list
77 colors = ["Blue", "Gold", "Red"]
78 
79 # We have also changed the callback function to be zero-based, using the list
80 # instead of module-level global variables
81 # Radiobutton Callback
82 def radCall():
83     radSel=radVar.get()
84     if  radSel == 0: win.configure(background=colors[0])  # now zero-based
85     elif radSel == 1: win.configure(background=colors[1])  # and using list
86     elif radSel == 2: win.configure(background=colors[2])
87 
88 # create three Radiobuttons using one variable
89 radVar = tk.IntVar()
90 
91 # Next we are selecting a non-existing index value for radVar
92 radVar.set(99)
93 
94 # Now we are creating all three Radiobutton widgets within one loop
95 for col in range(3):
96     curRad = tk.Radiobutton(win, text=colors[col], variable=radVar,
97                             value=col, command=radCall)
98     curRad.grid(column=col, row=5, sticky=tk.W)
99
```

